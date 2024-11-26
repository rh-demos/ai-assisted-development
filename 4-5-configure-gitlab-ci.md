# Configure Gitlab CI

Create an access token for gitlab from the sonarqube page, name is `gitlab`, type is `global`.

![image-20241030215500079](assets/4-5-configure-gitlab-ci/image-20241030215500079.png)

Create the `SONAR_HOST_URL` variable in the global CICD configuration item of the gitlab page, pointing to the sonarqube URL

![image-20241030215558441](assets/4-5-configure-gitlab-ci/image-20241030215558441.png)

Create the `SONAR_TOKEN` variable. The token had been created for gitlab on sonarqube earlier.

![image-20241030215650743](assets/4-5-configure-gitlab-ci/image-20241030215650743.png)

Create the `AI_AGENT_URL` variable. The ai agent url is ai-suggestion we deployed earlier.

![image-20241124165546922](assets/4-5-configure-gitlab-ci/image-20241124165546922.png)

The CICD variable configuration is completed.

![image-20241124165809367](assets/4-5-configure-gitlab-ci/image-20241124165809367.png)

`User1` and `user2`  had been created via gitlab installation script.

![image-20241030215837143](assets/4-5-configure-gitlab-ci/image-20241030215837143.png)

Create a new `dev` group.

![image-20241030215907699](assets/4-5-configure-gitlab-ci/image-20241030215907699.png)

Add `user1` and `user2` to the `dev` group.

![image-20241030215940560](assets/4-5-configure-gitlab-ci/image-20241030215940560.png)

Clone `https://github.com/rh-demos/my-quarkus` from github to the `dev` group of gitlab.

![image-20241030220134543](assets/4-5-configure-gitlab-ci/image-20241030220134543.png)

Import the gitlab project from the sonarqube project page

![image-20241030220205808](assets/4-5-configure-gitlab-ci/image-20241030220205808.png)

Enter the `access token` you created earlier for sonarqube.

![image-20241030220237218](assets/4-5-configure-gitlab-ci/image-20241030220237218.png)

Browse to find the `My Quarkus` project and click Set up.

![image-20241030220304459](assets/4-5-configure-gitlab-ci/image-20241030220304459.png)

Configure analyze to gitlab CI.

<img src="assets/4-5-configure-gitlab-ci/image-20241030220331414.png" alt="image-20241030220331414" style="zoom:50%;" />

Follow the prompts to add the `<sonar.qualitygate.wait>true<sonar.qualitygate.wait>` definition to the `pom.xml` of `My Quarkus` project.

![image-20241030220412882](assets/4-5-configure-gitlab-ci/image-20241030220412882.png)

Add and submit the changes from gitlab page.

![image-20241030220533262](assets/4-5-configure-gitlab-ci/image-20241030220533262.png)

Navigate to the CICD -> Pipeline configuration of the project from the gitlab page and stop the started pipeline.

![image-20241030220603465](assets/4-5-configure-gitlab-ci/image-20241030220603465.png)

Delete the pipeline after stopped

![image-20241030220651004](assets/4-5-configure-gitlab-ci/image-20241030220651004.png)

Get the projectKey of My Quarkus in sonarqube from the sonarqube analyze configuration wizard.

![image-20241030220717964](assets/4-5-configure-gitlab-ci/image-20241030220717964.png)

Click to complete the wizard configuration and enter the state of waiting for the first code analysis.

![image-20241030220748317](assets/4-5-configure-gitlab-ci/image-20241030220748317.png)

Navigate to the gitlab CICD edit page.

![image-20241030220814765](assets/4-5-configure-gitlab-ci/image-20241030220814765.png)

Add the following CI definition. The sonarqube `projectKey` and `ai suggestion url` need to be replaced, projectKey can be found from previous copied `.gitlab-ci.yml` content.

```
image: maven:3.6.3-jdk-11

cache:
  paths: []

variables:
  MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"
  PROJECT_KEY: "<your project key>"

stages:
  - build
  - sonar
  - test
  - package

build:
  stage: build
  script:
    - mvn clean compile
  only:
    - master
  tags:
    - your-runner-tag

sonar:
  image: maven:3.6.3-jdk-11
  variables:
    SONAR_USER_HOME: "${CI_PROJECT_DIR}/.sonar"  
    GIT_DEPTH: "0" 
  cache:
    paths: []
  script: | 
    mvn verify sonar:sonar -DskipTests -Dsonar.qualitygate.wait=true -Dsonar.projectKey=${PROJECT_KEY} || true
    SONAR_EXIT_CODE=$?

    if [ -n "${CI_MERGE_REQUEST_IID}" ]; then
      echo "Triggering AI suggestion..."
      curl -s -k -X GET "${AI_AGENT_URL}/suggestion?project_key=${PROJECT_KEY}&merge_request_iid=${CI_MERGE_REQUEST_IID}"
    fi

    exit $SONAR_EXIT_CODE
  allow_failure: false
  only:
  - merge_requests
  - main

test:
  stage: test
  script:
    - mvn test
  only:
    - master
  tags:
    - your-runner-tag

package:
  stage: package
  script:
    - mvn package
  artifacts:
    paths:
      - target/*.jar
  only:
    - master
  tags:
    - your-runner-tag
```

The first pipeline will be triggered after click `Save`, wait for its completion.

![image-20241030220938227](assets/4-5-configure-gitlab-ci/image-20241030220938227.png)

Log in to sonarqube as `user1` and fork `My Quarkus` project.

![image-20241030221017191](assets/4-5-configure-gitlab-ci/image-20241030221017191.png)

Navigate to the general configuration item of the cloned project (`Visibility, project features, permissions)` and turn off CICD, do not forget to save changes.

![image-20241030221049928](assets/4-5-configure-gitlab-ci/image-20241030221049928.png)

