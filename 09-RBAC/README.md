# Kubernetes RBAC 

## Create a Pod Reader Role
```
oc create role pod-reader --verb=get,list,watch --resource=pods --dry-run -o yaml > pod-reader-role.yaml
oc create -f pod-reader-role.yaml
```

## Create a POD Reader RoleBinding 
```
oc create rolebinding --help
oc create rolebinding read-pod-binding --role=pod-reader --user=amit --dry-run -o yaml > read-pod-binding.yaml
oc create -f read-pod-binding.yaml
oc auth can-i get pods --user=amit 
```

## Check the Roles & Context with role Permissions:
```
oc get role
oc get rolebinding 

oc config get-contexts
oc config use-context amit@kubernetes
oc config get-contexts

oc get pods 
oc get pods -n myspace
oc delete pods  helloworld-deployment-6dc57c75b4-8nlmw -n myspace
oc delete deploy  helloworld-deployment -n myspace
```


# Create a ClutserRole Binding with Cluster Admin role for normal user: amit


## View Cluster & Binding via kubernetes-admin context
```
oc config get-contexts
oc config use-context kubernetes-admin@kubernetes
oc config get-contexts
oc get pods -n kube-system

oc get clusterrole
oc describe clusterrole cluster-admin
oc get clusterrolebinding
oc describe clusterrolebinding cluster-admin
```

## Create a Clusterole binding with existing cluster admin role
```
oc create clusterrolebinding admin-user-amit --clusterrole=cluster-admin --user=amit  --dry-run 
oc create clusterrolebinding admin-user-amit --clusterrole=cluster-admin --user=amit  --dry-run -o yaml > amit-cluster-rolebinding.yaml
cat amit-cluster-rolebinding.yaml 

oc create -f amit-cluster-rolebinding.yaml
oc get clusterrolebinding
oc describe clusterrolebinding admin-user-amit
```

## Change the conetext & validate the role permissions
```
oc config get-contexts
oc config use-context amit@kubernetes
oc get pods 
oc get pods --all-namespaces
   
```

