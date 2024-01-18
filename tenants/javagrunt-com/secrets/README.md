## An Example of how to create secrets with SOPS and PGP

```bash
flux create kustomization secrets \
--source=flux-system \
--path=./tenants/javagrunt-com/secrets \
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
sops --encrypt --in-place yugabyte-cloud.yaml
```