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

## k3d setup

```bash
k3d cluster create juice --k3s-arg "--disable=traefik@server:0"
```