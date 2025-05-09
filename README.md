# Fleet Monitoring Agent Repo

Цей репозиторій автоматично ставить Prometheus agent (Rancher Monitoring) через Fleet на всі підключені кластери.

## Версія
Monitoring: 104.1.4+up57.0.3

## Як працює
- Lightweight Prometheus agent з `remoteWrite` на центральний Prometheus у local кластері.
- Не піднімає Grafana та Alertmanager.
- Передає external label `cluster` для ідентифікації.

## Підключення до Rancher
Створіть GitRepo ресурс:
```yaml
apiVersion: fleet.cattle.io/v1alpha1
kind: GitRepo
metadata:
  name: monitoring-agents
  namespace: fleet-default
spec:
  repo: https://your-git-repo-url
  branch: main
  paths:
    - ./fleet/clusters/monitoring-agent
  targets:
    - clusterSelector: {}  # або вкажіть matchLabels, щоб обмежити
```

## Перевірка
1. Fleet застосує Helm chart на кожен кластер.
2. Перевірте namespace `cattle-monitoring-system` → повинен бути запущений агент.
3. У Grafana (центральна) шукайте дані по external_labels: `cluster=<ім’я_кластера>`.
