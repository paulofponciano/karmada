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

## Kubectl karmada

```sh
curl -s https://raw.githubusercontent.com/karmada-io/karmada/master/hack/install-cli.sh | bash -s kubectl-karmada
```

## karmada-kubeconfig

> [!NOTE]
> Gerado durante o deploy com helm. Necessário para acessar o api-server do Karmada. 

```sh
kubectl get secrets -n karmada-system | grep karmada-kubeconfig
```

## Context - Join member

```sh
aws eks update-kubeconfig --region REGION --name CLUSTER_NAME --kubeconfig $HOME/.kube/CLUSTER_NAME.config
```

## Join cluster member (Push)

```sh
kubectl karmada --kubeconfig $HOME/.kube/karmada.config  join CLUSTER_NAME --cluster-kubeconfig=$HOME/.kube/CLUSTER_NAME.config
```
```sh
kubectl --kubeconfig $HOME/.kube/karmada.config get clusters
```

## ArgoCD

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
```sh
argocd login argocd-server.argocd.svc.cluster.local:80 --username admin
```
```sh
argocd cluster add karmada-apiserver --name karmada.config
```

---

Official repo:
- https://github.com/karmada-io/karmada/tree/master/charts/karmada