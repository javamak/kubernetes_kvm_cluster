
### Create the pods for the dashboard
```
$ kubectl apply -f dashboard/dashboard.yaml
```

### Create the admin-user service account with role binind
```
$ kubectl apply -f dashboard/service-account.yml
$ kubectl apply -f dashboard/cluster-role-binding.yml
```


### Create token for the admin-user
```
$ kubectl -n kubernetes-dashboard create token admin-user
```