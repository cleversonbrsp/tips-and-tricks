# Troubleshooting: "Failed to scrape node" Error in Minikube

## Issue

The following error occurs when attempting to scrape metrics from a Minikube node:
```
E1031 15:23:24.320462       1 scraper.go:149] "Failed to scrape node" err="Get \"https://192.168.49.2:10250/metrics/resource\": tls: failed to verify certificate: x509: cannot validate certificate for 192.168.49.2 because it doesn't contain any IP SANs" node="minikube"
```

## Cause

This error is due to the TLS certificate for the Minikube node missing the required Subject Alternative Name (SAN) with the IP address, preventing Prometheus (or other monitoring services) from verifying the certificate and collecting metrics.

## Solutions

### Solution 1: Disable Certificate Verification (Not Recommended for Production)

You can disable TLS certificate verification for testing purposes. This approach lowers security and should not be used in production environments.

1. If using Prometheus Operator, add the `insecure_skip_verify: true` setting in the scrape configuration. Here’s an example:
    ```yaml
    tls_config:
      insecure_skip_verify: true
    ```

2. This setting allows Prometheus to bypass certificate validation, enabling it to scrape metrics from the node.

---

### Solution 2: Regenerate Kubelet Certificate with IP SAN

To include the correct IP in the node’s TLS certificate, you’ll need to recreate the Minikube cluster with a specific configuration:

1. Delete the existing Minikube cluster:
    ```bash
    minikube delete
    ```

2. Start Minikube with the `--extra-config` option, specifying the IP SAN:
    ```bash
    minikube start --extra-config=kubelet.node-ip=192.168.49.2
    ```
    - This command regenerates the certificate for the `kubelet` service, including the specified IP address as a SAN.

---

### Solution 3: Use Minikube Tunnel

Running a `minikube tunnel` helps expose Minikube services and ensures the node IP can be reached, potentially bypassing certificate verification issues.

1. Run the following command in a separate terminal:
    ```bash
    minikube tunnel
    ```

2. This command may make the node IP accessible and allow metrics scraping without modifying certificates.

---

## Conclusion

These solutions can help you resolve the "Failed to scrape node" error due to TLS certificate issues in Minikube. Select the solution best suited to your environment, with careful consideration of security implications, especially in production setups.
```