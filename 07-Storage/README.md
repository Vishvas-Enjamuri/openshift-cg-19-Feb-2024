#Set up persistent volumes anywhere

### Begin by installing the NFS Provisioner Operator:

# Login
```
oc login -u kubeadmin -p kubeadmin-x1 https://api.crc.testing:6443
```

# Create a new namespace
```
oc new-project nfsprovisioner-operator

```
# Deploy NFS Provisioner operator in the terminal (You can also use OpenShift Console
```
cat << EOF | oc apply -f -
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: nfs-provisioner-operator
  namespace: openshift-operators
spec:
  channel: alpha
  installPlanApproval: Automatic
  name: nfs-provisioner-operator
  source: community-operators
  sourceNamespace: openshift-marketplace
EOF

```

# Check nodes
```
oc get nodes
NAME                 STATUS   ROLES           AGE   VERSION
crc-8rwmc-master-0   Ready    master,worker   54d   v1.22.3+e790d7f

```
# Set Env variable for the target node name
```
export target_node=$(oc get node --no-headers -o name|cut -d'/' -f2)
oc label node/${target_node} app=nfs-provisioner

```
# ssh to the node
```
oc debug node/${target_node}
```

# Create a directory and set up the Selinux label.
```
$ chroot /host
$ mkdir -p /home/core/nfs
$ chcon -Rvt svirt_sandbox_file_t /home/core/nfs
$ exit; exit
```

# Create NFSProvisioner Custom Resource
```
cat << EOF | oc apply -f -
apiVersion: cache.jhouse.com/v1alpha1
kind: NFSProvisioner
metadata:
  name: nfsprovisioner-sample
  namespace: nfsprovisioner-operator
spec:
  nodeSelector:
    app: nfs-provisioner
  hostPathDir: "/home/core/nfs"
EOF
```

# Check if NFS Server is running
```
oc get pod
NAME                               READY   STATUS    RESTARTS   AGE
nfs-provisioner-77bc99bd9c-57jf2   1/1     Running   0          2m32s
```
# Create a test PVC

# Check the test PV/PVC
oc get pv, pvc
```
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                                                 STORAGECLASS   REASON   AGE
persistentvolume/pvc-e30ba0c8-4a41-4fa0-bc2c-999190fd0282   1Mi        RWX            Delete           Bound       nfsprovisioner-operator/nfs-pvc-example               nfs                     5s

NAME                                    STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/nfs-pvc-example   Bound    pvc-e30ba0c8-4a41-4fa0-bc2c-999190fd0282   1Mi        RWX
```
