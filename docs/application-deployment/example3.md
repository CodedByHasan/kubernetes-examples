# Upgrade Helm Chart

## Question

One co-worker deployed an nginx helm chart on the cluster3 server called `lvm-crystal-apd`. A new update is pushed
to the helm chart, and the team wants you to update the helm repository to fetch the new changes.

After updating the helm chart, upgrade the helm chart version to above 13.2.9 and increase the replica count to 2.

> NOTE: We have to perform this task on the cluster3-controlplane node.

## Solution

Log in to the cluster3-controlplane node first and use the helm ls command to list all the releases installed using
Helm in the Kubernetes cluster.

```bash
helm ls -A
```

Identify the namespace where the resources get deployed.

Use the helm repo ls command to list the helm repositories.

```bash
helm repo ls
```

Now, update the helm repository with the following command:

```bash
helm repo update lvm-crystal-apd -n crystal-apd-ns
```

The above command updates the local cache of available charts from the configured chart repositories.

The helm search command searches for all the available charts in a specific Helm chart repository. In our
case, it's the nginx helm chart.

```bash
helm search repo lvm-crystal-apd/nginx -n crystal-apd-ns -l | head -n30
```

The -l or --versions option is used to display information about all available chart versions.

Upgrade the helm chart to above 13.2.9 and also, increase the replica count of the deployment to 2 from the
command line. Use the helm upgrade command as follows:

```bash
helm upgrade lvm-crystal-apd lvm-crystal-apd/nginx -n crystal-apd-ns --version=13.2.12 --set replicaCount=2
```

After upgrading the chart version, you can verify it with the following command:

```bash
helm ls -n crystal-apd-ns
```

Look under the CHART column for the chart version. Use the kubectl get command to check the replicas of the deployment:

```bash
kubectl get deploy -n crystal-apd-ns
```

The available count 2 is under the AVAILABLE column.
