# IoT-observability
Grafana + Promethues + Loki + Promtail

# Actors
* IoT Device
* Obervability Server

# Installing

## IoT Device

### Log Collector

* Grafana Promtail

```console
./promtail-linux-arm --config.file=promtail-local-config.yaml
```

### Metrics Collector

* Prometheus Node Exporter

```console
./node_exporter --collector.systemd --collector.processes
```

> Make sure to link the labels with the prometheus server configuration

* `host/promtail-local-config.yaml`

```yaml
scrape_configs:
- job_name: node
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      iotProperty1: iotProperty1Value
      __path__: /var/log/CTP*
```

## Observability Server

### Configuration

* Prometheus 

* `config/prometheus-config.yml`

```yaml
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'node'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['<IoT Host IP>:9100']
      labels:
        iotProperty1: iotProperty1Value
```

```console
docker-compose up -d
```

# Observing

Open `http://localhost:3000/`

* Username: admin
* Password: admin

`http://localhost:9090/`
`http://localhost:3100/`
