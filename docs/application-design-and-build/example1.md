# Create PV & PVC

## Question

Create a persistent volume called cloudstack-pv with the below properties:

- Its capacity should be 128Mi.
- The volume type should be `hostpath` and path should be /opt/cloudstack-pv.

Next, create a persistent volume claim called cloudstack-pvc as per below properties:

- Request 50Mi of storage from cloudstack-pv PV.

## Solution

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: cloudstack-pv
spec:
  capacity:
    storage: 128Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /opt/cloudstack-pv
  storageClassName: manual
```

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: cloudstack-pvc
spec:
  storageClassName: manual
  volumeName: cloudstack-pv # set volumeName to PV
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Mi
```
