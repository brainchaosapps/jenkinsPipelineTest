# Cloned Spring PetClinic Sample Application [![Build Status](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml/badge.svg)](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml)

This is a cloned, testing environment of https://github.com/spring-projects/spring-petclinic.

This is a test project to build a jenkins pipeline, which compiles and tests the code and finally packages the project as a runnable Docker image.

# How to run the project?

Please be informed, this documentation and the work was done a Windows PC, not on a Linux machine. So if you are also using Windows, please make sure you installed Linux for Windows: https://learn.microsoft.com/en-us/windows/wsl/install


## Prerequisites

- make sure you have Jenkins running on a machine or you may use (Jenkins in Docker container)[https://www.jenkins.io/doc/book/installing/docker/]
- if not already done, please add the the Pipline Plugin and Github to Jenkins
- install Java SDK 17 (not a JRE!)
- furthermore make sure you set JAVA_HOME environment variable to point to you java installation (e.g. "set JAVA_HOME=C:\Program Files\Java\jdk-17.0.5")
- get git command line tool ([https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git))
- have Docker installed and ensure the Docker daemon is running and is accessible (https://docs.docker.com/get-docker/)
- Optional: Make sure you have a JFrog Artifactory Server running
  https://jfrog.com/de/artifactory/install/
  
## Creating the Pipeline

We will now have to create the Jenkins pipeline. Perform the following steps on the Jenkins Classic UI:

1.  Create a new Jenkis job by clicking **New Item**.
2.  **Enter an item name**, feel free to choose whatever you want for testing puposes.
3.  Choose **Pipeline** as the job type and click **OK**.
4.  Under **Pipeline -> Definition** choose **Pipeline script from SCM** and then choose **Git**
6.  Under **Repository URL** enter: https://github.com/brainchaosapps/jenkinsPipelineTest.git
7.  Leave the rest at the default and click **Save**.

## Executing the Pipeline

You should now have a pipeline configured. When executing the pipeline, Jenkins will clone the Git repository, look for a file named `Jenkinsfile` at its root and execute the instructions (stages) in it:

##### The three stages are:

    a) Compile
    b) Test
    c) Build Docker Image

  
# Docker container and how to run it

Executing the pipeline delivers a docker image `spring-petclinic:3.0.0-SNAPSHOT` (last stage). You can use/run it the following way:

### Command to run the docker image
```
docker run --name testPetclinic spring-petclinic:3.0.0-SNAPSHOT
```

The `run` command basically is `docker create` and `docker start`, which creates a container and then starts the image. You can then access petclinic here: [http://localhost:8080/](http://localhost:8080/)

If there is an already created container but stopped, you may restart it with

```
docker start testPetclinic
```

#### Got a saved docker image?

If you have a saved Docker image, you can use the following to add it:

```
docker load -i imageDockerFile.tar
```

Now you can run the image like described above.

  


# Optional: How to integrate JFrog Artifactory

Please login to your Jenkins Server to integrate Artifactory with Jenkins.

## Steps

1. Install the [Jenkins Artifactory plug-in](https://plugins.jenkins.io/artifactory/). In the left menu, choose:
   `Manage Jenkins` -> `Jenkins Plugins` -> `available` -> `artifactory`
2. Configure Artifactory server credentials in the Jenkis UI (alternativly, you can create a [pipeline script](https://www.jfrog.com/confluence/display/JFROG/Declarative+Pipeline+Syntax))
   Please got to `Manage Jenkins` -> `Configure System` -> `Artifactory`
   - Artifactory Servers
      - Server ID : `Your-Artifactory-Server-ID`
      - URL : `Your-Artifactory-Server-URL`
      - Username : `jenkins`
      - Password : *`my-jenkins-password`*
    
  - **Important hint**: We have a default user called `admin` but its not best practice to use `admin` user. Please create a new user in your Artifactory server, e.g. `jenkins`. Please be aware that this user needs to have Admin Privileges, because otherwise the user is not able to store artifacts in the Artifactory.

Now we are done with integrating JFrog Artifactory to our Jenkins server. Next step is to send the result of our pipeline to the Artifactory server. This project used the declarative pipline syntax, so please refer to: https://www.jfrog.com/confluence/display/JFROG/Declarative+Pipeline+Syntax

# License

This is a clone of the Spring PetClinic sample application, which is released under version 2.0 of the [Apache License](https://www.apache.org/licenses/LICENSE-2.0).

  

This project is for private use only.