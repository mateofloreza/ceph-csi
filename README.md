# Commands to integrate charmed ceph and microk8s
```
kubectl apply -f csidriver.yaml
kubectl apply -f ceph-secret.yaml
kubectl apply -f storageclass.yaml
kubectl apply -f ceph-csi-configmap.yaml
kubectl apply -f ceph-configmap.yaml
kubectl apply -f csi-nodeplugin-rbac.yaml
kubectl apply -f csi-provisioner-rbac.yaml
kubectl apply -f csi-rbdplugin-provisioner.yaml
kubectl apply -f csi-rbdplugin.yaml
```
## Example using pod and PVC

```
kubectl create -f - <<EOF
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: ceph-rbd-sc-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: csi-rbd-sc
---    
apiVersion: v1
kind: Pod
metadata:
  name: testing-ceph
spec:
  containers:
  - name:  testing-ceph
    image: busybox
    command: ["sleep", "infinity"]
    volumeMounts:
    - mountPath: /mnt/ceph_rbd
      name: volume
  volumes:
  - name: volume
    persistentVolumeClaim:
      claimName: ceph-rbd-sc-pvc
EOF
```
Log in testing-ceph pod and download a file into the pvc:

```
kubectl exec -ti testing-ceph -- sh

wget http://releases.ubuntu.com/jammy/ubuntu-22.04.4-live-server-amd64.iso -O /mnt/ceph_rbd/testing.iso
```
Monitor the ceph-mon:

```
ceph osd df
```

