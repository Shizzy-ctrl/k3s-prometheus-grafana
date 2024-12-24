# k3s-prometheus-grafana

## Prerequisites
1. **Kubernetes Cluster**  
   Ensure you have a running Kubernetes cluster (e.g., [k3s](https://k3s.io/)).

2. **Helm Installed**  
   Verify Helm is installed on your system. If not, follow the installation guide [here](https://helm.sh/docs/intro/install/).

3. **KUBECONFIG Environment Variable**  
   If the `KUBECONFIG` environment variable is not set, you must specify the kubeconfig path using `--kubeconfig` in all Helm commands. For example:
   ```sh
   helm --kubeconfig /etc/rancher/k3s/k3s.yaml <command>


## Installation Steps

1. Add Prometheus Helm Repository

```sh
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

2. Create Namespace
Create a dedicated namespace for monitoring tools:
```sh
kubectl create namespace monitoring
```

3. Create custom-values.yaml
Create a file named custom-values.yaml with the following configuration to set up Prometheus and Grafana services as NodePort:
```yaml
prometheus:
  service:
    type: NodePort
grafana:
  service:
    type: NodePort
```


4. Install Prometheus and Grafana

```sh
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f custom-values.yaml -n monitoring
```

## Accesing grafana

1. Get Node IP Address
Run the following command to find the IP address of your nodes:
```sh
kubectl get nodes -o wide
```
2. Check Grafana Service Port
Determine the NodePort assigned to Grafana by running:

```sh
kubectl get services -n monitoring
```
3. Access Grafana
Open your browser and navigate to:
http://<node-ip-address>:<nodeport>

![obraz](https://github.com/user-attachments/assets/c66e8195-d676-4451-9c82-1004a80b4e2b)


## Default login credentials

Login: admin
Password: prom-operator


## Dashboard
Once logged in, you can explore the predefined dashboards available in Grafana. Below is a sample view of the Grafana dashboard:

![obraz](https://github.com/user-attachments/assets/c512d0f2-c5c3-4eff-8c13-00c016df6948)

