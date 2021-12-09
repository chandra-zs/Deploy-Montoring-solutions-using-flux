# Deploy-Montoring-solutions-using-flux

# Install K3s Cluster

```
curl -sfL https://get.k3s.io | sh -
```
# Install Fluxcli

```
curl -s https://fluxcd.io/install.sh | sudo bash
```
# Install Flux System On Cluster

```
flux bootstrap github \
  --owner=chandra-zs \
  --repository=flux2-1 \
  --branch=main \
  --path=./manifests/monitoring
```
# Add Source 

```
flux create source git monitoring \
  --interval=30m \
  --url=https://github.com/chandra-zs/flux2-1.git \
  --branch=main
```
# Create Monitoring-Stack Kustomization

```
flux create kustomization monitoring-stack \
  --interval=1h \
  --prune=true \
  --source=monitoring \
  --path="./manifests/monitoring/kube-prometheus-stack"
```
# Create Monitoring-Config Kustomization

```
flux create kustomization monitoring-config \
  --interval=1h \
  --prune=true \
  --source=monitoring \
  --path="./manifests/monitoring/monitoring-config"
```
