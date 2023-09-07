# Estrategias_Deploy

This test has executed into a K3D cluster.

- k3d cluster create -p "80:30000@loadbalancer"
- wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/baremetal/deploy.yaml
- Add nodePort 30000 to the ingress-nginx-controller cluster service.

Install Prometheus:
  - helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  - helm repo update
  - helm upgrade --install prometheus prometheus-community/prometheus --set alertmanager.enabled=false,server.persistentVolume.enabled=false,server.service.type=LoadBalancer,server.global.scrape_interval=10s,pushgateway.enabled=false

Install Grafana Loki:
  - helm repo add grafana https://grafana.github.io/helm-charts
  - helm repo update
  - helm upgrade --install loki grafana/loki-stack

Install Grafana with grafana.yaml.

To configure Grafana, add Prometheus as a DataSource and generate a dashboard with it.

PromQL to test in Grafana:
sum(rate(http_requests_total[30s])) by (versao)
