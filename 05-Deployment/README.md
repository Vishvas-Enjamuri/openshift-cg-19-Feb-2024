 ```
   35  oc apply -f helloworld.yaml 
   36  oc  get deploy,rs,pod
   37  oc get pods 
   38  oc  get svc 
   39  oc  get deploy
       oc expose deploy helloworld-deployment --type=NodePort 
   40  cat README.md 
   41  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:2
   42  oc  get deploy 
   43  oc  get deploy,rs,pod
       oc  scale --replicas=5 deploy helloworld-deployment
   44  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:3
   45  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:4
   46  cat helloworld.yaml 
   47  cat README.md 
   48  oc rollout history deploy helloworld-deployment
   49  oc rollout history deploy helloworld-deployment --revision=1
   50  oc rollout history deploy helloworld-deployment --revision=2
   51  oc rollout history deploy helloworld-deployment
   52  oc rollout undo deploy helloworld-deployment
   53  oc rollout history deploy helloworld-deployment
   54  oc rollout undo deploy helloworld-deployment
   55  oc rollout history deploy helloworld-deployment
   56  oc rollout undo deploy helloworld-deployment --to-revision=2
   57  oc rollout history deploy helloworld-deployment
   58  oc rollout undo deploy helloworld-deployment --to-revision=1
   59  oc rollout history deploy helloworld-deployment
   60  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:4 --record 
   61  oc rollout history deploy helloworld-deployment
   62  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:3 --record 
   63  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web:2 --record 
   64  oc set image deployment helloworld-deployment k8s-demo=amitvashist7/k8s-tiny-web --record 
   65  oc rollout history deploy helloworld-deployment
       oc edit deploy helloworld-deployment
       oc get deploy helloworld-deployment -o yaml > abc.yaml    
```
