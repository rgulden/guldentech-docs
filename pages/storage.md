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

## nfs cloud backup

A custom option, is to have GuldenTech provision a PV / PVC pair to a location that is stored on a cloud instance.

If you are interested in this, please open a git issue here ( https://github.com/rgulden/guldentech-docs/issues ) and tag it "Cloud PVC Request". Someone from GuldenTech will reach out and let you know if your use case works for this setup.
