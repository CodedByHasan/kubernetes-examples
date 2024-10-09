# Deploy Tomcat

Deploy Tomcat and expose it using a NodePort Service.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: tomcat-deployment-nautilus
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-deployment-nautilus
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: tomcat-deployment-nautilus
    spec:
      containers:
        - image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          name: tomcat-container-nautilus
          ports:
            - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: tomcat-deployment-nautilus
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  ports:
    - port: 8080
      protocol: TCP
      targetPort: 8080
      nodePort: 32227
  selector:
    app: tomcat-deployment-nautilus
  type: NodePort
```
