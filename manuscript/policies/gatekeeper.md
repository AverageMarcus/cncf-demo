# Policies with Open Policy Agent (OPA) and Gatekeeper

TODO: Intro

## Setup

```bash
export GITOPS_APP=$(yq ".gitOps.app" settings.yaml)
```

## Do

```bash
# Gatekeeper causes issues with Argo CD (not sure about Flux),
#   so we'll install it manually.

helm repo add gatekeeper \
    https://open-policy-agent.github.io/gatekeeper/charts

helm repo update

helm upgrade --install gatekeeper gatekeeper/gatekeeper \
    --namespace gatekeeper-system --create-namespace

cat policies/gatekeeper-templates.yaml

cp policies/gatekeeper-templates.yaml infra/policy-templates.yaml

git add .

git commit -m "Policy templates"

git push

kubectl get constrainttemplates

# Wait until the contraints templates are created

cat policies/gatekeeper-constraints.yaml

cp policies/gatekeeper-constraints.yaml \
    infra/policy-constraints.yaml

git add .

git commit -m "Policy templates"

git push

kubectl get constraints

export POLICY_KIND=constraints

yq --inplace ".policies.type = \"gatekeeper\"" settings.yaml

yq --inplace ".policies.kind = \"$POLICY_KIND\"" settings.yaml
```

## How Did You Define Your App?

* [Helm](helm.md)
* [Kustomize](kustomize.md)
* [Carvel ytt](carvel.md)
* [cdk8s](cdk8s.md)
