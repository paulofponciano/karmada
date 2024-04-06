# [Karmada](https://karmada.io/) | Open, Multi-Cloud, Multi-Cluster Kubernetes Orchestration

<p align="center"><img src="https://github.com/karmada-io/karmada/blob/master/docs/images/Karmada-logo-horizontal-color.png" width="250" alt="karmada"></p>

## Deploy with Helm

```sh
helm repo add karmada-charts https://raw.githubusercontent.com/karmada-io/karmada/master/charts
helm repo update
```
```sh
helm --namespace karmada-system upgrade -i karmada karmada-charts/karmada --version=1.9.0 --create-namespace --values karmada-values.yaml
```

Official repo:
- https://github.com/karmada-io/karmada/tree/master/charts/karmada