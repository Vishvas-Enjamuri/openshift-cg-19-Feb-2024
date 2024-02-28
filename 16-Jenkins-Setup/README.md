## Deploy Jenkins as a Service On Openshift. 

### Step1: Login to Openshift with "Developer" Profile in GUI. 

### Step2: Create a new project named : "jenkins-demo"

### Step3: Click on +Add -> Developer Catalog -> All Services -> Select "CI/CD" from left menu -> Select "Jenkins(Ephemeral)" -> Instantiate Template. 

### Step4: Check the Pod Status of Jenkins from a Topology Section or 

```
oc get pods -n jenkins-demo
```

### Step5: Check the Jenkins Deployment Route & Try to login with Kubeadmin User. 

### Step6: Create a FreeStyle Jenkins Job -> Go to Build Section -> Add Shell -> "date; hostname -f" -> Save -> Click on Build Now.


## Create a Build Config with a strategy type  "JenkinsPipeline" 

### Step1: Login to Openshift with "Developer" Profile in GUI. 

### Step2: Select project named : "jenkins-demo"

### Step3: Go to Build -> Create a new Build Config -> Select Samples from the left hand Menu -> Choose -> S2I Template -> Tryit. 

### Step4: Update the Build Config Sample. 

### Step4.1: Update the Name of Build Config : "cicd"

### Step4.2: Update the Namespace : "jenkins-demo" 

### Step4.3: Update the Git Repo URL [ https://github.com/amitvashisttech/openshift-jenkins-cicd.git ] & Branch [main] 

### Step4.4: Update the Image Steam: 'jenkins-demo-maven:latest'

### Step4.5: Update the strategy: 
```

  strategy:
    type: JenkinsPipeline
    jenkinsPipelineStrategy:
      jenkinsfilePath: Jenkinsfile

```
