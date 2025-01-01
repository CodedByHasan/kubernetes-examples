# Create CRD - Example 3

## Question

Create a custom resource my-anime of kind Anime with the below specifications:

- Name of Anime: Naruto
- Episode Count: 220

> TIP: You may find the respective CRD with anime substring in it.

## Solution

```bash
kubectl get crd | grep -i anime
```

```
animes.animes.k8s.io
```

```bash
kubectl get crd animes.animes.k8s.io \
                 -o json \
                 | jq .spec.versions[].schema.openAPIV3Schema.properties.spec.properties
```

```
{
  "animeName": {
    "type": "string"
  },
  "episodeCount": {
    "maximum": 300,
    "minimum": 24,
    "type": "integer"
  }
}

```

```bash
k api-resources | grep anime
animes                            an           animes.k8s.io/v1alpha1                 true         Anime
```

cat << YAML | kubectl apply -f -

```yaml
apiVersion: animes.k8s.io/v1alpha1
kind: Anime
metadata:
  name: my-anime
spec:
  animeName: "Naruto"
  episodeCount: 220
```
