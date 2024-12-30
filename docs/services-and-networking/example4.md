# Troubleshooting Ingress

## Question

There is an application deployed in the green-space namespace. There is also an ingress controller
and the ingress resource deployed.

However, the ingress controller is not working as expected. Inspect the ingress definitions
and troubleshoot the issue so that the services are accessible as per the ingress resource definition.

Also, update the path for the app-wear-service to `/app-wear` and app-video-service to `/app-video`.

> Note: You are allowed to edit or delete the resources related to ingress but do NOT change the pods.

## Solution

Check the status of the ingress, pods, and application related services.

```bash
kubectl get pods -n ingress-nginx

NAME READY STATUS RESTARTS AGE
ingress-nginx-admission-create-l6fgw 0/1 Completed 0 11m
ingress-nginx-admission-patch-sfgc4 0/1 Completed 0 11m
ingress-nginx-controller-5f8964959d-278rc 0/1 Error 2 (26s ago) 29s
```

You would see an Error or CrashLoopBackOff in the ingress-nginx-controller. Inspect the logs of the controller pod.

```bash
k logs -n ingress-nginx ingress-nginx-controller-5f8964959d-278rc

---

F0316 08:03:28.111614 57 main.go:83] No service with name default-backend-service found in namespace default:

---
```

You see an error msg saying _"No service with name default-backend-service found in namespace default"_.

There is no service with that name in the default namespace. Edit the ingress controller deployment
to use the service that we have i.e. default-backend-service in the green-space namespace.

To create the controller deployment with correct backend service, first save the deployment in a file,
delete the controller deployment, edit the file and create the deployment.

Save the deployment in file

```bash
kubectl get -n ingress-nginx deployments.apps ingress-nginx-controller -o yaml >> ing-control.yaml
```

```bash
Delete the deployment.
kubectl delete -n ingress-nginx deploy ingress-nginx-controller
```

Edit the file to match the correct service.

```
spec:
containers: - args: - /nginx-ingress-controller - --publish-service=$(POD_NAMESPACE)/ingress-nginx-controller
          - --election-id=ingress-controller-leader
          - --watch-ingress-without-class=true
          - --default-backend-service=green-space/default-backend-service   # Changed to correct namespace
          - --controller-class=k8s.io/ingress-nginx
          - --ingress-class=nginx
          - --configmap=$(POD_NAMESPACE)/ingress-nginx-controller - --validating-webhook=:8443 - --validating-webhook-certificate=/usr/local/certificates/cert - --validating-webhook-key=/usr/local/certificates/key
```

Apply the manifest, it should be up and running.
