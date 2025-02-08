# Troubleshooting Ingress - Example 1

## Question

We have an external webserver running on student-node which is exposed at port 9999.

We have also created a service called `external-webserver-ckad01-svcn` that can connect to our local webserver from
within the cluster3 but, at the moment, it is not working as expected.

Fix the issue so that other pods within cluster3 can use `external-webserver-ckad01-svcn` service to access the webserver.

## Solution

Check if the webserver is working or not:

```bash
curl student-node:9999
...

<h1>Welcome to nginx!</h1>
...
```

Next, check if service is correctly defined:

```bash
kubectl describe svc external-webserver-ckad01-svcn
```

```
Name: external-webserver-ckad01-svcn
Namespace: default
.
.
Endpoints: <none> # there are no endpoints for the service
...
```

There are no endpoints specified for the service, hence we won't be able to get any output. Since we can not destroy
any k8s object, let's create the endpoint manually for this service as shown below:

```bash
export IP_ADDR=$(ifconfig eth0 | grep inet | awk '{print $2}')
```

```bash
kubectl --context cluster3 apply -f - <<EOF
apiVersion: v1
kind: Endpoints
metadata:

# the name here should match the name of the Service

name: external-webserver-ckad01-svcn
subsets:

- addresses: - ip: $IP_ADDR
  ports: - port: 9999
  EOF
```

Check if the curl test works now:

```bash
kubectl --context cluster3 run --rm -i test-curl-pod \
        --image=curlimages/curl --restart=Never -- curl -m 2 external-webserver-ckad01-svcn
...

<title>Welcome to nginx!</title>
```

## Reference

More information about endpoints can be found [here](https://kubernetes.io/docs/concepts/services-networking/service/#endpoints).
