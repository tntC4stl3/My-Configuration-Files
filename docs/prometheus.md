Prometheus
==========

* [Prometheus Official](https://prometheus.io)
* [Prometheus Github](https://github.com/prometheus)
* [Prometheus Docker](https://hub.docker.com/r/prom)


## Prometheus

### Docker

For production deployments it is highly recommended to use Docker data volume to store the actual metrics of Prometheus, that will be much easier to manage the data on Prometheus upgrades.

Suppose we want to use the `/prometheus-data` folder on host to work as volume and there is a prometheus configuraion file inside.

```
docker run -d --name=prometheus -p 9090:9090 \
    -v /prometheus-data:/prometheus \
    prom/prometheus:v1.6.0 -config.file=/prometheus/prometheus.yml
```

## Node-exporter

Download binary from [official release](https://github.com/prometheus/node_exporter/releases).

Extract and put the node-exporter binary to `/usr/local/bin`.

Create service file for node_exporter

```
$ cat /usr/lib/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter

[Service]
ExecStart=/usr/local/bin/node_exporter
Restart=always
RestartSec=20

[Install]
WantedBy=default.target
```

Enable service and start it.

```
# systemctl daemon-reload
# systemctl enable node_exporter.service
# systemctl start node_exporter.service
```

The node_exporter service will listen on 9100 port.

## Collect node metric

Basic prometheus yaml configuration:

```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'

    static_configs:
      - targets: ['xxx.xxx.xxx.xxx:9100', 'xxx.xxx.xxx.xxx:9100']
        labels:
          group: 'master'
```

Restart prometheus service.
