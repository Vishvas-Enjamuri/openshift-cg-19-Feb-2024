## User Management in Openshift 

# Grab the Current Auth Secrets 

```
oc get secret htpass-secret -n openshift-config -o yaml > openshift-passwrd.yaml 
```

# Copy the string of htpassword & decode the base64. 

# Online Decoder : https://www.base64decode.org/

# Now you you must have a decoded string like the following : 
```
developer:$2a$10$o5u4N1q82yDtHBRmQ6eYk.oZR2.fPE1/KC71yo.NmDWhQgXTgoj/a
kubeadmin:$2a$10$cXuhbz5LHd5VyFA/C5McBO5fD5h4z.SgjObZfrtZ/e/B6uY3WRw9G
```
# Now make a copy of developer's creds & pasted it for your users ( amit & vashist ) 
```
developer:$2a$10$o5u4N1q82yDtHBRmQ6eYk.oZR2.fPE1/KC71yo.NmDWhQgXTgoj/a
kubeadmin:$2a$10$cXuhbz5LHd5VyFA/C5McBO5fD5h4z.SgjObZfrtZ/e/B6uY3WRw9G
amit:$2a$10$o5u4N1q82yDtHBRmQ6eYk.oZR2.fPE1/KC71yo.NmDWhQgXTgoj/a
vashist:$2a$10$o5u4N1q82yDtHBRmQ6eYk.oZR2.fPE1/KC71yo.NmDWhQgXTgoj/a
```

# Copy the above string and encode the same in base64. 

# Online encoder : https://www.base64encode.org/

# Now update the htpasswd section in your openhshift-password.yaml file. 

# Apply the openshift-password file. 
```
oc apply -f openshift-passwrd.yaml 
```

# Now login with one of the newly created user: 
```
oc login -u <newuser> -p developer https://api.crc.testing:6443
```

## New let's try to run couple of commands ( if you are getting Forebiden error don't worry its because of missing RBAC roles of the newly created user )
```
oc get pods
```

