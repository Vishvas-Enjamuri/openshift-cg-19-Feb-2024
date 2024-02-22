```
  128  cd 06-Service/

  129  ls
  130  cat 01-helloworld.yaml 
  131  vim 01-helloworld.yaml 
  132  oc  apply -f 01-helloworld.yaml 
  133  oc  get deploy 
  134  oc  get svc 
  135  oc  delete svc helloworld-deployment
  137  oc  get deploy 
  138  oc  get pods 
  139  oc  expose deploy helloworld-deployment
  140  oc  get svc 
  144  oc describe svc helloworld-deployment
  145  oc  get pods -o wide --show-labels
  146  oc describe svc helloworld-deployment
  147  oc  get pods --show-labels
  148  oc describe svc helloworld-deployment
  149  ls
  150  oc  get svc 
  151  oc  delete svc helloworld-deployment
  152  ls
  153  vim 02-helloworld-svc.yaml
  154  oc  apply -f 02-helloworld-svc.yaml 
  155  oc  get svc 
  156  oc describe svc helloworld-service
  157  ls
  158  cat 03-app-svc-deployment.yaml 
  159  oc  apply -f 03-app-svc-deployment.yaml 
  160  vim 03-app-svc-deployment.yaml
  161  oc  apply -f 03-app-svc-deployment.yaml 
  162  oc  get deploy,svc,pods 
  163  oc  get pods 
  164  oc  get deploy,svc,pods 
  165  oc  describe svc python-webapp-svc
  166  oc  get pods -o wide 
  167  oc  describe svc python-webapp-svc
  168  oc exec -it python-webapp-deployment-54b95bd8d-xgsdm -- sh 
  169  oc exec -it python-webapp-deployment-54b95bd8d-xgsdm 
  170  oc exec -it python-webapp-deployment-54b95bd8d-xgsdm -- cat app.py 
  171  oc logs  python-webapp-deployment-54b95bd8d-xgsdm
```
