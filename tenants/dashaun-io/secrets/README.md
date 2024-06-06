## An Example of how to create secrets with SOPS and PGP

[Guide](https://fluxcd.io/flux/guides/mozilla-sops/)

Create a kustomization for reconciling the secrets on the tenant:

```bash
flux create kustomization secrets \
--source=flux-system \
--path=./tenants/dashaun-io/secrets \
--prune=true \
--interval=5m \
--decryption-provider=sops \
--decryption-secret=sops-gpg
```

```bash
kubectl -n javagrunt-com create secret generic yugabyte-cloud \
--from-literal=DATASOURCE_URL=$(bw get item yugabyte-cloud-free | jq -r .login.username) \
--from-literal=DATASOURCE_USERNAME=admin \
--from-literal=DATASOURCE_PASSWORD=$(bw get item yugabyte-cloud-free | jq -r .login.password) \
--dry-run=client \
-o yaml > yugabyte-cloud.yaml
```

```bash
kubectl -n javagrunt-com create secret generic tunnel-credentials \
--from-literal=credentials.json=$(bw get item javagrunt-com-tunnel-secret | jq -r .login.password) \
--dry-run=client\
-o yaml > cloudflared-tunnel.yaml
```

```bash
sops --encrypt --in-place yugabyte-cloud.yaml
```