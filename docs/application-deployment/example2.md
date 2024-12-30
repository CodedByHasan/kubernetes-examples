# Example 2

## Question

The team has installed multiple helm charts on a different namespace. By mistake, those deployed
resources include one of the vulnerable images called `kodekloud/click-counter:latest`. Find out
the release name and uninstall it.

## Solution

Run the `helm ls` command with `-A` option to list the releases deployed on all the namespaces using helm.

```bash
helm ls -A
```

Use the `jq` tool to extract the image name from the deployments.

```bash
kubectl get deploy -n <NAMESPACE> <DEPLOYMENT-NAME> -o json | jq -r '.spec.template.spec.containers[].image'
```

Replace <NAMESPACE> with the namespace and <DEPLOYMENT-NAME> with the deployment name, which was obtained
from the previous commands.

After finding the `kodekloud/click-counter:latest` image, use the `helm uninstall` to remove the deployed chart
that are using this vulnerable image.

```bash
helm uninstall <RELEASE-NAME> -n <NAMESPACE>
```
