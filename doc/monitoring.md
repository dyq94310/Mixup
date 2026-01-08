快速打开prometheus
```bash
kubectl port-forward deployment/prometheus 9090:9090 -n monitoring
```