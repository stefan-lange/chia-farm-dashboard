# Chia Farm Dashboard

Displays all important metrics from your [Chia](https://github.com/Chia-Network/chia-blockchain/) farming node and all
connected harvesters, collected by the official
[Chia Exporter](https://github.com/Chia-Network/chia-exporter/). To show the current price of Chia, the
unofficial [Chia Price Exporter](https://github.com/stefan-lange/chia-price-exporter/) is required.

This dashboard is based on the great work made by [Philipp Normann](https://github.com/philippnormann/chia-monitor) and
available under <https://grafana.com/grafana/dashboards/16456>

<img src="doc/screenshot.png" alt="Screenshot">

**_The dashboard is an alpha version and may contain some bugs or unimplemented features!_**

## Configuration

A working [Chia Exporter](https://github.com/Chia-Network/chia-exporter/) (version `>= 0.10.0`)
and [Chia Price Exporter](https://github.com/stefan-lange/chia-price-exporter/) is required.

### Example: chia farms using a reverse proxy to limit open ports

In this example, metrics are available with the following endpoints:

- `<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>`:9914/price/metrics
- `<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>`:9914/farmer/metrics
- `<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>`:9914/harvester-1/metrics

Add a block to the `scrape_configs` of your `prometheus.yml` config file:

```yaml
scrape_configs:
    -   job_name: 'chia-price-exporter'
        scrape_interval: 60s
        metrics_path: /price/metrics
        static_configs:
            -   targets: [ '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914' ]

    -   job_name: 'chia-exporter_farmer'
        scrape_interval: 15s
        metrics_path: /farmer/metrics
        static_configs:
            -   targets: [ '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914' ]
                labels:
                    application: 'chia-blockchain'
                    node_id: 'farmer'

    -   job_name: 'chia-exporter_harvester-1'
        scrape_interval: 15s
        metrics_path: /harvester-1/metrics
        static_configs:
            -   targets: [ '<<CHIA-FARM-REVERSE-PROXY-HOSTNAME>>:9914' ]
                labels:
                    application: 'chia-blockchain'
                    node_id: 'harvester-1'
```

Adjust the host name accordingly.