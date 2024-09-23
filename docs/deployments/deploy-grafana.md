# Deploy Grafana

Deploy Grafana and expose it using NodePort service.

```yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: grafana-deployment-datacenter
  name: grafana-deployment-datacenter
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana-deployment-datacenter
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: grafana-deployment-datacenter
    spec:
      containers:
        - image: grafana/grafana
          name: grafana-container
          ports:
            - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: grafana-deployment-datacenter
  name: grafana-deployment-datacenter
spec:
  ports:
    - port: 3000
      protocol: TCP
      targetPort: 3000 # matches containerPort
      nodePort: 32000
  selector:
    app: grafana-deployment-datacenter
  type: NodePort
```
