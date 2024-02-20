```
 232  mkdir 04-Replication-Cantroller
  233  cd 04-Replication-Cantroller/
  234  vim helloworld-rc.yaml
  235  ls
  236  oc apply -f helloworld-rc.yaml
  237  cat helloworld-rc.yaml
  238  oc get rc
  239  oc describe rc helloworld-controller
  240  oc delete pod helloworld-controller-x569j helloworld-controller-d8mjc
  241  oc describe rc helloworld-controller
  242  oc get rc
  243  oc scale replicas=1 rc helloworld-controller
  244  oc scale --replicas=1 rc helloworld-controller
  245  oc get rc
  246  oc delete pod helloworld-controller-n9n8k
  247  oc scale --replicas=5 rc helloworld-controller
  248  oc delete pod hello-k8s-2
  249  ls
  250  cat helloworld-rc.yaml
  251  oc apply -f helloworld-rc.yaml
  252  oc delete -f helloworld-rc.yaml

```


```
 watch -n .5 oc get pods -o wide
```

     
