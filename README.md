# chia-farm-dashboard

This dashboard is also available online: https://grafana.com/grafana/dashboards/16456

<img src="doc/screenshot.png" alt="Screenshot">

## chia farms using a reverse proxy to limit open ports

In this example metrics, are available with the following endpoints:

- '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914/farmer/metrics
- '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914/harvester-1/metrics

Add a block to the `scrape_configs` of your `prometheus.yml` config file:

```
scrape_configs:
  - job_name: 'chia-exporter_farmer'
    scrape_interval: 15s
    metrics_path: /farmer/metrics
    static_configs:
      - targets: [ '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914' ]
        labels:
          application: 'chia-blockchain'
          node_id: 'farmer'

  - job_name: 'chia-exporter_harvester-1'
    scrape_interval: 15s
    metrics_path: /harvester-1/metrics
    static_configs:
      - targets: [ '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914' ]
        labels:
          application: 'chia-blockchain'
          node_id: 'harvester-1'
```

Adjust the host name accordingly.