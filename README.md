# flux-control

```bash
kustomize build ./env/juice-docker-desktop | envsubst | kubectl apply -f -
```

```bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=flux-control \
  --branch=main \
  --path=./clusters/juice-docker-desktop
```