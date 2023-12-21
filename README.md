# flux-control

## juice-docker-desktop

```bash
kustomize build ./env/dashaun-dev-tunnel | envsubst | kubectl apply -f -
```

```bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=flux-control \
  --branch=main \
  --path=./clusters/$(kubectx -c)
```

## k3s setup

```bash
k3sup install --ip 100.94.235.91 --user dashaun --local-path ~/.kube/config --merge --k3s-extra-args '--disable traefik' --context nucs
```

## k3d setup

```bash
k3d cluster create juice --k3s-arg "--disable=traefik@server:0"
```

## Cleanup

On the control node:
```bash
 sudo /usr/local/bin/k3s-uninstall.sh
```
> Note: This will remove all k3s data, and will not be recoverable.

On the node with the kubeconfig:
```bash
kubectl config delete-user nucs
kubectl config delete-context nucs
kubectl config delete-cluster nucs
```
