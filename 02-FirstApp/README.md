# Creating out First App

## Check the health of Cluster
```
oc get nodes
```

## Deploy Nginx App
```
oc run hello-ocp --image=nginx --port=80
```

## Check the status of PODs
```
oc get pods
oc describe pods hello-ocp
```

```
oc get pods
NAME             READY   STATUS    RESTARTS   AGE
hello-ocp        1/1     Running   0          6m22s
```
