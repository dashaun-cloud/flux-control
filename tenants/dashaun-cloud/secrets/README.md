## An Example of how to create secrets with SOPS and PGP

[Guide](https://fluxcd.io/flux/guides/mozilla-sops/)

```bash
flux create kustomization dashaun-cloud-secrets \
--source=flux-system \
--path=./tenants/dashaun-cloud/secrets \
--prune=true \
--interval=5m \
--decryption-provider=sops \
--decryption-secret=sops-gpg
```

```bash
kubectl -n dashaun-cloud create secret generic yugabyte-cloud \
--from-literal=DATASOURCE_URL=$(bw get item yugabyte-cloud-free | jq -r .login.username) \
--from-literal=DATASOURCE_USERNAME=admin \
--from-literal=DATASOURCE_PASSWORD=$(bw get item yugabyte-cloud-free | jq -r .login.password) \
--dry-run=client \
-o yaml > yugabyte-cloud.yaml
```

```bash
kubectl -n dashaun-cloud create secret generic tunnel-credentials \
--from-literal=credentials.json=$(bw get item dashaun-cloud-tunnel-secret | jq -r .login.password) \
--dry-run=client\
-o yaml > cloudflared-tunnel.yaml
```

```bash
sops --encrypt --in-place yugabyte-cloud.yaml
```