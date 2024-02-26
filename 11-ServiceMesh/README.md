## Service Mesh Deployment is resource intsive 
## RAM : 28G
## CPU : 6 vCPU 

## Install Redhat Service Mesh Operator Guide: 

### We need to Install Couple of Operators [ Opshift ElasticSearch, Jaeger, Kiali, Redhat Service Mesh

### Step1: Login with kubeadmin Credentials 

### Step2: Navigate to Operators -> OperatorHub -> Install the above menstion Operators for all namespaces:

### Step3: Deploy the control plane form the web console 

### Step3.1: Login with kubeadmin 
### Step3.2: Create new project name: istio-system 
### Step3.3: Navigate to Operators and select istio-system project -> Install the following components: 
### Step3.3.1 : Select the service mesh operators -> [ ServiceMesgContolPlane ] Resource
### Step3.3.2 : Select the service mesh operators -> [ ServiceMesgMemberRoll ] Resource
### Step 4 : oc get all -n istio-system 
```
NAME                                     READY   STATUS             RESTARTS   AGE
grafana-7bf5764d9d-2b2f6                 2/2     Running            0          28h
istio-citadel-576b9c5bbd-z84z4           1/1     Running            0          28h
istio-egressgateway-5476bc4656-r4zdv     1/1     Running            0          28h
istio-galley-7d57b47bb7-lqdxv            1/1     Running            0          28h
istio-ingressgateway-dbb8f7f46-ct6n5     1/1     Running            0          28h
istio-pilot-546bf69578-ccg5x             2/2     Running            0          28h
istio-policy-77fd498655-7pvjw            2/2     Running            0          28h
istio-sidecar-injector-df45bd899-ctxdt   1/1     Running            0          28h
istio-telemetry-66f697d6d5-cj28l         2/2     Running            0          28h
jaeger-896945cbc-7lqrr                   2/2     Running            0          11h
kiali-78d9c5b87c-snjzh                   1/1     Running            0          22h
prometheus-6dff867c97-gr2n5              2/2     Running            0          28h


```

### Step 5: Create new project with name ( bookinfo), where you want istio injection:
### Step 6: Navigate to Operators and select istio-system project -> Redhat Openshift Service Mesh -> Edit -> ServiceMeshMemberRoll -> select default -> add project memeber as "bookinfo"
### Step 7: Now you can try create a pod with the following files: 
```
oc apply -f 01-hello-nginx-deploy.yaml
```

```
oc apply -f 02-hello-nginx-deploy-with-injection.yaml
```

### Step 8: BookInfo Deployment: 
```
 oc get smmr -n istio-system -o wide
```

```
oc apply -n bookinfo -f 03-bookinfo.yaml
```

```
oc apply -n bookinfo -f 04-bookinfo-gateway.yaml
```

```
oc apply -n bookinfo -f 05-destination-rule-all.yaml
```

```
oc get pods -n bookinfo
```
```
NAME                              READY   STATUS    RESTARTS   AGE
details-v1-55b869668-jh7hb        2/2     Running   0          12m
productpage-v1-6fc77ff794-nsl8r   2/2     Running   0          12m
ratings-v1-7d7d8d8b56-55scn       2/2     Running   0          12m
reviews-v1-868597db96-bdxgq       2/2     Running   0          12m
reviews-v2-5b64f47978-cvssp       2/2     Running   0          12m
reviews-v3-6dfd49b55b-vcwpf       2/2     Running   0          12m
```

```
oc get route -n istio-system 
```

```
curl -vv http://$INGRESS_GATEWAY_ROUTE/productpage
```
