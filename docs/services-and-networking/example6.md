# Create Network Policy with Namespace Restrictions

## Question

We have deployed some pods in the namespaces ckad-alpha and `ckad-beta`.

You need to create a NetworkPolicy named `ns-netpol-ckad` that will restrict all Pods in namespace `ckad-alpha` to only
have outgoing traffic to Pods in namespace `ckad-beta` . Ingress traffic should not be affected.

However, the NetworkPolicy you create should allow egress traffic on port 53 TCP and UDP.

## Solution

The following manifest will restrict all the pods in `ckad-alpha` namespace to only have egress traffic to pods in
namespace `ckad-beta`. But it will allow egress on port 53.

```yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ns-netpol-ckad
  namespace: ckad-alpha
spec:
  podSelector: {}
  policyTypes:
    - Egress
  egress:
    - ports:
        - port: 53
          protocol: TCP
        - port: 53
          protocol: UDP
    - to:
        - namespaceSelector:
            matchLabels:
              kubernetes.io/metadata.name: ckad-beta
```
