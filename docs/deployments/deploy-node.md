# Deploy Node

Deploy Node and expose it using NodePort.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: node-deployment
  name: node-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: node-deployment
    spec:
      containers:
        - image: gcr.io/kodekloud/centos-ssh-enabled:node
          name: centos-ssh-enabled
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: node-deployment
  name: node-deployment
spec:
  ports:
    - port: 8080
      protocol: TCP
      targetPort: 8080
      nodePort: 30012
  selector:
    app: node-deployment
  type: NodePort
```
