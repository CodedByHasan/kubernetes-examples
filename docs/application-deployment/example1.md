# Example 1

## Question

A web application running on cluster2 called robox-west-apd on the fusion-apd-x1df5 namespace. The Ops team
has created a new service account (SA) with a set of permissions for this web application. Update the newly
created SA for this deployment.

Also, change the strategy type to `Recreate`, so it will delete all the pods immediately and update the newly
created SA to all the pods.

## Solution

Use the kubectl get command to list all the given resources:

```bash
kubectl get po,deploy,sa,ns -n fusion-apd-x1df5
```

Inspect the service account is used by the pods/deployment.

```bash
kubectl get deploy -n fusion-apd-x1df5 robox-west-apd -oyaml
```

The deployment is using the default service account.

Now, use the kubectl get command to retrieves the YAML definition of the deployment named robox-west-apd and
save it into a file.

```bash
kubectl get deploy -n fusion-apd-x1df5 robox-west-apd -o yaml > <FILE-NAME>.yaml
```

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    global-kgh: robox-west-apd
  name: robox-west-apd
  namespace: fusion-apd-x1df5
spec:
  replicas: 3
  selector:
    matchLabels:
      global-kgh: robox-west-apd
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        global-kgh: robox-west-apd
    spec:
      containers:
        - image: nginx
          imagePullPolicy: Always
          name: robox-container
      serviceAccountName: galaxy-apd-xb12 # Change default service account
```

Now, replace the resource with the following command:

```bash
kubectl replace -f <FILE-NAME>.yaml --force
```
