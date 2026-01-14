# Monitoring Stack (Local)

Local Docker Compose monitoring stack built around Prometheus + Grafana + Loki + Promtail.

## What’s Included

- **Prometheus** (`:9090`) for metrics scraping and alert rule evaluation
- **Grafana** (`:3000`) for dashboards (connects to Prometheus/Loki)
- **Loki** (`:3100`) for log storage and querying
- **Promtail** to ship Docker container logs to Loki

## Project Layout

- `docker-compose.yml`: Runs the stack on an external Docker network
- `prometheus.yml`: Prometheus scrape jobs + alert rule loading
- `alerts/*.yml`: Prometheus alerting rules
- `promtail.yml`: Promtail scrape/relabel config and log paths

## Quickstart

1) Create the external network (once):

`docker network create monitoring_net`

2) Start the stack:

`docker compose up -d`

3) Check logs if needed:

`docker compose logs -f`

4) Stop the stack:

`docker compose down`

## Endpoints

- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`
- Loki: `http://localhost:3100`

## Validate Configs

Prometheus config:

`docker run --rm -v "$PWD:/work" -w /work prom/prometheus promtool check config prometheus.yml`

Alert rules:

`docker run --rm -v "$PWD:/work" -w /work prom/prometheus promtool check rules alerts/*.yml`

## Notes

- `monitoring_net` is marked `external: true` in `docker-compose.yml`; create it first or update the compose file to manage the network.
- `promtail.yml` reads Docker container logs from `/var/lib/docker/containers` (mounted read-only into the promtail container).
