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
 │     ├─► {cluster1}                             ├─► notifications                         ├─► base                                │
 │     │    │                                     │   │                                     │   │                                   │
 │     │    ├─► flux-system                       │   ├─► kustomization.yaml                │   ├─► cloudos-apps                    │
 │     │    │   │                                 │   │                                     │   │   │                               │
 │     │    │   ├─► kustomization.yaml            │   ├─► deployment.yaml                   │   │   ├─► kustomization.yaml          │
 │     │    │   │                                 │   │                                     │   │   │                               │
 │     │    │   ├─► gotk-sync.yaml                │   └─► alerts.yaml                       │   │   ├─► repository.yaml             │
 │     │    │   │                                 │                                         │   │   │                               │
 │     │    │   └─► gotk-components.yaml          ├─► chartmuseum                           │   │   └─► rabc.yaml                   │
 │     │    │                                     │   │                                     │   │                                   │
 │     │    ├─► infrastructure.yaml               │   ├─► kustomization.yaml                │   ├─► infra-apps                      │
 │     │    │                                     │   │                                     │   │   │                               │
 │     │    └─► tenants.yaml                      │   ├─► repository.yaml                   │   │   ├─► kustomization.yaml          │
 │     │                                          │   │                                     │   │   │                               │
 │     ├─► {cluster2}                             │   └─► deployment.yaml                   │   │   ├─► repository.yaml             │
 │     │    │                                     │                                         │   │   │                               │
 │     │    ├─► flux-system                       ├─► ingress-internal                      │   │   └─► rabc.yaml                   │
 │     │    │   │                                 │   │                                     │   │                                   │
 │     │    │   ├─► kustomization.yaml            │   ├─► kustomization.yaml                │   └─► data-apps                       │
 │     │    │   │                                 │   │                                     │       │                               │
 │     │    │   ├─► gotk-sync.yaml                │   ├─► helm-repository.yaml              │       ├─► kustomization.yaml          │
 │     │    │   │                                 │   │                                     │       │                               │
 │     │    │   └─► gotk-components.yaml          │   └─► helm-deployment.yaml              │       ├─► repository.yaml             │
 │     │    │                                     │                                         │       │                               │
 │     │    ├─► infrastructure.yaml               └─► ingress-external                      │       └─► rabc.yaml                   │
 │     │    │                                         │                                     │                                       │
 │     │    └─► tenants.yaml                          ├─► kustomization.yaml                ├─► {client1}                           │
 │     │                                              │                                     │   │                                   │
 │     └─► {clusterN}                                 ├─► helm-repository.yaml              │   ├─► dev                             │
 │          │                                         │                                     │   │   │                               │
 │          ├─► flux-system                           └─► helm-deployment.yaml              │   │   ├─► cloudos-apps                │
 │          │   │                                                                           │   │   │   │                           │
 │          │   ├─► kustomization.yaml                                                      │   │   │   ├─► kustomization.yaml      │
 │          │   │                                                                           │   │   │   │                           │
 │          │   ├─► gotk-sync.yaml                                                          │   │   │   └─► repo-path.yaml          │
 │          │   │                                                                           │   │   │                               │
 │          │   └─► gotk-components.yaml                                                    │   │   └─► data-apps                   │
 │          │                                                                               │   │       │                           │
 │          ├─► infrastructure.yaml                                                         │   │       ├─► kustomization.yaml      │
 │          │                                                                               │   │       │                           │
 │          └─► tenants.yaml                                                                │   │       └─► repo-path.yaml          │
 │                                                                                          │   │                                   │
 │                                                                                          │   ├─► qa                              │
 │                                                                                          │   │   │                               │
 │                                                                                          │   │   ├─► cloudos-apps                │
 │                                                                                          │   │   │   │                           │
 │                                                                                          │   │   │   ├─► kustomization.yaml      │
 │                                                                                          │   │   │   │                           │
 │                                                                                          │   │   │   └─► repo-path.yaml          │
 │                                                                                          │   │   │                               │
 │                                                                                          │   │   └─► data-apps                   │
 │                                                                                          │   │       │                           │
 │                                                                                          │   │       ├─► kustomization.yaml      │
 │                                                                                          │   │       │                           │
 │                                                                                          │   │       └─► repo-path.yaml          │
 │                                                                                          │   │                                   │
 │                                                                                          │   └─► pro                             │
 │                                                                                          │       │                               │
 │                                                                                          │       ├─► cloudos-apps                │
 │                                                                                          │       │   │                           │
 │                                                                                          │       │   ├─► kustomization.yaml      │
 │                                                                                          │       │   │                           │
 │                                                                                          │       │   └─► repo-path.yaml          │
 │                                                                                          │       │                               │
 │                                                                                          │       └─► data-apps                   │
 │                                                                                          │           │                           │
 │                                                                                          │           ├─► kustomization.yaml      │
 │                                                                                          │           │                           │
 │                                                                                          │           └─► repo-path.yaml          │
 │                                                                                          │                                       │
 │                                                                                          └─► {client2}                           │
 │                                                                                              │                                   │
 │                                                                                              ├─► dev                             │
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
 │                                                                                              ├─► qa                              │
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
