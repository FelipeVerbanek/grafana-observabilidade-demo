# Observabilidade com Grafana, Tempo, Loki, FluentBit, OpenTelemetry, Mimir

Este guia fornece instruções para instalar componentes de observabilidade em um cluster Kubernetes, incluindo Grafana, Tempo, Loki, FluentBit, OpenTelemetry Collector, Mimir

## Pré-requisitos
- Helm instalado
- kubectl configurado para acessar seu cluster Kubernetes

---

## Passo a Passo de Instalação

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```
## Criação do namespace mimir

```bash
kubectl apply -f namespace/mimir.yaml
```
### 1. Instalação do Grafana
```bash
helm upgrade -f grafana/grafana-helm.yaml --install grafana grafana/grafana
```
### 2. Instalação do Tempo
```bash
helm upgrade --install tempo grafana/tempo -f tempo/values.yaml
```
### 3. Instalação do Ingress para Tempo e Grafana
```bash
kubectl apply -f grafana/ingress.yaml
```
### 4. Instalação do Loki
```bash
helm upgrade --install --values loki/values.yaml loki grafana/loki
```
### 5. Instalação do FluentBit
```bash
helm upgrade --install fluent-bit fluent/fluent-bit -f fluentbit/values.yaml
```
### 6. Instalação do OpenTelemetry Collector
```bash
helm upgrade --install my-opentelemetry-collector open-telemetry/opentelemetry-collector --set mode=daemonset --set image.repository="otel/opentelemetry-collector-k8s" --set command.name="otelcol-k8s" -f otel/values.yaml
```
### 7. Instalação do Mimir
```bash
helm upgrade --install mimir grafana/mimir-distributed -n mimir -f mimir/values.yaml
```
