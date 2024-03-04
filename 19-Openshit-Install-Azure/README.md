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


