# Installing ClusterIssuer in Minikube

## Overview
A `ClusterIssuer` is a resource in Kubernetes that manages the issuance of certificates across the cluster. It is commonly used with cert-manager to automate the process of obtaining SSL/TLS certificates. This guide will walk you through the steps to install cert-manager and create a `ClusterIssuer` in Minikube.

## Prerequisites
- Ensure you have a running Minikube cluster.
- Make sure `kubectl` is installed and configured to interact with your Minikube cluster.

## Installation Steps

### 1. Install cert-manager
You can install cert-manager using Helm. If you haven't installed Helm yet, you can do so by following the [official Helm installation guide](https://helm.sh/docs/intro/install/).

Add the Jetstack Helm repository, which contains cert-manager:

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

Now, install cert-manager with the following command:

```bash
kubectl create namespace cert-manager
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.12.0 --set installCRDs=true
```

### 2. Verify cert-manager Installation
After the installation, verify that cert-manager components are running:

```bash
kubectl get pods --namespace cert-manager
```

You should see pods for cert-manager, cert-manager-cainjector, and cert-manager-webhook in the running state.

### 3. Create a ClusterIssuer
Now, you can create a `ClusterIssuer` resource. Below is an example of a `ClusterIssuer` configuration that uses Let's Encrypt as the certificate authority.

Create a YAML file named `clusterissuer.yaml` with the following content:

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-clusterissuer
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: your-email@example.com
    # Set to true to enable the HTTP-01 challenge provider
    privateKeySecretRef:
      name: letsencrypt-account-key
    solvers:
    - http01:
        ingress:
          class: nginx
```

Replace `your-email@example.com` with your email address.

### 4. Apply the ClusterIssuer
Apply the `ClusterIssuer` configuration using `kubectl`:

```bash
kubectl apply -f clusterissuer.yaml
```

### 5. Verify the ClusterIssuer
You can check if the `ClusterIssuer` has been created successfully:

```bash
kubectl get clusterissuer
```

You should see your `letsencrypt-clusterissuer` listed.

## Conclusion
You have now installed cert-manager and created a `ClusterIssuer` in your Minikube cluster. You can use this `ClusterIssuer` to request certificates for your applications automatically.