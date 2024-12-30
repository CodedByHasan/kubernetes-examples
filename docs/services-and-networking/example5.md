# Create Ingress

## Question

A new payment service has been introduced. Since it is a sensitive application, it is deployed in its own namespace
sensitive-space. Inspect the resources and service created.

You are requested to make the new application available at /pay. Create an ingress resource named ingress-ckad14-pay
for the payment application to make it available at /pay

Identify and implement the best approach to making this application available on the ingress controller and test to
make sure its working. Look into annotations: rewrite-target as well.

## Solution

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-ckad14-pay
  namespace: sensitive-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /pay
            pathType: Prefix
            backend:
              service:
                name: pay-service
                port:
                  number: 8282
```
