---
title: Docker Prometheus收集Docker metrics
date: 2018-11-16 16:32:42
tags:
---

### Configure Docker

- **Docker for Mac / Docker for Windows**: Click the Docker icon in the toolbar, select **Preferences**, then select **Daemon**. Click **Advanced**.

If the file is currently empty, paste the following:

```
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

```shell
docker pull prom/prometheus:v2.5.0
```



### Configure and run Prometheus

Prometheus runs as a Docker service on a Docker swarm.

> **Prerequisites**
>
> 1. One or more Docker engines are joined into a Docker swarm, using `docker swarm init`on one manager and `docker swarm join` on other managers and worker nodes.
> 2. You need an internet connection to pull the Prometheus image.

 Copy one of the following configuration files and save it to `/tmp/prometheus.yml` (Linux or Mac) or `C:\tmp\prometheus.yml` (Windows). This is a stock Prometheus configuration file, except for the addition of the Docker job definition at the bottom of the file. Docker for Mac and Docker for Windows need a slightly different configuration.

vim /tmp/prometheus.yml

```yaml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
      monitor: 'codelab-monitor'

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
       - targets: ['docker.for.mac.localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
    # - targets: ['docker.for.mac.host.internal:9323']
      - targets: ['docker.for.mac.localhost:9323']
```

运行

```shell
docker run -d --name prometheus-2.5.0 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml -p 9090:9090 prom/prometheus:v2.5.0
```

~~Next, start a single-replica Prometheus service using this configuration.~~


```shell
docker service create --replicas 1 --name prometheus-2.5.0 \
    --mount type=bind,source=/tmp/prometheus.yml,destination=/etc/prometheus/prometheus.yml \
    --publish published=9090,target=9090,protocol=tcp \
    prom/prometheus:v2.5.0
```

## 参考

[Collect Docker metrics with Prometheus](https://docs.docker.com/config/thirdparty/prometheus/)

[prometheus](https://hub.docker.com/r/prom/prometheus/)