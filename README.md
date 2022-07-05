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
 │     ├─► {client1}                              ├─► notifications                         ├─► base                                │
 │     │   │                                      │   │                                     │   │                                   │
 │     │   ├─► dev                                │   ├─► kustomization.yaml                │   ├─► cloudos-apps                    │
 │     │   │   │                                  │   │                                     │   │   │                               │
 │     │   │   ├─► flux-system                    │   ├─► deployment.yaml                   │   │   ├─► kustomization.yaml          │
 │     │   │   │   │                              │   │                                     │   │   │                               │
 │     │   │   │   ├─► kustomization.yaml         │   └─► alerts.yaml                       │   │   ├─► repository.yaml             │
 │     │   │   │   │                              │                                         │   │   │                               │
 │     │   │   │   ├─► gotk-sync.yaml             ├─► chartmuseum                           │   │   └─► rabc.yaml                   │
 │     │   │   │   │                              │   │                                     │   │                                   │
 │     │   │   │   └─► gotk-components.yaml       │   ├─► kustomization.yaml                │   ├─► infra-apps                      │
 │     │   │   │                                  │   │                                     │   │   │                               │
 │     │   │   ├─► infrastructure.yaml            │   ├─► repository.yaml                   │   │   ├─► kustomization.yaml          │
 │     │   │   │                                  │   │                                     │   │   │                               │
 │     │   │   └─► tenants.yaml                   │   └─► deployment.yaml                   │   │   ├─► repository.yaml             │
 │     │   │                                      │                                         │   │   │                               │
 │     │   ├─► qa                                 ├─► ingress-internal                      │   │   └─► rabc.yaml                   │
 │     │   │   │                                  │   │                                     │   │                                   │
 │     │   │   ├─► flux-system                    │   ├─► kustomization.yaml                │   └─► data-apps                       │
 │     │   │   │   │                              │   │                                     │       │                               │
 │     │   │   │   ├─► kustomization.yaml         │   ├─► helm-repository.yaml              │       ├─► kustomization.yaml          │
 │     │   │   │   │                              │   │                                     │       │                               │
 │     │   │   │   ├─► gotk-sync.yaml             │   └─► helm-deployment.yaml              │       ├─► repository.yaml             │
 │     │   │   │   │                              │                                         │       │                               │
 │     │   │   │   └─► gotk-components.yaml       └─► ingress-external                      │       └─► rabc.yaml                   │
 │     │   │   │                                      │                                     │                                       │
 │     │   │   ├─► infrastructure.yaml                ├─► kustomization.yaml                ├─► {client1}                           │
 │     │   │   │                                      │                                     │   │                                   │
 │     │   │   └─► tenants.yaml                       ├─► helm-repository.yaml              │   ├─► dev                             │
 │     │   │                                          │                                     │   │   │                               │
 │     │   └─► pro                                    └─► helm-deployment.yaml              │   │   ├─► cloudos-apps                │
 │     │       │                                                                            │   │   │   │                           │
 │     │       ├─► flux-system                                                              │   │   │   ├─► kustomization.yaml      │
 │     │       │   │                                                                        │   │   │   │                           │
 │     │       │   ├─► kustomization.yaml                                                   │   │   │   └─► repo-path.yaml          │
 │     │       │   │                                                                        │   │   │                               │
 │     │       │   ├─► gotk-sync.yaml                                                       │   │   └─► data-apps                   │
 │     │       │   │                                                                        │   │       │                           │
 │     │       │   └─► gotk-components.yaml                                                 │   │       ├─► kustomization.yaml      │
 │     │       │                                                                            │   │       │                           │
 │     │       ├─► infrastructure.yaml                                                      │   │       └─► repo-path.yaml          │
 │     │       │                                                                            │   │                                   │
 │     │       └─► tenants.yaml                                                             │   ├─► qa                              │
 │     │                                                                                    │   │   │                               │
 │     └─► {client2}                                                                        │   │   ├─► cloudos-apps                │
 │         │                                                                                │   │   │   │                           │
 │         ├─► dev                                                                          │   │   │   ├─► kustomization.yaml      │
 │         │   │                                                                            │   │   │   │                           │
 │         │   ├─► flux-system                                                              │   │   │   └─► repo-path.yaml          │
 │         │   │   │                                                                        │   │   │                               │
 │         │   │   ├─► kustomization.yaml                                                   │   │   └─► data-apps                   │
 │         │   │   │                                                                        │   │       │                           │
 │         │   │   ├─► gotk-sync.yaml                                                       │   │       ├─► kustomization.yaml      │
 │         │   │   │                                                                        │   │       │                           │
 │         │   │   └─► gotk-components.yaml                                                 │   │       └─► repo-path.yaml          │
 │         │   │                                                                            │   │                                   │
 │         │   ├─► infrastructure.yaml                                                      │   └─► pro                             │
 │         │   │                                                                            │       │                               │
 │         │   └─► tenants.yaml                                                             │       ├─► cloudos-apps                │
 │         │                                                                                │       │   │                           │
 │         ├─► qa                                                                           │       │   ├─► kustomization.yaml      │
 │         │   │                                                                            │       │   │                           │
 │         │   ├─► flux-system                                                              │       │   └─► repo-path.yaml          │
 │         │   │   │                                                                        │       │                               │
 │         │   │   ├─► kustomization.yaml                                                   │       └─► data-apps                   │
 │         │   │   │                                                                        │           │                           │
 │         │   │   ├─► gotk-sync.yaml                                                       │           ├─► kustomization.yaml      │
 │         │   │   │                                                                        │           │                           │
 │         │   │   └─► gotk-components.yaml                                                 │           └─► repo-path.yaml          │
 │         │   │                                                                            │                                       │
 │         │   ├─► infrastructure.yaml                                                      └─► {client2}                           │
 │         │   │                                                                                │                                   │
 │         │   └─► tenants.yaml                                                                 ├─► dev                             │
 │         │                                                                                    │   │                               │
 │         └─► pro                                                                              │   ├─► infra-apps                  │
 │             │                                                                                │   │   │                           │
 │             ├─► flux-system                                                                  │   │   ├─► kustomization.yaml      │
 │             │   │                                                                            │   │   │                           │
 │             │   ├─► kustomization.yaml                                                       │   │   └─► repo-path.yaml          │
 │             │   │                                                                            │   │                               │
 │             │   ├─► gotk-sync.yaml                                                           │   └─► cloudos-apps                │
 │             │   │                                                                            │       │                           │
 │             │   └─► gotk-components.yaml                                                     │       ├─► kustomization.yaml      │
 │             │                                                                                │       │                           │
 │             ├─► infrastructure.yaml                                                          │       └─► repo-path.yaml          │
 │             │                                                                                │                                   │
 │             └─► tenants.yaml                                                                 ├─► qa                              │
 │                                                                                              │   │                               │
 │                                                                                              │   ├─► infra-apps                  │
 │                                                                                              │   │   │                           │
 │                                                                                              │   │   ├─► kustomization.yaml      │
 │                                                                                              │   │   │                           │
 │                                                                                              │   │   └─► repo-path.yaml          │
 │                                                                                              │   │                               │
 │                                                                                              │   └─► cloudos-apps                │
 │                                                                                              │       │                           │
 │                                                                                              │       ├─► kustomization.yaml      │
 │                                                                                              │       │                           │
 │                                                                                              │       └─► repo-path.yaml          │
 │                                                                                              │                                   │
 │                                                                                              └─► pro                             │
 │                                                                                                  │                               │
 │                                                                                                  ├─► infra-apps                  │
 │                                                                                                  │   │                           │
 │                                                                                                  │   ├─► kustomization.yaml      │
 │                                                                                                  │   │                           │
 │                                                                                                  │   └─► repo-path.yaml          │
 │                                                                                                  │                               │
 │                                                                                                  └─► cloudos-apps                │
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
