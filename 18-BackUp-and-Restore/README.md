
## Openshift CRC Backup

### Step 1: Login with kubeadmin 

### Step 2: Go inside the master nodes 
```
oc get nodes 
oc debug node/master-x1x1x
chroot /host
```

### Step 3: Create a backup Dir. 
```
mkdir -p /home/coreos/openshift-backup
```

### Step 4: Locate the Per Defined Cluster Backup Script under the path : "/usr/local/bin/"

### Step 5: Excute the BackUp Script
```
./cluster_backup.sh /home/coreos/openshift-backup/
```

### Step 6: Check the Backed Up Configs
```
ls -ltr /home/coreos/openshift-backup/
```



## CRC Restore 

### Step 1: Create some sample app for testing
```
oc new-project nginx-demo-test
```
```
oc run hello-nginx --image=nginx --port=80
```
```
oc expose pod hello-nginx 
```
```
oc expose svc hello-nginx 
```
### Step 2: Go inside the master nodes 
```
oc get nodes 
oc debug node/master-x1x1x
chroot /host
```

### Step 3: Locate the Per Defined Cluster Resoute Script under the path : "/usr/local/bin/" & Last Backup location or Dir.

### Step 6: Excute the Restore Script
```
./cluster_restore.sh /home/coreos/openshift-backup/
```

### Step 6: Check if the "nginx-demo-test" project still exist or not ?
```
oc get all -n nginx-demo-test
```

