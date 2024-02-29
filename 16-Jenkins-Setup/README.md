## Deploy Jenkins as a Service On Openshift. 

#### Step1: Login to Openshift with "Developer" Profile in GUI. 

#### Step2: Create a new project named : "jenkins-demo"

#### Step3: Click on +Add -> Developer Catalog -> All Services -> Select "CI/CD" from left menu -> Select "Jenkins(Ephemeral)" -> Instantiate Template. 

#### Step4: Check the Pod Status of Jenkins from a Topology Section or 

```
oc get pods -n jenkins-demo
```

#### Step5: Check the Jenkins Deployment Route & Try to login with Kubeadmin User. 

#### Step6: Create a FreeStyle Jenkins Job -> Go to Build Section -> Add Shell -> "date; hostname -f" -> Save -> Click on Build Now.


## Create a Build Config with a strategy type  "JenkinsPipeline" 

#### Step1: Login to Openshift with "Developer" Profile in GUI. 

#### Step2: Select project named : "jenkins-demo"

#### Step3: Go to Build -> Create a new Build Config -> Select Samples from the left hand Menu -> Choose -> S2I Template -> Tryit. 

#### Step4: Update the Build Config Sample. 

#### Step4.1: Update the Name of Build Config : "cicd"

#### Step4.2: Update the Namespace : "jenkins-demo" 

#### Step4.3: Update the Git Repo URL [ https://github.com/amitvashisttech/openshift-jenkins-cicd.git ] & Branch [main] 

#### Step4.4: Update the Image Steam: 'jenkins-demo-maven:latest'

#### Step4.5: Update the strategy: 
```

  strategy:
    type: JenkinsPipeline
    jenkinsPipelineStrategy:
      jenkinsfilePath: Jenkinsfile

```



#### Step 5: Check the CICD Build Job 

#### Step 6: Click on view logs -> you will be redirected to jenkins -> check the logs -> In the logs you probably notices it stuck & looking for maven-xx1z agent to be running. 

#### Step 7: Most likely Maven Agent Pod is not able to pull the image from internal openshift registry or repository: 

#### Step 8: To be sure which image its try to pull - check the pod logs or go inside jenkins pod & look for config.xml. 
```
grep -i maven /var/lib/jenkins/config.xml
```
```
sh-4.4$ grep -i maven /var/lib/jenkins/config.xml
          <id>maven-50e53b0fd012e7ade37fd375f47db620</id>
          <name>maven</name>
          <label>maven</label>
              <image>image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven:latest</image>
sh-4.4$ 


```
#### Step 9: Due to the imagepull back off error, we come to know that "Jenkins-agent-maven" image is missing in our internal openshift registory

#### Step 10: Now we need to download "Jenkins-agent-maven" image & publish the same in our internal openshift registory & for the same we need to follow below steps: 


#### Step 10.1 : We need to login to our master node in dehub host mode: 
```
oc get nodes 
```
```
oc debug node/<masternodename>
```
```
chroot /host
```

#### Step 10.2 : Check the existing & download the "Jenkins-agent-maven" image from external Repo : Quay.io

```
crictl images 
```

```
crictl pull quay.io/openshift/origin-jenkins-agent-maven
```

#### Step 10.3: Tag the download image to point to internal openshift registory
```
podman tag quay.io/openshift/origin-jenkins-agent-maven:latest image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven:latest
```

#### Step 10.4: Authenticate with Internal Openshift Registory
```
oc whoami
```
```
oc whoami --show-token
```

#### Step 10.5: Push the image into internal openshift registory
```
podman login -u kubeadmin -p <kubeadmin-user-token>  image-registry.openshift-image-registry.svc:5000
```

```
podman push  image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven:latest
```

#### Step 11: Now go to the GUI & Check if the job is normalize or not, in case not cancel the current build & retrigger new build with same build config. 

