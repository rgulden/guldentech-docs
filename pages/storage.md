# Storage

GuldenTech provides two types of storage.

1. local-path storage class
2. Cloud backed up nfs storage (by request)

## local-path

Local path creates a pvc that is linked to a folder on the server, each time the pod starts it will be tied to that node. To use this storage class, refrence the example below.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: local-path
  resources:
    requests:
      storage: 1Gi
```

!> All these local paths are backed up once a day to a seperate HD.

##  block-storage with longhorn

For apps that need 100% uptime, please create a longhorn pvc. The data will be replicated across nodes, so pods are not bound to the nodes their pvcs exist on.

Example:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: longhorn
  resources:
    requests:
      storage: 1Gi
```
