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
