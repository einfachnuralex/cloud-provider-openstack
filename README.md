# cloud-provider-openstack-sample


K8s version 1.17.0, provider version 1.17

Create cloud-config
```
[Global]
region=RegionOne
username=username
password=supersecret
auth-url=https://authurl
tenant-id=2o34ijwfhw9susdhswww
domain-id=default
ca-file=/etc/kubernetes/ca.pem

[LoadBalancer]
subnet-id=596c132e-2140-4d7b-86a8-6f35e5be949b
use-octavia=true

[BlockStorage]
bs-version=auto
```

Install k8s with kubeadm
```
kubeadm init --config=kubeadm-config.yaml
```

Create secret from file
```
kubectl create secret -n kube-system generic cloud-config --from-literal=cloud.conf="$(cat cloud-config)"
```

Replace Cluster name
```
grep -rl test1 * | xargs sed -i 's/test1/CLUSTER_NAME/g'
```

Add Openstack cloud controller
```
kubectl apply -f ./cloud-provider-openstack/
```

Add cinder csi controller & nodeplugin
```
kubectl apply -f ./cinder-csi/
```

Inspired by
https://kubernetes.io/blog/2020/02/07/deploying-external-openstack-cloud-provider-with-kubeadm/

