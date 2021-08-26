# Storage

GuldenTech provides two types of storage.

1. local-path storage class
2. Cloud backed up nfs storage (by request)

!> The NFS storage is rather slow, so this is only suggested in certain use cases. Talk with GuldenTech admins for nfs use cases.

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

!> If you use local path, please note that if the node goes down, your pod will not move nodes, it will be in pending state until the node comes back online. Your pod is tied to the node the local-stoage is created on.

##  Cloud storage with NFS

For apps that need 100% uptime, please create a nfs pvc.

!> NFS storage is very slow, please talk with GT admins before leveraging this.

Example:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: nfs
  resources:
    requests:
      storage: 1Gi
```
