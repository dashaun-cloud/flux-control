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
kubectl -n dashaun-io create secret generic tunnel-credentials \
--from-literal=credentials.json=$(bw get item dashaun-io-tunnel-secret | jq -r .login.password) \
--dry-run=client \
-o yaml > cloudflared-tunnel.yaml
```

```bash
sops --encrypt --in-place cloudflared-tunnel.yaml
```