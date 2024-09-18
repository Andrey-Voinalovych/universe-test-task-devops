# VictoriaMetrics & Grafana Helm Chart

Цей чарт розгортає кластер VictoriaMetrics з інтеграцією Grafana для моніторингу.

## Що не вдалося / Що можна покращити
1. Не коректно автоматично доабвляється кастомний дешборд графани. Ймовірне рішення - реалізація монтування кастомного дешборда через додатковий config-map.
2. Не вважаю оптимальним те, як додається scrapeConfig для vmagent. Не впевнений у best-practice для такого додавання, але можливо було б правильніше винести конфігурацію або в основний values файл, або в додатковий. Зараз мені не подобається, що конфіг такий переповнений.
3. Конфігурацію для скрейпінгу K8s кластера було взято з документації. За наявності більшого часу, міг би доопрацювати цей конфіг і змінити/почистити надлишкові дані.
4. Необхідно описати data lifecycle для сховища vm для PVC, оскільки з часом сховище заповнюється. Моя реалізація cold storage передбачала б використання S3 Glacier для міграції логів із vmstorage. Для фактичної міграції можливо використовував би Kubernetes Scheduled Jobs.
5. Необхідно більше часу попрацювати над візуалізацією отриманих метрик у Grafana для того, щоб знайти цікавіші закономірності для відображення.
6. Для vmagent варто прописати initContainer для очікування усіх цілей моніторингу бо зараз на початку з'являється багато failed логів через недоступність подів сервісу.

## Test Deploy Demo
Демо запуску чарта. 
https://www.loom.com/share/b81b80fac6134060b0a5cf246f490eef?sid=bdb4fd1b-4b9b-4eda-88de-d5d87dd2a866


## Встановлення чарта

Впевнимось, що всі залежності встановлені згідно з `Chart.yaml`:
```
helm dependency update
```

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



