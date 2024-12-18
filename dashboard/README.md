
### Create the pods for the dashboard
```
$ kubectl apply -f dashboard/dashboard.yaml
```

### Create the admin-user service account with role binind
```
$ kubectl apply -f dashboard/service-account.yml
$ kubectl apply -f dashboard/cluster-role-binding.yml
```

### Accessing the dashboard.

`http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/workloads?namespace=default`


### Create token for the admin-user
```
$ kubectl -n kubernetes-dashboard create token admin-user
```

