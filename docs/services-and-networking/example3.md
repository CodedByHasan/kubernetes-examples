# Example 3

## Question

There are several applications in the `ns-ckad17-svcn` namespace that are exposed inside the cluster via ClusterIP.
Create a [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) type service
that will serve traffic to the applications based on its labels. Create the resources as follows:

- Service `lb1-ckad17-svcn` for serving traffic at port 31890 to pods with labels `"exam=ckad, criteria=location"`.
- Service `lb2-ckad17-svcn` for serving traffic at port 31891 to pods with labels `"exam=ckad, criteria=cpu-high"`.

## Solution

### Find pods with specified labels

To create the load balancer for the pods with the specified labels, find the pods with the mentioned labels:

```bash
kubectl -n ns-ckad17-svcn get pod -l exam=ckad,criteria=location
```

```bash
kubectl -n ns-ckad17-svcn get pod -l exam=ckad,criteria=cpu-high
```

### Create LoadBalancer Service

After identifying target pods, create load balancer service using the following commands:

```bash
kubectl -n ns-ckad17-svcn expose pod <target-pod-a>
        --type=LoadBalancer
        --name=lb1-ckad17-svcn
```

```bash
kubectl -n ns-ckad17-svcn expose pod <target-pod-b>
        --type=LoadBalancer
        --name=lb2-ckad17-svcn
```

Once deployed, edit service to use specified node port.
