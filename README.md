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
oc create -f openshift/from-git-s2i-build.yaml
```

- Create a new Application from the template and passing as parameter the git repo to be built within the docker container using maven

```bash
oc new-app spring-boot-rest-http \
  -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/ocp-template-build-install.git
```

- Same as before but pushing the local project as a binary stream

```bash
oc delete svc,is,bc,dc,route,template spring-boot-rest-http
oc delete is runtime
oc create -f openshift/from-dir-s2i-build.yaml
oc new-app spring-boot-rest-http

```

**Remark** : Command to be used to create the buildconfig is `oc new-build --docker-image=fabric8/s2i-java:latest --binary=true --strategy=source --to=spring-boot-rest-http:1.5.13-1
`

**Steps**

- BuildConfig, ImageStream resources will be first created
- A build will be then started which is a S2I build and the content of the `target` directory uploaded (TO BE UPDATED)

- UnDeploy the application

```bash
oc delete svc,is,bc,dc,route,template spring-boot-rest-http
oc delete is runtime
oc delete is s2i-java
```