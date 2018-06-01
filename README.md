# Instructions

- Git clone project

```bash
git clone https://github.com/snowdrop/ocp-template-build-install.git && cd ocp-template-build-install
```

- Log on to OpenShift and create project

```bash
oc login $(minishift ip):8443 -u admin -p admin
oc new-project ocp-template-build-install
```

- Install the OpenShift template

```bash
oc create -f openshift/application.yaml
```

- Create a new Application from the template

```bash

```
**Steps**

- BuildConfig, ImageStream resources will be first created
- A build will be then started which is a S2I build and the content of the `target` directory uploaded (TO BE UPDATED)

- UnDeploy the application

```bash
oc delete svc,is,bc,dc,route -l app=ocp-template-build-install 
```