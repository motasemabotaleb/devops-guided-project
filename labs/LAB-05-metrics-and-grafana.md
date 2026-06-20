# LAB-05 Metrics and Grafana

## Goal

Use metrics to see service behavior over time.

## Problem Scenario

You know requests are happening, but you need trends instead of single events.

## Files Used

- `monitoring/prometheus/prometheus.yml`
- `monitoring/grafana/dashboards/devops-overview.json`
- `app/src/metrics.js`

## Commands to Run

```bash
docker compose logs prometheus --tail=30
docker compose logs grafana --tail=30
```

## GUI Actions to Click

- Generate Slow Request
- Generate Error
- Check Readiness

## Expected Output

- request rate increases
- error count changes after `/error`
- latency rises after `/slow`
- DB and Redis readiness gauges show current state

## Checkpoint Questions

- What can the dashboard tell you quickly?
- What can the dashboard not tell you without logs?

## Common Issues

- dashboard empty because no traffic was generated

## Team Task Split

- Student 1 generates traffic
- Student 2 reads the dashboard
- Student 3 correlates metrics with logs
- Student 4 explains readiness gauges

## Instructor Checkpoint

Teams must explain one thing they learned from metrics and one thing they still needed logs to understand.

## Next Step

Continue to [LAB-06 GitHub Actions ACR](LAB-06-github-actions-acr.md).
