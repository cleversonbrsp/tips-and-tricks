Sure! Here's a markdown document for installing the Metrics Server in Minikube.
# Metrics Server Installation in Minikube

## Overview
The Metrics Server is an essential component in Kubernetes that collects resource metrics from Kubelets and exposes them via the Kubernetes API. This guide will walk you through the steps to install the Metrics Server in a Minikube cluster.

## Installation Steps

### 1. Enable the Metrics Server Add-on
Minikube provides a convenient command to enable the Metrics Server as an add-on. Run the following command in your terminal:

```bash
minikube addons enable metrics-server
```

This command will automatically install and configure the Metrics Server in your Minikube cluster.

### 2. Verify the Metrics Server Status
After enabling the Metrics Server, you can check its status with the following command:

```bash
kubectl get deployment metrics-server -n kube-system
```

Ensure that the output indicates the Metrics Server has been successfully started.

### 3. Test the Metrics Server
Once the Metrics Server is running, you can check the metrics for nodes and pods using the following commands:

- To view node metrics:

  ```bash
  kubectl top nodes
  ```

- To view pod metrics across all namespaces:

  ```bash
  kubectl top pods --all-namespaces
  ```

### 4. Troubleshooting
If the Metrics Server is not collecting metrics correctly, you can check its logs for errors with the following command:

```bash
kubectl logs -n kube-system deployment/metrics-server
```

## Conclusion
By following these steps, you should have the Metrics Server installed and operational in your Minikube cluster. You can now monitor resource usage effectively.