# Develop The Application With DevSpace And cdk8s

TODO: Intro

## Setup

* Install the [`devspace` CLI](https://devspace.sh/docs/getting-started/installation)

```bash
chmod +x manuscript/develop/devspace-cdk8s.sh

./manuscript/develop/devspace-cdk8s.sh
```

## Do

```bash
cat devspace.yaml

devspace dev --namespace dev

go run .

# Open a second terminal and navigate to the project root

# In the second terminal
# Open `root.go` and modify the output however you like

# In the first terminal
# `ctrl+c`

# In the first terminal
go run .

# Refresh the browser tab

# In the first terminal
# `ctrl+c`

# In the first terminal
exit

# In the second terminal
exit

devspace purge --namespace dev
```

## Continue The Adventure

The Adventure will continue soon...

[Destroy Everything](../destroy/dev.md)
