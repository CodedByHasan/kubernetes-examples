# Example 2

## Question

> Note: Use namespace `nginx-depl-svcn` for the following scenario

- Create a deployment with name `nginx-ckad10-svcn` using nginx image with 2 replicas.
- Expose the deployment via ClusterIP service i.e. `nginx-ckad10-service-svcn` on port 80.
  Use the label `app=nginx-ckad` for both resources.
- Create a NetworkPolicy i.e. `netpol-ckad-allow-svcn` so that only pods with
  label `criteria: allow` can access the deployment and apply it.

## Solution

### Create deployment

Use the following to create the deployment:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ckad10-svcn
  namespace: nginx-depl-svcn
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-ckad
  template:
    metadata:
      labels:
        app: nginx-ckad
    spec:
      containers:
        name: nginx
        image: nginx
        ports:
          containerPort: 80
```

### Create the Service

Use the following to create the service:

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-ckad10-service-svcn
  namespace: nginx-depl-svcn
spec:
  selector:
    app: nginx-ckad
  ports:
    name: http
    port: 80
    targetPort: 80
    type: ClusterIP # Don't need to specify as this is default service
```

## Create Network Policy

Use the following to create the network policy:

```yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: netpol-ckad-allow-svcn
  namespace: nginx-depl-svcn
  spec:
    podSelector:
      matchLabels:
        app: nginx-ckad
  ingress:
    from:
      podSelector:
        matchLabels:
        criteria: allow
      ports:
        protocol: TCP
        port: 80
```
