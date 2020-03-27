# Traefik - Promtheus operator

In this exmaple we will see how to install and configure the Prometheus Operator to scrape Traefik metrics.

First of all we will install a k3s cluster with the following command:
```bash
k3d create --server-arg "--no-deploy=traefik" \
--name="test" \
--workers="2" \
--publish="80:80" \
--publish="443:443"
```

Once the k3s is up and running we will install the prometheus opeator:

## Install prometheus-operator

```bash
kubectl create namespace monitoring
helm install --namespace monitoring monitoring stable/prometheus-operator --values monitoring/values.yaml
```

## Deploy traefik

```
helm repo add traefik https://containous.github.io/traefik-helm-chart
helm repo update

kubectl create namespace traefik
helm install --namespace traefik traefik traefik/traefik --values traefik/values.yaml
```

## Deploy ingresses

```
kubectl apply -f ingresses
```

https://dashboard.docker.localhost
https://prometheus.docker.localhost


## Deploy monitor

```
kubectl apply -f monitor/
```

Traefik is now monitored by prometheus.
