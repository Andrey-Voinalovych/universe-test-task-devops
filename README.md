# VictoriaMetrics & Grafana Helm Chart

Цей чарт розгортає кластер VictoriaMetrics з інтеграцією Grafana для моніторингу.

## Встановлення чарта

Для того щоб запустити даний чарт, виконайте наступну команду:

```bash
helm install vmcluster .
```

Після успішного деплою ви побачите повідомлення, схоже на це:

```
NAME: vmcluster
LAST DEPLOYED: Wed Sep 18 10:44:33 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Hi, Universe team! vmcluster chart is deploying.
**********************************************************************************
1. To retrieve the admin password for Grafana, run the following command:

  kubectl get secret --namespace default vmcluster-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

2. To get access to grafana panel use:

  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=vmcluster" -o jsonpath="{.items[0].metadata.name}") &&
  kubectl --namespace default port-forward $POD_NAME 3000

```

## Використаний сетап для розробки цього чарту
Для розробки та тестування використовувався кластер Minikube на 3 ноди.

## Що не вдалося / Що можна покращити


