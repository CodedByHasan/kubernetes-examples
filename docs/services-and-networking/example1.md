# Exposing Pods & Creating Policy

## Question

Create the following:

- A service `frontend-ckad-svcn` to expose the frontend pods outside the cluster on port 31100.
- A service `backend-ckad-svcn` to make backend pods to be accessible within the cluster.
- A policy `database-ckad-svcn` to limit access to database pods only to backend pods

## Solution

### Frontend service

A service `frontend-ckad-svcn` to expose the frontend pods _outside_ the cluster on port 31100.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: frontend-ckad-svcn
  namespace: app-ckad
spec:
  selector:
    app: frontend
  type: NodePort
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 31100
```

Using `kubectl`:

```bash
kubectl expose pod <pod-name>
        --type=NodePort
        --name=frontend-ckad-svcn
        --namespace=app-ckad
        --port=80
        --target-port=80
        --node-port=31100
```

### Backend Service

A service `backend-ckad-svcn` to make backend pods to be accessible _within_ the cluster:

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: backend-ckad-svcn
  namespace: app-ckad
spec:
  selector:
    app: backend
  ports:
    - name: http
      port: 80
      targetPort: 80
```

Using `kubectl`:

```bash
kubectl expose pod <pod-name>
        --name=backend-ckad-svcn
        --namespace=app-ckad
        --port=80
        --target-port=80
```

???+ note

    No need to specify `.spec.type = ClusterIP` because [ClusterIP](https://kubernetes.io/docs/concepts/services-networking/service/#type-clusterip) is the
    default service type in Kubernetes.

### Policy

A policy `database-ckad-svcn` to limit access of database pods only to backend pods:

```yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: database-ckad-svcn
  namespace: app-ckad
spec:
  podSelector:
    matchLabels:
      app: database
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: backend
```
