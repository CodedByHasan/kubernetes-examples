# Example 1

## Question

Define a Kubernetes Custom Resource Definition (CRD) for a new resource kind called Foo (plural form - foos)
in the samplecontroller.k8s.io group.

This CRD should have a version of `v1alpha1` with a schema that includes two properties as given below:

- `deploymentName` (a string type)
- `replicas` (an integer type with minimum value of 1 and maximum value of 5).

It should also include a status subresource which enables retrieving and updating the status of Foo object,
including the `availableReplicas` property, which is an integer type. The Foo resource should be namespace scoped.

## Solution

```yaml
---
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: foos.samplecontroller.k8s.io
  annotations:
    "api-approved.kubernetes.io": "unapproved, experimental-only"
spec:
  group: samplecontroller.k8s.io
  scope: Namespaced
  names:
    kind: Foo
    plural: foos
  versions:
    - name: v1alpha1
      served: true
      storage: true
      schema:
        # schema used for validation
        openAPIV3Schema:
          type: object
          properties:
            spec:
              # Spec for schema goes here !
              type: object
              properties:
                deploymentName:
                  type: string
                replicas:
                  type: integer
                  minimum: 1
                  maximum: 5
            status:
              type: object
              properties:
                availableReplicas:
                  type: integer
      # subresources for the custom resource
      subresources:
        # enables the status subresource
        status: {}
```
