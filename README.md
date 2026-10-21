# kubernetes_tech-week-unianchieta-2026-10
Conteúdos da apresentação "Kubernetes: o que é, por que é tão importante e como funciona na prática?".

## Exemplo

Um deployment em Kubernetes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: workerhttp
spec:
  selector:
    matchLabels:
      app: workerhttp
  replicas: 1
  template:
    metadata:
      labels:
        app: workerhttp
    spec:
      containers:
        - name: worker
          image: renatogroffe/dotnet10-worker-httprequest:2
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "256Mi"
          env:
          - name: EndpointRequest
            value: URL_TESTS
```

Escalando a aplicação:

```yaml
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: workerhttp-scaledobject-cron
spec:
  minReplicaCount: 1
  scaleTargetRef:
    name: workerhttp
  triggers:
  - type: cron
    metadata:
      timezone: America/Sao_Paulo
      start: 31 20 * * *
      end: 32 20 * * *
      desiredReplicas: "5"
```
