# flux_platform

## bootstrap

```shell  
flux bootstrap github \
  --token-auth \
  --hostname=github.com \           
  --owner=dtrinf \  
  --repository=flux_platform \
  --branch=main \
  --path=clusters/{{cluster-name}}
```

## create kustomization

``` shell
flux create kustomization monitoring-config \
  --depends-on=kube-prometheus-stack \
  --interval=1h \
  --prune=true \
  --source=flux-monitoring \
  --path="./manifests/monitoring/monitoring-config" \
  --health-check-timeout=1m \
  --wait \
  --export > flux-monitoring-grafana-release.yaml
```

## create source

``` shell
flux create source git flux-monitoring \
  --interval=30m \
  --url=https://github.com/fluxcd/flux2 \
  --branch=main \
  --export > flux-monitoring-repository.yaml
```

## SLACK notifications

``` shell
kubectl -n flux-system create secret generic slack-url \
  --from-literal=address={{slack_url}}
```
