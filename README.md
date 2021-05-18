# Monitoring

## Node exporter

Lancez le conteneur **node-exporter** sur votre poste.

```
docker run --net="host" -d -v "/:/host:ro,rslave" --pid="host" quay.io/prometheus/node-exporter:latest --web.listen-address=":9100" --path.rootfs=/host
docker ps
```

- Qu'observez-vous sur l'URL suivante ? : http://localhost:9100/metrics
- Expliquez les paramètres suivants:
  * -d
  * -v "/:/host:ro,rslave"
  * --pid="host"
  
## Prometheus

Créez localement le fichier `prometheus.yml` avec le contenu suivant

```
global:
  scrape_interval: 5s

scrape_configs:
- job_name: 'node-exporter'
  static_configs:
    - targets: ['localhost:9100']
```

Lancez le conteneur Prometheus:

```
docker run --net="host" -d -v $PWD/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus:latest
docker ps
```

Allez sur: http://localhost:9090/graph

Graphez la métrique: `node_filesystem_free_bytes`

## Docker Hub Exporter

Pour grapher des mesures sur votre repository Docker Hub cesisocial, il existe un exporter pour ça !

```
docker run --net="host" -d infinityworks/docker-hub-exporter:latest -listen-address=:9170 -images="yanc0/cesisocial"
docker ps
```
Allez sur: http://localhost:9090/graph

Graphez la métrique: `docker_hub_image_pulls_total`


