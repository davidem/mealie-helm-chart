# mealie-helm-chart
---

A simple helm chart for the deployment of [mealie](https://github.com/mealie-recipes/mealie) into a Kubernetes cluster.

## Installation

1. [Optional] Setup persistent storage
2. [Optional] Create a `my-values.yaml` to override any default values in [values.yaml](./mealie/values.yaml)
3. Deploy helm chart: `helm upgrade -i mealie -f .\my-values.yaml .\mealie\`

## Handy Commands

| Command                                                           | Description                            | 
|-------------------------------------------------------------------|----------------------------------------| 
| `helm upgrade -i <deployment_name> -f .\my-values.yaml .\mealie\` | Deploy/upgrade the Mealie helm chart   |
| `helm delete test-mealie`                                         | Remove the helm chart from the cluster |