# flux_platform

## Schema

``` plain
 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │                                                                                                                                  │
 │ flux_platform repository                                                                                                         │
 │                                                                                                                                  │
 ├──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
 │                                                                                                                                  │
 │   ┌────────────────────────────────────┐     ┌───────────────────────────────────┐     ┌─────────────────────────────────────┐   │
 │   │                                    │     │                                   │     │                                     │   │
 │   │ clusters                           │     │ infrastructure                    │     │ tenants                             │   │
 │   │                                    │     │                                   │     │                                     │   │
 │   └─┬──────────────────────────────────┘     └─┬─────────────────────────────────┘     └─┬───────────────────────────────────┘   │
 │     │                                          │                                         │                                       │
 │     ├─► {cluster1}                             ├─► notifications                         └─► cloudOS-apps                        │
 │     │    │                                     │   │                                         │                                   │
 │     │    ├─► flux-system                       │   ├─► kustomization.yaml                    ├─► {client1}                       │
 │     │    │   │                                 │   │                                         │   │                               │
 │     │    │   ├─► kustomization.yaml            │   ├─► deployment.yaml                       │   ├─► dev                         │
 │     │    │   │                                 │   │                                         │   │   │                           │
 │     │    │   ├─► gotk-sync.yaml                │   └─► alerts.yaml                           │   │   ├─► kustomization.yaml      │
 │     │    │   │                                 │                                             │   │   │                           │
 │     │    │   └─► gotk-components.yaml          ├─► chartmuseum                               │   │   └─► repo-path.yaml          │
 │     │    │                                     │   │                                         │   │                               │
 │     │    ├─► infrastructure.yaml               │   ├─► kustomization.yaml                    │   ├─► qa                          │
 │     │    │                                     │   │                                         │   │   │                           │
 │     │    └─► tenants.yaml                      │   ├─► repository.yaml                       │   │   ├─► kustomization.yaml      │
 │     │                                          │   │                                         │   │   │                           │
 │     ├─► {cluster2}                             │   └─► deployment.yaml                       │   │   └─► repo-path.yaml          │
 │     │    │                                     │                                             │   │                               │
 │     │    ├─► flux-system                       ├─► ingress-internal                          │   └─► pro                         │
 │     │    │   │                                 │   │                                         │       │                           │
 │     │    │   ├─► kustomization.yaml            │   ├─► kustomization.yaml                    │       ├─► kustomization.yaml      │
 │     │    │   │                                 │   │                                         │       │                           │
 │     │    │   ├─► gotk-sync.yaml                │   ├─► helm-repository.yaml                  │       └─► repo-path.yaml          │
 │     │    │   │                                 │   │                                         │                                   │
 │     │    │   └─► gotk-components.yaml          │   └─► helm-deployment.yaml                  ├─► {client2}                       │
 │     │    │                                     │                                             │   │                               │
 │     │    ├─► infrastructure.yaml               └─► ingress-external                          │   ├─► dev                         │
 │     │    │                                         │                                         │   │   │                           │
 │     │    └─► tenants.yaml                          ├─► kustomization.yaml                    │   │   ├─► kustomization.yaml      │
 │     │                                              │                                         │   │   │                           │
 │     └─► {clusterN}                                 ├─► helm-repository.yaml                  │   │   └─► repo-path.yaml          │
 │          │                                         │                                         │   │                               │
 │          ├─► flux-system                           └─► helm-deployment.yaml                  │   ├─► qa                          │
 │          │   │                                                                               │   │   │                           │
 │          │   ├─► kustomization.yaml                                                          │   │   ├─► kustomization.yaml      │
 │          │   │                                                                               │   │   │                           │
 │          │   ├─► gotk-sync.yaml                                                              │   │   └─► repo-path.yaml          │
 │          │   │                                                                               │   │                               │
 │          │   └─► gotk-components.yaml                                                        │   └─► pro                         │
 │          │                                                                                   │       │                           │
 │          ├─► infrastructure.yaml                                                             │       ├─► kustomization.yaml      │
 │          │                                                                                   │       │                           │
 │          └─► tenants.yaml                                                                    │       └─► repo-path.yaml          │
 │                                                                                              │                                   │
 │                                                                                              └─► {client3}                       │
 │                                                                                                  │                               │
 │                                                                                                  ├─► dev                         │
 │                                                                                                  │   │                           │
 │                                                                                                  │   ├─► kustomization.yaml      │
 │                                                                                                  │   │                           │
 │                                                                                                  │   └─► repo-path.yaml          │
 │                                                                                                  │                               │
 │                                                                                                  ├─► qa                          │
 │                                                                                                  │   │                           │
 │                                                                                                  │   ├─► kustomization.yaml      │
 │                                                                                                  │   │                           │
 │                                                                                                  │   └─► repo-path.yaml          │
 │                                                                                                  │                               │
 │                                                                                                  └─► pro                         │
 │                                                                                                      │                           │
 │                                                                                                      ├─► kustomization.yaml      │
 │                                                                                                      │                           │
 │                                                                                                      └─► repo-path.yaml          │
 │                                                                                                                                  │
 │                                                                                                                                  │
 │                                                                                                                                  │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

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
