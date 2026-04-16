# Projet de Monitoring Kubernetes avec Prometheus & Grafana

# Monitoring Kubernetes - Partie 1 : Déploiement de Prometheus

## Objectifs
- Installer Prometheus via Helm
- Déployer Node Exporter
- Déployer Kube-State-Metrics
- Configurer les Scrape Jobs

## Prérequis
- Minikube
- Helm
- Namespace monitoring

## Commandes

```bash
minikube start
# Démarre un cluster Kubernetes local

kubectl create namespace monitoring
# Crée un namespace dédié au monitoring

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
# Ajoute le repository Helm officiel de Prometheus

helm repo update
# Met à jour la liste des charts Helm disponibles

helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
# Installe Prometheus, Node Exporter, Kube-State-Metrics et Grafana via le chart kube-prometheus-stack
