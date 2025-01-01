# Configure Pod to Include Projected Volume

## Question

In the ckad14-sa-projected namespace, configure the ckad14-api-pod Pod to include a projected volume named vault-token.

Mount the service account token to the container at /var/run/secrets/tokens, with an expiration time of 7000 seconds.

Additionally, set the intended audience for the token to vault and path to vault-token.

## Solution

```bash
kubectl get pod -n ckad14-sa-projected ckad14-api-pod -o yaml > ckad-pro-vol.yaml
```

Update the pod manifest like so:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ckad14-api-pod
  namespace: ckad14-sa-projected
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: nginx
.
.
.
   volumeMounts:                              # Added
    - mountPath: /var/run/secrets/tokens      # Added
      name: vault-token                       # Added
.
.
.
  serviceAccount: ckad14-sa
  serviceAccountName: ckad14-sa
  volumes:
  - name: vault-token                   # Added
    projected:                          # Added
      sources:                          # Added
      - serviceAccountToken:            # Added
          path: vault-token             # Added
          expirationSeconds: 7000       # Added
          audience: vault               # Added
```

Replace the pod with the updated manifest:

```bash
kubectl replace -f ckad-pro-vol.yaml --force
```
