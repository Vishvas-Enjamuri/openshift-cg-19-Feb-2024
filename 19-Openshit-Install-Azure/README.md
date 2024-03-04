### Openshift 4.14 Installation on Azure Platform

#### 0. Create a Service Principal

#### Please follow the below steps inorder to setup Azure Account.!
#### 1. Open Azure Cloud Shell - Bash Prompt

##### Now Create Azure Contributor Service Principal for terraform Auth.
```
# az login
```

```
# az account list -o table
```

```
# az account set -s "<subscription-id>"
```

```
# az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```


#### 1. Create a Resource Group: "ocp-azure" 

#### 2. Create a DNS Zone in the above Resource Group as : "ocp-azure.com"

#### 3. Create a Jumpbox Virtual Machine in the above Resource Group : "OS -> CentOS or Ubuntu or anylinux" 

#### 4. Login to Jumpbox & Become Supper User: 

#### 5. Download Openshift Client & Installer Utility: 
```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.14.0/openshift-client-linux-4.14.0.tar.gz
tar -zxvf openshift-client-linux-4.14.0.tar.gz ; mv oc kubectl /usr/local/bin/ ; rm -rf openshift-client-linux-4.14.0.tar.gz
```

```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.14.0/openshift-install-linux-4.14.0.tar.gz
tar -zxvf openshift-install-linux-4.14.0.tar.gz; mv openshift-install /usr/local/bin/; rm -rf openshift-install-linux-4.14.0.tar.gz
```

#### 6. Create SSH Key and set up an SSH agent.
```
ssh-keygen -b 4096 -t rsa -N ‘’ -f ~/.ssh/id_rsa
```

#### 7. Create a Installer Directory as "/root/openshift"
```
mkdir -p /root/openshift"
```

#### 8. Generate Initial Igination file: 
```
openshift-install create install-config --dir ~/openshift --log-level=debug |& tee install-config.log
```
#### 9. Create at least one dedicated subnet for ocp nodes in the existing vnet:
```
- ocp-ctl [10.1.0.0/24]
```

#### 10. During the above command, we need to answer the following questions: 
```
- Select the path of your SSH key
- Select “Azure” as your platform
- Enter Azure subscription id
- Enter azure tenant id
- Enter azure service principal client id
- Enter azure service principal client secret
- Select Region
- Select Base Domain
- Enter Cluster Name in lower case
- Enter Pull Secret
```

#### 11. As we have create a dedicated subnet for our ocp deployment, its time to update the same in install-config.yaml as following: [ networkResourceGroupName, virtualNetwork, controlPlaneSubnet, computeSubnet ]
```
platform:
  azure:
    baseDomainResourceGroupName: ocp-azure
    cloudName: AzurePublicCloud
    outboundType: Loadbalancer
    region: centralindia
    networkResourceGroupName: 
    virtualNetwork: 
    controlPlaneSubnet: 
    computeSubnet: 
```

#### 12. Start the installation procedure & it will take around 40 to 50 mins: 
```
openshift-install create cluster --dir ./ --log-level=debug |& tee create-cluster.log 
```

#### 13. Post Installation, we'll get a following message:  

```
level=info msg=To access the cluster as the system:admin user when using 'oc', run 'export KUBECONFIG=/root/openshift/auth/kubeconfig'
level=info msg=Access the OpenShift web-console here: https://console-openshift-console.apps.ocp-4-14-clust.ocp-azure.com
level=info msg=Login to the console with user: "kubeadmin", and password: "6EF7s-RNNXw-Gc9Vt-nwKnR"
level=debug msg=Time elapsed per stage:
level=debug msg=              vnet: 12m44s
level=debug msg=         bootstrap: 2m45s
level=debug msg=           cluster: 4m34s
level=debug msg=Bootstrap Complete: 21m41s
level=debug msg=               API: 16s
level=debug msg= Bootstrap Destroy: 1m30s
level=debug msg= Cluster Operators: 14m10s
level=info msg=Time elapsed: 58m18s
```

#### 14. Now try to open openshift console: 
```
curl -k https://console-openshift-console.apps.ocp-4-14-clust.ocp-azure.com
```

#### 15. In Case the above command didn't respond: 

#### 15.1 Check the DNS Resolution: 
```
nslookup console-openshift-console.apps.ocp-4-14-clust.ocp-azure.com
```

#### 15.2 Check the Port Connectivity 
```
telnet console-openshift-console.apps.ocp-4-14-clust.ocp-azure.com 443
```

#### In case Telent Fails, then check the subnet & security group binding or assocation. 

#### Go to VNET -> Select Jumpbox Subnet -> Select the Secutity group from drop down as Jumpbox SG. 
#### Go to VNET -> Select Ocp-ctl Subnet -> Select the Secutity group from drop down as ocp-xx11-y1 SG.

#### Now update the Jumpbox SG Group for Inbound Traffice Rules for Port [80,443] 


#### 16. Now try to open openshift console, it should work. 
```
curl -k https://console-openshift-console.apps.ocp-4-14-clust.ocp-azure.com
```

#### 17. In case you want to access the console outside Azure Nodes, such your own laptop then we need to update our local resolver for the same : "/etc/hosts"
```

20.23.210.11 console-openshift-console.apps.ocp14-4.ocp-azure.com *.apps.ocp14-4.ocp-azure.com oauth-openshift.apps.ocp14-4.ocp-azure.com
```

