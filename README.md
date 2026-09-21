# KubernetesSubmissions-config

Kubernetes configuration for the project of the DevOps with Kubernetes course. The application code lives in [KubernetesSubmissions](https://github.com/DavidBotero/KubernetesSubmissions), and ArgoCD syncs this repository to the cluster.

## Layout

| Path | What it holds |
|---|---|
| `base` | The manifests shared by every environment, with placeholder image names. |
| `overlays/staging` | Staging: namespace `staging`, no database backup and a broadcaster that only logs. |
| `overlays/production` | Production: namespace `production`. |
| `applications` | The ArgoCD Applications, one per environment. |

## How it is updated

The workflows in the code repository build the images and commit the new image tags here. A commit to `main` there updates `overlays/staging` and a tagged commit updates `overlays/production`. ArgoCD notices the commit and rolls the environment out, so nobody applies anything by hand.

Secrets are not stored here. They are created in the cluster outside of ArgoCD.

## First setup

```
kubectl apply -f applications/staging.yaml -f applications/production.yaml
```
