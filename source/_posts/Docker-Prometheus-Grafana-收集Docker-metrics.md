---
title: Docker Prometheus收集Docker metrics
date: 2018-11-16 16:32:42
tags:
---

启动 grafana
```shell
docker run --name grafana -d -p 3000:3000 grafana/grafana
```

启动 prometheus
```shell
docker run --name prometheus -d -p 9090:9090 -v ~/docker/prom/prometheus.yml:/etc/prometheus/prometheus.yml -v /docker/prom/rules/:/etc/prometheus/ prom/prometheus --config.file=/etc/prometheus/prometheus.yml
```

## Configure Docker

- **Docker for Mac / Docker for Windows**: Click the Docker icon in the toolbar, select **Preferences**, then select **Daemon**. Click **Advanced**.

If the file is currently empty, paste the following:
```
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

配置 prometheus.yml
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
      
# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093
      
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
  
  - job_name: 'mac'
    # 采集node exporter监控数据
    static_configs:
      - targets: ['docker.for.mac.localhost:9100']
```

## 参考

[Collect Docker metrics with Prometheus](https://docs.docker.com/config/thirdparty/prometheus/)

[prometheus](https://hub.docker.com/r/prom/prometheus/)