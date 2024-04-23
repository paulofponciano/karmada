![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)

# [Karmada](https://karmada.io/) | Open, Multi-Cloud, Multi-Cluster Kubernetes Orchestration

<p align="center"><img src="https://github.com/karmada-io/karmada/blob/master/docs/images/Karmada-logo-horizontal-color.png" width="250" alt="karmada"></p>

## Deploy with Helm (control plane)

```sh
helm repo add karmada-charts https://raw.githubusercontent.com/karmada-io/karmada/master/charts
helm repo update
```
```sh
helm --namespace karmada-system upgrade -i karmada karmada-charts/karmada --version=1.9.0 --create-namespace --values karmada-values.yaml
```

## CLI Tools

- Karmadactl:

```sh
curl -s https://raw.githubusercontent.com/karmada-io/karmada/master/hack/install-cli.sh | bash
```

- Kubectl karmada:

```sh
curl -s https://raw.githubusercontent.com/karmada-io/karmada/master/hack/install-cli.sh | bash -s kubectl-karmada
```

## karmada-kubeconfig

> [!NOTE]
> Generated during deployment with helm. Required to access the Karmada api-server. 

```sh
kubectl get secrets -n karmada-system | grep karmada-kubeconfig
```

## Join cluster member (Push)

```sh
aws eks update-kubeconfig --region REGION --name CLUSTER_NAME --kubeconfig $HOME/.kube/CLUSTER_NAME.config
```
```sh
kubectl karmada --kubeconfig $HOME/.kube/karmada.config  join CLUSTER_NAME --cluster-kubeconfig=$HOME/.kube/CLUSTER_NAME.config
```
```sh
kubectl --kubeconfig $HOME/.kube/karmada.config get clusters
```

## Register cluster member (Pull)

- Perform in control plane karmada:

```sh
karmadactl token create --print-register-command --kubeconfig $HOME/.kube/karmada.config
```

- Perform in member cluster:

```sh
$ karmadactl register 10.10.x.x:32443 --token t2jgtm.9nybj0526mjw1jbf --discovery-token-ca-cert-hash sha256:f5a5a43869bb44577dba582e794c3e3750f2050d62f1b1dc80fd3d6a371b6ed4
```

## ArgoCD

```sh
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
```sh
argocd login argocd-server.argocd.svc.cluster.local:80 --username admin
```
```sh
argocd cluster add karmada-apiserver --name karmada-controlplane
```
```sh
kubectl apply -f karmada-argo-app.yaml
``` 

---

Official repo:
- https://github.com/karmada-io/karmada/tree/master/charts/karmada

Official docs:
- https://karmada.io/docs/