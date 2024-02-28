# kubecon2024

**cnvrg MLOps on Intel Cloud with Intel Kubernetes Service (IKS)**

## Deployment

### Istio Installation

```bash
# Add Istio Helm repository
helm repo add istio https://istio-release.storage.googleapis.com/charts

# Update Helm repositories
helm repo update

# Create namespace for Istio
kubectl create namespace istio-system

# Install Istio base components
helm install istio-base istio/base --create-namespace -n istio-system

# Install Istio control plane
helm install istiod istio/istiod --create-namespace -n istio-system

# Install Istio gateway with external IP configurations
helm install istio-gateway istio/gateway \
  --set service.externalIPs[0]=100.82.189.186 \
  --set service.externalIPs[1]=100.82.189.93 \
  --create-namespace -n istio-system


# Install Longhorn with specified settings
helm install longhorn longhorn/longhorn \
 --namespace longhorn-system \
 --create-namespace \
 --version 1.6.0 \
 --set defaultSettings.defaultDataPath=/mnt/longhorn


# Install cnvrg MLOps IDC with specified configurations
helm install cnvrg cnvrg-mlops-idc \
 --create-namespace -n cnvrg \
 --set clusterDomain=cnvrg-on-idc.azops.cnvrg.io \
 --set controlPlane.image=cnvrg/app:v4.7.52-DEV-15824-cnvrg-agnostic-infra-45 \
 --set registry.user="cnvrghelm" \
 --set registry.password="cabbecc7-4330-47b6-85c6-ea0ad5019cfa" \
 --set networking.ingress.istioIngressSelectorKey="istio" \
 --set networking.ingress.istioIngressSelectorValue="gateway"


These comments provide a brief explanation for each command, enhancing the clarity and understanding of the deployment process.
