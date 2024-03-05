# flux-control

## juice-docker-desktop

Bootstrap the cluster with flux:
```bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=flux-control \
  --branch=main \
  --path=./clusters/$(kubectx -c)
```

Add the private key to the cluster for SOPS:
```bash
gpg --export-secret-keys --armor "${ARM64_KEY_FP}" |
kubectl create secret generic sops-gpg \
--namespace=flux-system \
--from-file=sops.asc=/dev/stdin
```

## k3s setup

```bash
k3sup install --ip  100.93.234.120 --user dashaun --local-path ~/.kube/config --merge --k3s-extra-args '--disable traefik' --context arm64

k3sup join --ip $AGENT_IP --server-ip $SERVER_IP --user dashaun
```

## k3d setup

```bash
k3d registry create juice --port 0.0.0.0:5000
```

```bash
#delete cluster
k3d cluster delete juice

#create cluster
k3d cluster create juice --k3s-arg "--disable=traefik@server:0"

#create cluster using local registry
k3d cluster create juice --registry-use k3d-juice:5000 --k3s-arg "--disable=traefik@server:0"
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
## Import Cluster Public Key

```bash
gpg --import ./clusters/$(kubectx -c)/.sops.pub.asc
```

## Configure directory for encryption

```bash
cat <<EOF > ./clusters/$(kubectx -c)/secrets/.sops.yaml
creation_rules:
  - path_regex: .*.yaml
    encrypted_regex: ^(data|stringData)$
    pgp: ${K3D_JUICE_KEY_FP}
EOF
```
