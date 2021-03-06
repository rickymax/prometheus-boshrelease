# Deploying Prometheus on OpenStack

## Example

```
bosh -e spacex -d prometheus deploy /opt/spacex/workspaces/prometheus-boshrelease/manifests/custom/prometheus-deployment.yml \
    --vars-store /opt/spacex/workspaces/prometheus-vars.yml \
    -v system_domain=<DOMAIN> \
    -v nginx_public_ip=<NGINX_PUBLIC_IP> \
    -v bosh_url=<BOSH_URL> \
    --var-file bosh_ca_cert=<BOSH_CA_CERT_FILE> \
    -v uaa_clients_prometheus_bosh_secret=`bosh int /opt/spacex/creds.yml --path /uaa_clients_prometheus_bosh_secret` \
    -v uaa_clients_prometheus_cf_secret=`bosh int /opt/spacex/workspaces/cf-vars.yml --path /uaa_clients_prometheus_cf_secret` \
    -v uaa_clients_prometheus_firehose_secret=`bosh int /opt/spacex/workspaces/cf-vars.yml --path /uaa_clients_prometheus_firehose_secret`
```

## Enable rabbitmq_exporter

Note: append all bosh deploy command as described above

```
bosh -e spacex -d prometheus deploy ...
    ...
    -o /opt/spacex/workspaces/prometheus-boshrelease/manifests/operators/monitor-rabbitmq.yml \
    -v rabbitmq_url=http://<RABBITMQ_HAPROXY_PUBLIC_IP>:15672 \
    -v rabbitmq_user=admin \
    -v rabbitmq_password=`bosh int /opt/spacex/workspaces/p-rabbitmq-vars.yml --path /management_password`
```
