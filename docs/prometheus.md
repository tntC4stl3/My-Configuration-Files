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

## Alertmanager
If you want to all alert function to prometheus, you need another component with name [Alertmanager](https://github.com/prometheus/alertmanager) in Prometheus' eco-system.

Install is easy, and you can run it in a docker container.

You need to create a configuration for Alertmanager, here is a simple one to integrate with Slack:
```
[root@localhost ~]# cat /alertmanager/simple.yml 
route:
  group_by: ['alertname', 'group', 'location']
  receiver: 'slack-notifications'

receivers:
- name: 'slack-notifications'
  slack_configs:
  - api_url: 'http://slack/api/address'
    send_resolved: true
    channel: '#firing'
    title: '[{{ .Status | toUpper }}{{ if eq .Status "firing" }}:{{ .Alerts.Firing | len }}{{ end }}] {{ .GroupLabels.SortedPairs.Values | join " | " }}'
    text: "Relative servers: \n{{ range .Alerts }}[{{ .Status | toUpper }}] {{ .Labels.instance }}\n{{ end }}"
```

With this configuration, your alert notification will be group by prometheus metric label `alertname`, `group` and `location`. 

The alert will be reported at Slack firing channel and you will get a resolve notification as well if it is resolved.

After setting up alertmanager, create alert rules in Prometheus and let Prometheus communicate with alertmanager.

For example, add a rule to server is down:
```
[root@localhost ~]# cat /prometheus-data/alert.rules 
# Alert for any instance that is unreachable for >5 minutes.
ALERT InstanceDown
  IF up{job="node-exporter"} == 0
  FOR 5m

  ANNOTATIONS {
    summary = "Instance {{ $labels.instance }} is down",
    description = "Instance {{ $labels.instance }} of job {{ $labels.job }} has been down for more than 5 minutes."
  }
  ```
  
Add rules section to prometheus configuration:
```
[root@localhost ~]# cat /prometheus-data/prometheus.yml 
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'prometheus-monitor'

rule_files:
  - "alert.rules"
...
```

Restart prometheus and add an extra `-alertmanager.url` option to let Prometheus know where is the alertmanager service.