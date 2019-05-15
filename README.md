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

- A. Create a new Application from the template and passing as parameter the **git repo** to be built within the docker container using maven

  ```bash
  oc new-app spring-boot-rest-http \
    -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/ocp-template-build-install.git
  ```

- B. Same as before but **pushing** the local project as a binary stream

  ```bash
  oc delete svc,is,bc,dc,route,template spring-boot-rest-http
  oc delete is runtime
  oc create -f openshift/from-dir-s2i-build.yaml
  oc new-app spring-boot-rest-http
  
  oc start-build spring-boot-rest-http --from-dir=. --follow
  ```

  **Remark** : Command to be used to create the buildconfig is `oc new-build --docker-image=fabric8/s2i-java:latest --binary=true --strategy=source --to=spring-boot-rest-http:1.5.13-1`

- C. Using a Pipeline - Jenkinsfile as Source strategy to delegate the responsibility of the build on the cloud platform

  ```bash
  oc delete svc,is,bc,dc,route,template spring-boot-rest-http
  oc delete is runtime
  oc create -f openshift/from-jenkinsfile-s2i-build.yaml
  
  oc new-app spring-boot-rest-http \
    -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/ocp-template-build-install.git 
  ```

  **Steps**
  
  - The following resources are created from an OpenShift template when a new application is created :
    - BuildConfig, ImageStream (s2i and application), Service, Route and DeploymentConfig
  - A build will be then started from either :
    - Git repository
    - Source code pushed as binary stream
    - Jenkinsfile
  - In both cases, the S2I image will do a `mvn package` and the uber jar file generated will then be pushed to the Docker's applicaiyon image
  
- UnDeploy the application

  ```bash
  oc delete svc,is,bc,dc,route,template spring-boot-rest-http
  oc delete is runtime
  ```
  
  
<fabric8.generator.from>fabric8/s2i-java:latest</fabric8.generator.from> 