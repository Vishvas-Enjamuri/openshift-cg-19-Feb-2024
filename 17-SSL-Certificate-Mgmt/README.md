########################## Certificate ##################################

## API Server Certificate

1. Genrate Openssl Self Sign Key 
```
 openssl genrsa -out ocp-crc-custom-key.pem 2048
``` 

2. Genrate CSR Request with the above key
```
openssl req -new -key ocp-crc-custom-key.pem -out ocp-crc-custom-csr.pem
```

3. Sign the CSR Request with Key
```
openssl x509 -req -days 365 -in ocp-crc-custom-csr.pem -signkey ocp-crc-custom-key.pem -out ocp-crc-custom-crt.pem
```

4. Now Check the Certificate
```
openssl x509 -in ocp-crc-custom-crt.pem -text -noout
```


## Wild Certificate of Ingress Contoller Server Certificate

1. Genrate Openssl Self Sign Key 
```
openssl genrsa -out star-ocp-crc-custom-key.pem 2048
```
2. Genrate CSR Request with the above key
```
openssl req -new -key star-ocp-crc-custom-key.pem -out star-ocp-crc-custom-csr.pem
```

3. Sign the CSR Request with Key
```
openssl x509 -req -days 365 -in star-ocp-crc-custom-csr.pem -signkey star-ocp-crc-custom-key.pem -out star-ocp-crc-custom-crt.pem
```

4. Now Check the Certificate
```
openssl x509 -in star-ocp-crc-custom-crt.pem -text -noout
```


## Patch API Server Cert in CRC Setup to SNO Cluster 

## Create a TLS Secret for API Server

1. Login to the new API as the kubeadmin user.
```
oc login -u kubeadmin -p <password> https://FQDN:6443
```

2.Get the kubeconfig file.
```
oc config view --flatten > kubeconfig-newapi
```

3. Create a secret that contains the certificate chain and private key in the openshift-config namespace.
 
```
oc create secret tls api-secret --cert=ocp-crc-custom-crt.pem --key=ocp-crc-custom-key.pem -n openshift-config
```

4. Now we need to update windows "/etc/hosts" file for out api domain dns resolution: 
4.1 Go to the following path for host file "C:\Windows\System32\drivers\etc", append the existing entry with you api fqdn such "api.ocp-crc.domain.biz" 


```
# Added by CRC
127.0.0.1        jenkins-jenkins-demo.apps-crc.testing sample-app-jenkins-new-jenkins-demo.apps-crc.testing api.crc.testing canary-openshift-ingress-canary.apps-crc.testing console-openshift-console.apps-crc.testing default-route-openshift-image-registry.apps-crc.testing downloads-openshift-console.apps-crc.testing oauth-openshift.apps-crc.testing  api.ocp-training.domain.biz
# End of CRC section

```


5. As we know "servingCerts" Spec is missing in CRC Deploy, hence we need to add the same manually  
```
oc edit apiserver cluster -o yaml
```
```
#### Add the following Lines 
```
spec:
  audit:
    profile: Default
  servingCerts:
    namedCertificates:
    - names:
      - api.ocp-training.domain.biz
      servingCertificate:
        name: api-secret
```


5. Examine the apiserver/cluster object and confirm the secret is now referenced.
```
oc get apiserver cluster -o yaml
```

6. Check the kube-apiserver operator, and verify that a new revision of the Kubernetes API server rolls out. It may take a minute for the operator to detect the configuration change and trigger a new deployment. While the new revision is rolling out, PROGRESSING will report True.
```
oc get clusteroperators kube-apiserver
```

```
NAME             VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE
kube-apiserver   4.14.0     True        False         False      145m


7. Now you can logout & re-login with new api fqdn to test: 
```
PS C:\Users\openshift2> oc logout
Logged "kubeadmin" out on "https://api.crc.testing:6443"
PS C:\Users\openshift2> oc login -u kubeadmin https://api.ocp-training.domain.biz:6443
```

8. Now you can also login to the console with new fqdn URL :  https://api.ocp-training.domain.biz:6443/console




## Patch API Server Cert in Multi Node Cluster 

## Create a TLS Secret for API Server

1. Login to the new API as the kubeadmin user.
```
oc login -u kubeadmin -p <password> https://FQDN:6443
```

2.Get the kubeconfig file.
```
oc config view --flatten > kubeconfig-newapi
```

3. Create a secret that contains the certificate chain and private key in the openshift-config namespace.
 
```
oc create secret tls api-secret --cert=ocp-crc-custom-crt.pem --key=ocp-crc-custom-key.pem -n openshift-config
```

4. Update the API server to reference the created secret.
```
oc patch apiserver cluster --type=merge -p '{"spec":{"servingCerts": {"namedCertificates": [{"names": ["<FQDN>"], "servingCertificate": {"name": "<secret>"}}]}}}' 

oc patch apiserver cluster --type=merge -p '{"spec":{"servingCerts": {"namedCertificates": [{"names": ["api.ocp-crc.domain.biz"], "servingCertificate": {"name": "api-secret"}}]}}}'

```

5. Examine the apiserver/cluster object and confirm the secret is now referenced.
```
oc get apiserver cluster -o yaml
```

6. Check the kube-apiserver operator, and verify that a new revision of the Kubernetes API server rolls out. It may take a minute for the operator to detect the configuration change and trigger a new deployment. While the new revision is rolling out, PROGRESSING will report True.
```
oc get clusteroperators kube-apiserver
```

```
NAME             VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE
kube-apiserver   4.14.0     True        False         False      145m
