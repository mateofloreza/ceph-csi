
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
