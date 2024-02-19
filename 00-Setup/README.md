## Openshift CRC Setup


## Step 1: Rest the Openshift User Name & Password 
```
-> Control Panel -> User Accounts -> User Accounts -> Manage Accounts -> Add an Account
Select  User - Openshit
change account name : openshift
change password : Password@1234

-> Control Panel -> RDP Setting -> add Opeshift user. 
```

## Step 2: Download Openshift CRC installation & Run the same, also copy the pull seccrets. 


## Step 3: Login with Openshift User, as Openshift CRC installation is allowed to run from a regular User. 

## Step 3: Open Terminal & Start CRC. 

```
crc.exe setup
crc.exe status 
crc.exe start 
```

## Step 4: Wait for the Openshift CRC to boots up, once done you will able to the below message: 
```
Started the OpenShift cluster.

The server is accessible via web console at:
  https://console-openshift-console.apps-crc.testing

Log in as administrator:
  Username: kubeadmin
  Password: pxwGf-eejhe-GGfAI-rSMPL

Log in as user:
  Username: developer
  Password: developer

Use the 'oc' command line interface:
  PS> & crc oc-env | Invoke-Expression
  PS> oc login -u developer https://api.crc.testing:6443
``` 
