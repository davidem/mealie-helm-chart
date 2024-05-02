# mealie-helm-chart
---

A simple helm chart for the deployment of [mealie](https://github.com/mealie-recipes/mealie) into a Kubernetes cluster.

## Installation

### From a clone of the repo

1. [Optional] Setup persistent storage
2. [Optional] Create a `my-values.yaml` to override any default values in [values.yaml](./mealie/values.yaml)
3. Deploy helm chart: `helm upgrade -i mealie -f .\my-values.yaml .\mealie\`

### Using ArgoCD

```yaml
#
# Description: This is the Argo CD Application manifest for the Mealie application (helm chart)
#
# To apply run:
#   kubectl.exe apply -f .\argocd-mealie-app.yaml
#
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: mealie
  namespace: argocd
spec:
  destination:
    namespace: test
    server: 'https://kubernetes.default.svc'
  sources:
    # Helm chart
    - repoURL: 'https://github.com/ben-kemister/mealie-helm-chart.git'
      targetRevision: main
      path: mealie
      helm:
        valueFiles:
          # This path is relative to the root of the 'my-values' source/repo
          - '$my-values/my-values.yaml'
    # Deployment values
    - repoURL: 'https://<YOUR_DEPLOYMENT_GIT_REPO>'
      targetRevision: main
      ref: my-values
  project: default
  syncPolicy:
    # Automatically sync cluster when git resources change
    automated:
      # Remove kubernetes objects/resources that are no longer present
      prune: true
    syncOptions:
      - CreateNamespace=true
  # Reduce application's revision history to reduce storage used
  revisionHistoryLimit: 3
```

## Viewing generated yaml files

```powershell
helm install --dry-run test-mealie .\mealie\ > test.yaml
```

## Handy Commands

| Command                                                           | Description                            | 
|-------------------------------------------------------------------|----------------------------------------| 
| `helm upgrade -i <deployment_name> -f .\my-values.yaml .\mealie\` | Deploy/upgrade the Mealie helm chart   |
| `helm delete test-mealie`                                         | Remove the helm chart from the cluster |