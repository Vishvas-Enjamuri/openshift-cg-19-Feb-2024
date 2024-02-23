# Kubernetes User Accounts - Demo

## Create a new normal user

```   
apt install openssl -y 
openssl genrsa -out amit.pem 2048
cat amit.pem 
openssl  req -new -key amit.pem -out amit-csr.pem -subj "/CN=amit/O=training"
   
cat amit-csr.pem 
openssl x509 -req -in amit-csr.pem -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out amit.crt -days 10000

openssl x509 -in amit.crt -text -noout
```

## Set the credentials in kube config
```
oc config view
oc config set-credentials amit --client-certificate=/root/amit.crt --client-key=/root/amit.pem 
oc config view

oc config get-contexts
oc config set-context amit@kubernetes --cluster=kubernetes --user=amit
```

## Check the Credentials
```
oc config view
oc config get-contexts
```

## Create a new context in kube config
```
oc config use-context amit@kubernetes
oc config get-contexts
```

## New let's try to run couple of commands ( if you are getting Forebiden error don't worry its because of missing RBAC roles of the newly created user )  
```
oc get pods 
```


## Switch back to the old context for regular opperations.
```
oc config use-context kubernetes-admin@kubernetes
oc get pods 
```

