# Setting Up RabbitMQ on Kubernetes in KillerCoda

This guide walks you through the process of installing RabbitMQ in a Kubernetes cluster on *KillerCoda* and accessing its web management interface.

## Steps to Set Up RabbitMQ on Kubernetes

### 1. **Create a New Kubernetes Cluster in KillerCoda**
   - Log into your *KillerCoda* environment.
   - Follow the instructions to create a new Kubernetes cluster or use an existing one.
   - If you don't have a cluster, you can easily create one by following the steps provided in the *KillerCoda* interface.

### 2. **Install RabbitMQ on Kubernetes**
   To install RabbitMQ on your Kubernetes cluster, you can use the official Helm chart. Follow these steps:

   - First, ensure you have `helm` installed in your *KillerCoda* environment. You can check this by running:

     ```bash
     helm version
     ```

   - Add the RabbitMQ chart repository:

     ```bash
     helm repo add bitnami https://charts.bitnami.com/bitnami
     helm repo update
     ```

   - Install RabbitMQ using Helm:

     ```bash
     helm install my-rabbitmq bitnami/rabbitmq
     ```

   This will deploy RabbitMQ in your Kubernetes cluster under the name `my-rabbitmq`.

### 3. **Check RabbitMQ Pod Status**
   Ensure that RabbitMQ is up and running by checking the status of the pods:

   ```bash
   kubectl get pods
   ```

   You should see the `my-rabbitmq` pod in a `Running` state. If it's not running, you can check the logs for any errors:

   ```bash
   kubectl logs <my-rabbitmq-pod-name>
   ```

### 4. **Forward RabbitMQ Web Port**
   The RabbitMQ management interface is exposed on port `15672`. To access it from the *KillerCoda* environment, forward the port using `kubectl port-forward`:

   ```bash
   kubectl port-forward svc/my-rabbitmq 15672:15672 --address 0.0.0.0
   ```

   > **Tip:** The `--address 0.0.0.0` option is required in *KillerCoda* to allow access from the integrated environment and external interfaces.

### 5. **Access RabbitMQ Web Interface**
   Now that the port is forwarded, you can access the RabbitMQ management interface via the integrated browser in *KillerCoda*.

   - Open the internal browser in your *KillerCoda* environment.
   - Navigate to the following URL:

     ```bash
     http://localhost:15672
     ```

### 6. **Login to RabbitMQ Web Interface**
   When the RabbitMQ login page appears, use the following default credentials (unless you configured custom ones):

   - **Username:** `guest`
   - **Password:** `guest`

### 7. **Verify RabbitMQ Operation**
   Once logged in, you should be able to view the RabbitMQ dashboard. You can check queues, exchanges, and other settings to ensure that RabbitMQ is functioning correctly.

### 8. **Troubleshooting**
   If you encounter issues, here are some things to check:
   - Ensure that the `kubectl port-forward` command is still running and has not been interrupted.
   - Confirm that RabbitMQ is running correctly with:

     ```bash
     kubectl get pods
     ```

   - If you can't connect, verify that there are no firewalls or network restrictions in *KillerCoda* that might block port `15672`.

   - You can also check the logs of the RabbitMQ pod for any issues:

     ```bash
     kubectl logs <my-rabbitmq-pod-name>
     ```

## Conclusion

By following these steps, you should have RabbitMQ installed and running on your Kubernetes cluster in *KillerCoda*, and you should be able to access the RabbitMQ management web interface via the integrated browser. Don't forget to use the `--address 0.0.0.0` option for port forwarding when working in *KillerCoda*.