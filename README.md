# devops-otel-lab

Assignment 8 – OpenTelemetry Observability (DevOps course, NTUST)
Ramon Brechbühl, F11509001

## Files
- `app_console.py` – Task 1: OTel SDK, parent + child span, console exporter
- `app_otlp.py` – Task 2: same app, sends spans via OTLP to the collector
- `otel-collector-config.yaml` – collector pipeline: OTLP receiver → batch processor → debug + file exporter
- `out/telemetry.json` – traces exported by the collector

## Run

Install the SDK and the OTLP exporter:
```bash
pip install opentelemetry-sdk opentelemetry-exporter-otlp
```

Task 1 – console output:
```bash
python app_console.py
```

Task 2 – start the collector, then send spans:
```bash
mkdir -p out && chmod 777 out
docker run -d --rm --name otelcol \
  -p 4317:4317 \
  -v "$PWD/otel-collector-config.yaml:/etc/otelcol/config.yaml" \
  -v "$PWD/out:/out" \
  otel/opentelemetry-collector:0.164.0

python app_otlp.py
docker logs otelcol
```