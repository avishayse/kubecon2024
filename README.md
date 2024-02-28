# Deploying cnvrg Operator on Intel Developer Cloud (IDC)

This guide provides step-by-step instructions for deploying the cnvrg operator on Intel Developer Cloud (IDC). Follow these simple steps to get started.

## Prerequisites

- Access to [Intel Developer Cloud (IDC) console](https://intel.com/idc).
- Permissions to spin up resources and manage Kubernetes services.

## Deployment Steps

1. **Go to Intel Developer Cloud Console:**
   - Open your web browser and navigate to the [Intel Developer Cloud (IDC) console](https://console.cloud.intel.com/)). console.


2. **Spin Up Resources:**
   - In the IDC console, navigate to the Kubernetes service.
   - Launch a control plane cluster by following these steps:
![cnvrg Logo](launch_k8s_idc.png)  
     1. Provide a unique name for your cluster.
     2. Choose the desired Kubernetes version.
     3. Proceed with the default or customize advanced settings as needed.
   - Click on "Launch" to spin up the cluster.

3. **Deploy cnvrg Operator:**
   - Once the cluster is up and running, you can proceed to deploy the cnvrg operator.
   - Follow the [official documentation](https://docs.cnvrg.io/) or consult the cnvrg support team for instructions on deploying the operator on a Kubernetes cluster.

![cnvrg Logo](kially1.png)

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

```

### Longhorn Installation
You can install Longhorn using Helm by following the steps outlined in the Longhorn [documentation.](https://longhorn.io/docs/1.6.0/deploy/install/install-with-helm/)

```bash
# Install Longhorn with specified settings
helm install longhorn longhorn/longhorn \
 --namespace longhorn-system \
 --create-namespace \
 --version 1.6.0 \
 --set defaultSettings.defaultDataPath=/mnt/longhorn
```

### cnvrg MLOps Installation
```bash
# Install cnvrg MLOps IDC with specified configurations
helm install cnvrg cnvrg-mlops-idc \
 --create-namespace -n cnvrg \
 --set clusterDomain=cnvrg-on-idc.azops.cnvrg.io \
 --set controlPlane.image=cnvrg/app:v4.7.52-DEV-15824-cnvrg-agnostic-infra-45 \
 --set registry.user="cnvrghelm" \
 --set registry.password="cabbecc7-4330-47b6-85c6-ea0ad5019cfa" \
 --set networking.ingress.istioIngressSelectorKey="istio" \
 --set networking.ingress.istioIngressSelectorValue="gateway"

# These comments provide a brief explanation for each command, enhancing the clarity and understanding of the deployment process.


