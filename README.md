# Monitoring Kubernetes Project: Observability with Prometheus & Grafana

A hands-on demonstration of building a complete Kubernetes monitoring stack using **Prometheus**, **Node Exporter**, and **Kube-State-Metrics** on a **Minikube cluster**.

---

## Infrastructure Overview

| Component | Resource Name |
| :--- | :--- |
| **Kubernetes Cluster** | `Minikube` |
| **Monitoring Namespace** | `monitoring` |
| **Prometheus Stack** | `kube-prometheus-stack` |
| **Helm Repository** | `prometheus-community` |
| **Metrics Agent (Nodes)** | `Node Exporter` |
| **Cluster Metrics** | `Kube-State-Metrics` |

---

## Deploy Prometheus for Metrics Collection

### 1. Cluster Initialization
* Created and started a local Kubernetes cluster using Minikube.
* Created a dedicated namespace for monitoring workloads.

```bash
minikube start
kubectl create namespace monitoring
```
### 2.Prometheus on Kubernetes (EKS) using Helm
* Added Prometheus Helm repository and updated Helm charts.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
### 3.Installed Prometheus, Node Exporter, Kube-State-Metrics, Alertmanager, and Grafana using Helm
* Installed the full monitoring stack using Helm.
```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```
## Set up Grafana for Data Visualization
### 1. Grafana Access & Configuration
* Grafana was deployed automatically as part of the kube-prometheus-stack.
* Accessed Grafana locally using port-forwarding.

```bash
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```
* Password: retrieved via Kubernetes secret
```bash
kubectl get secret --namespace monitoring monitoring-grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode
```
### 2. Custom Dashboards Creation
* Cluster Health Dashboard
* This dashboard provides an overview of the overall state of the Kubernetes cluster.
| Metric | Description |
| :--- | :--- |
| **CPU Usage** | `Total CPU consumption across the cluster` |
| **Memory Usage** | `Total memory usage across all containers` |
| **Running Pods** | `Number of active pods in Running state` |

* Storage Monitoring
* This panel tracks disk utilization across nodes.
| Metric | Description |
| :--- | :--- |
| **Disk Usage** | `Percentage of used storage capacity` |

### 3. Visualization Choices

Different panel types were selected based on the nature of the data:

* Time Series
Used for CPU and memory metrics to show trends over time.
* Stat Panel
Used for displaying real-time values such as running pods.
* Gauge
Used for storage utilization to represent percentage-based capacity.
