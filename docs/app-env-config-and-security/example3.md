# Update SA for Pod

## Question

We have a Kubernetes namespace called ckad12-ctm-sa-aecs, which contains a service account and a pod. Your task is to
modify the pod so that it uses the service account defined in the same namespace.

Additionally, you need to ensure that the pod has access to the API credentials associated with the service account
by enabling the automounting feature for the credentials.

## Solution

Remove `automountServiceAccountToken: false` field to enable automount of credentials and specifying service
account as: `serviceAccountName: ckad12-my-custom-sa-aecs`

```bash
kubectl get pods -n ckad12-ctm-sa-aecs ckad12-ctm-nginx-aecs -o yaml > ckad-custom-sa.yaml
```

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: ckad12-ctm-nginx-aecs
  namespace: ckad12-ctm-sa-aecs
spec:
  # automountServiceAccountToken: false *removed to enable automount of creds
  containers:
    - image: nginx
      imagePullPolicy: Always
      name: nginx
  serviceAccountName: ckad12-my-custom-sa-aecs # using custom sa
```

```bash
kubectl replace -f ckad-custom-sa.yaml --force
```
