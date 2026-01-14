# Repository Guidelines

## Project Structure & Module Organization

- `docker-compose.yml`: Local monitoring stack (Prometheus, Grafana, Loki, Promtail).
- `prometheus.yml`: Prometheus scrape jobs and alert rule loading.
- `alerts/`: Prometheus alerting rules (one or more `*.yml` files).
- `promtail.yml`: Promtail scrape/relabel config and log file paths.

## Build, Test, and Development Commands

- `docker network create monitoring_net`: Create the external Docker network used by `docker-compose.yml`.
- `docker compose up -d`: Start the stack in the background.
- `docker compose logs -f`: Tail service logs to debug configuration and connectivity.
- `docker compose down`: Stop and remove containers.

Endpoints (defaults): Prometheus `http://localhost:9090`, Grafana `http://localhost:3000`, Loki `http://localhost:3100`.

## Coding Style & Naming Conventions

- YAML: 2-space indentation, no tabs. Keep keys aligned and use lists for multiple targets/rules.
- Filenames: `kebab-case` for config and alert files (example: `alerts/voiceinvoices-alerts.yml`).
- Prometheus labels: prefer stable keys like `job`, `application`, `app`, and `severity`.

## Testing Guidelines

There are no automated tests in this repository. Validate configs before pushing:

- `docker run --rm -v "$PWD:/work" -w /work prom/prometheus promtool check config prometheus.yml`
- `docker run --rm -v "$PWD:/work" -w /work prom/prometheus promtool check rules alerts/*.yml`

## Commit & Pull Request Guidelines

- Commit messages: no strict convention yet (history is “Initial commit”). Use short, imperative summaries (e.g., “Add loki datasource”).
- PRs: include a clear description of the change, any new ports/paths/labels introduced, and screenshots for Grafana dashboard changes (if applicable).

## Configuration Tips

- `monitoring_net` is marked `external: true`; ensure it exists or change the compose file to manage it.
- `promtail.yml` currently references an absolute `__path__`; update it to match your host (or mount logs into the promtail container).
