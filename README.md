## Provisioning the Kubernetes cluster

```
$ vagrant up --provider libvirt
```
### Copy the kubeconfig file from kmaster
Password for root user is _kubeadmin_
```
$ mkdir ~/.kube
$ scp root@172.16.16.100:/etc/kubernetes/admin.conf ~/.kube/config
```
### Shutdown the cluster
```
$ vagrant halt
```

### Restart the cluster
```
$ vagrant up
```

### Destroy the cluster
```
$ vagrant destroy -f
```

### Create token for the dashboard
```
$ kubectl -n kubernetes-dashboard create token admin-user
```

## Deploying Add Ons
### Deploy dynamic nfs volume provisioning
```
$ cd misc/nfs-subdir-external-provisioner
$ cat setup_nfs | vagrant ssh kmaster
$ cat setup_nfs | vagrant ssh kworker1
$ cat setup_nfs | vagrant ssh kworker2
$ cat setup_nfs | vagrant ssh kworker3
$ kubectl create -f 01-setup-nfs-provisioner.yaml

###### for testing
$ kubectl create -f 02-test-claim.yaml
$ kubectl delete -f 02-test-claim.yaml
```
### Deploy metalLB load balancing
```
$ cd misc/metallb
$ kubectl create -f 01_metallb.yaml

###### wait for 10 seconds or so for the pods to run
$ kubectl create -f 02_metallb-config.yaml

###### for testing
$ kubectl create -f 03_test-load-balancer.yaml

#### to check the port and ip address of the services exposed.
kubectl get services

#### To access the nginx via load balancer.
http://172.16.16.230/

$ kubectl delete -f 03_test-load-balancer.yaml
```


