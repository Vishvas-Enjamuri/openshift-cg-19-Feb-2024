1. Scale Out Openshift Worker Node. -> Login as Kubeadmin -> Admin Profile -> Compute -> MachineSet -> Select one of the machineset & increase the machine count +1. 

2. Now check the node status
```
root@jumpbox:~/openshift-cg-19-Feb-2024/05-Deployment# oc get nodes    --kubeconfig=/root/openshift/auth/kubeconfig
NAME                                              STATUS   ROLES                  AGE     VERSION
ocp-4-14-clust-txszr-master-0                     Ready    control-plane,master   78m     v1.27.6+f67aeb3
ocp-4-14-clust-txszr-master-1                     Ready    control-plane,master   79m     v1.27.6+f67aeb3
ocp-4-14-clust-txszr-master-2                     Ready    control-plane,master   75m     v1.27.6+f67aeb3
ocp-4-14-clust-txszr-worker-centralindia1-ktgg8   Ready    worker                 9m10s   v1.27.6+f67aeb3
ocp-4-14-clust-txszr-worker-centralindia2-nmmhj   Ready    worker                 62m     v1.27.6+f67aeb3
ocp-4-14-clust-txszr-worker-centralindia3-96qzz   Ready    worker                 7m42s   v1.27.6+f67aeb3
root@jumpbox:~/openshift-cg-19-Feb-2024/05-Deployment#
```

2. Create a python web app deployment 
```
oc apply -f py-deplyment-and-svc.yaml
```

3. Create a Route for the service 
```
oc expose svc python-webapp-svc
```

4. Scale the deployment with 10 replicas
```
oc scale --replicas=10 deploy python-webapp-deployment   --kubeconfig=/root/openshift/auth/kubeconfig 
```

5. Bring the Out Node form OCP Cluster
```
oc adm drain <nodename>  --ignore-daemonsets --delete-local-data --kubeconfig=/root/openshift/auth/kubeconfig 
```
```
oc delete nodes <nodename>  --kubeconfig=/root/openshift/auth/kubeconfig
```

6. Update the Machine Set Count to 0. 
