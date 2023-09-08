# Policies With Helm

TODO: Intro

## Setup

* Install `gum` by following the instructions in https://github.com/charmbracelet/gum#installation.
* Watch https://youtu.be/U8zCHA-9VLA if you are not familiar with Charm Gum.

```bash
chmod +x manuscript/policies/helm.sh

./manuscript/policies/helm.sh

source .env
```

## Do

```bash
# TODO: kapp-controller

cp $GITOPS_APP/cncf-demo-helm.yaml apps/cncf-demo.yaml

git add .

git commit -m "CNCF Demo"

git push

kubectl --namespace production rollout status \
    --watch statefulset cncf-demo-controller

# Repeat the previous command if the output claims that it was
#   `not found` since that means that Argo CD or Flux did not
#   yet synchronize the resources.

kubectl --namespace production get deployments

# The Deployment was NOT created

# If Gatekeeper
export POLICY_KIND=deploymentreplicas

kubectl describe $POLICY_KIND deploymentproduction

# Gatekeeper does not show violations, but it does enforce them.

cat apps/cncf-demo.yaml

# If Argo CD
yq --inplace ".spec.source.helm.parameters[9].value = \"3\"" \
    apps/cncf-demo.yaml

# If Flux
yq --inplace ".spec.values.replicas = 3" apps/cncf-demo.yaml

git add .

git commit -m "CNCF Demo scaled"

git push

kubectl --namespace production get deployments

# Wait until the Deployment is created

kubectl --namespace production get pods

# Pods are not be running since the database was not created and,
#   with it, the Secret with the authentication was not created
#   either, hence the Pods that require the Secret are not
#   starting).

kubectl --namespace production get sqlclaims

# The SqlClaim was NOT created

# If Gatekeeper
export POLICY_KIND=dbsize

kubectl describe $POLICY_KIND dbcluster

kubectl describe $POLICY_KIND dbproduction

# Gatekeeper (still) does not show violations, but it does
#   enforce them.

cat apps/cncf-demo.yaml

# If Argo CD
yq --inplace \
    ".spec.source.helm.parameters[10].value = \"medium\"" \
    apps/cncf-demo.yaml

# If Flux
yq --inplace ".spec.values.db.size = small" apps/cncf-demo.yaml

git add .

git commit -m "DB resize"

git push

kubectl --namespace production get sqlclaims

# Wait until the SqlClaim is created

kubectl --namespace production wait sqlclaim cncf-demo \
    --for=condition=ready --timeout=15m
```

## Continue The Adventure

* [Runtime Policies](../runtime-policies/README.md)