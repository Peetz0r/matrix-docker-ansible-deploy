# Enabling metrics and graphs for your Matrix server (optional)

It can be useful to have some (visual) insight in the performance of your homeserver.

You can enable this with the following settings in your configuration file (`inventory/host_vars/matrix.<your-domain>/vars.yml`):

```yaml
matrix_prometheus_enabled: true

matrix_synapse_metrics_enabled: true
matrix_prometheus_node_exporter_enabled: true

matrix_grafana_enabled: true
matrix_grafana_anonymous_access: true
matrix_grafana_default_admin_user: yourname
matrix_grafana_default_admin_password: securelongpassword
```

The dashboards will by default be available on the `stats.<your-domain>` subdomain, proxied via Nginx.

## What does it do?

Name | Description
-----|----------
`matrix_prometheus_enabled`|Prometheus is a time series database. It holds all the data we're going to talk about.
`matrix_synapse_metrics_enabled`|Enables metrics specific to Synapse
`matrix_prometheus_node_exporter_enabled`|Node Exporter is an addon of sorts to Prometheus that collects generic system information such as CPU, memory, filesystem, and even system temperatures
`matrix_grafana_enabled`|Grafana is the visual component. It shows the dashboards with the graphs that we're interested in
`matrix_grafana_anonymous_access`|By default you need to login to see graphs. If you want to publicly share your graphs (e.g. when asking for help in [`#synapse:matrix.org`](https://matrix.to/#/#synapse:matrix.org?via=matrix.org&via=privacytools.io&via=mozilla.org)) you'll want to enable this option.
`matrix_grafana_default_admin_user`<br>`matrix_grafana_default_admin_password`|By default Grafana creates a user with `admin` as the username and password. If you feel this is insecure and you want to change it beforehand, you can do that here

## Security and privacy

Metrics and resulting graphs can contain a lot if information. This includes system specs but also usage patterns. This applies especially to small personal/family scale homeservers. Someone might be able to figure out when you wake up and go to sleep by looking at the graphs over time. Think about this before enabling anonymous access. And you should really not forget to change your Grafana password.

Most of our docker containers run with limited system access, but the `prometheus-node-exporter` has access to the host network stack and (readonly) root filesystem. This is required to report on them. If you don't like that, you can set `matrix_prometheus_node_exporter_enabled: false` (which is actually the default). You will still get Synapse metrics with this container disabled. Both of the dashboards will always be enabled, so you can still look at historical data after disabling either source.

## More inforation

- [Understanding Synapse Performance Issues Through Grafana Graphs](https://github.com/matrix-org/synapse/wiki/Understanding-Synapse-Performance-Issues-Through-Grafana-Graphs) at the Synapse Github Wiki
- [The Prometheus scraping rules](https://github.com/matrix-org/synapse/tree/master/contrib/prometheus) (we use v2)
- [The Synapse Grafana dashboard](https://github.com/matrix-org/synapse/tree/master/contrib/grafana)
- [The Node Exporter dashboard](https://github.com/rfrail3/grafana-dashboards) (for generic non-synapse performance graphs)
