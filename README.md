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

## configure sops

### SOPS Standalon

[FluxCD SOPS](https://fluxcd.io/docs/guides/mozilla-sops/)

Generate secret key (only necessary to be done once for each client):

``` shell
export KEY_NAME="CLIENT"
export KEY_COMMENT="flux secrets"

gpg --batch --full-generate-key <<EOF
%no-protection
Key-Type: 1
Key-Length: 4096
Subkey-Type: 1
Subkey-Length: 4096
Expire-Date: 0
Name-Comment: ${KEY_COMMENT}
Name-Real: ${KEY_NAME}
EOF

gpg --list-secret-keys "${KEY_NAME}"

export KEY_FP=$(gpg --list-secret-keys ${KEY_NAME} | grep -A1 "sec " | tail -1 | awk '{print $1}')

gpg --export-secret-keys --armor "${KEY_FP}" |
  kubectl create secret generic sops-gpg \
  --namespace=flux-system \
  --from-file=sops.asc=/dev/stdin

gpg --export --armor "${KEY_FP}" > ./clusters/{client}/{env}/.sops.pub.asc

```

Export Private GPG key and upload it to LastPass

``` shell
gpg --export-secret-keys --armor "${KEY_FP}" > private.gpg

# Upload private.gpg file to LastPass

rm -rf private.gpg
```

Import Private key into our local machine, this is necessary to be able to encrypt secrets

```shell
gpg --import private.gpg
```

Use kustomization with sops secrets

``` yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: secret
  namespace: app
spec:
  decryption:
    provider: sops
    secretRef:
      name: sops-gpg
```

### SOPS KMS

[FluxCD SOPS](https://fluxcd.io/docs/guides/mozilla-sops/#encrypting-secrets-using-various-cloud-providers)

[EKSCTL](https://eksctl.io/usage/iamserviceaccounts/)

``` shell
eksctl utils associate-iam-oidc-provider --cluster=<clusterName>

eksctl create iamserviceaccount \
  --role-only \
  --name=kustomize-controller \
  --namespace=flux-system \
  --attach-policy-arn=<policyARN> \
  --cluster=<clusterName>

kubectl -n flux-system annotate serviceaccount kustomize-controller \
  --field-manager=flux-client-side-apply \
  eks.amazonaws.com/role-arn='arn:aws:iam::<ACCOUNT_ID>:role/<KMS-ROLE-NAME>'

kubectl -n flux-system rollout restart deployment/kustomize-controller
```
