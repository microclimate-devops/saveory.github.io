---
layout: page
title: Experience
---

# Microservice Builder Experience

Docker, microservices, Kubernetes... All new to us when we started the Saveory project. We had an idea and a few plans, not much else. Learning curves are inevitable but getting a new service started with Microservice Builder (MsB) was refreshingly simple. The setup instructions are clear and work exactly as intended.  IBM's lightweight Liberty app server got our first endpoint up in a matter of minutes. MsB takes care of setting up deployment details so that focus can remain on development details. A solid understanding of the various deployment platforms available, such as Docker and Kubernetes, is helpful but not necessarily required. Don't worry, all the config files are there, plenty to satisfy a fastidious DevOps engineer. For the developer just getting started, a well organized file structure helps along the way. We recommend setting up the Microservice Builder Jenkins pipeline as well. It's open source(!) and an excellent deployment agent for Kubernetes. Creating an application from scratch will always be challenging but if you need microservices, MsB offers an excellent starting point.

# Our Microsevice Builder Setup

Follow the 3 steps in the [IBM Microservice Builder webpage](https://developer.ibm.com/microservice-builder/#getStarted). After following these steps you should have Docker, Git and the Bluemix CLI installed in your system.

## Create Project

To create MSB projects we use the command bx dev create which creates a new project in the current directory with a default setup that uses all of the components that MSB offers
When the command is issued you are first asked if you want to login (if you have a Bluemix account and want this project to be synchronized to your Bluemix Dashboard)
Then a list of project patterns is displayed. We want write “4” to select the Microservice project pattern
Then we select the only starter currently available which is Basic by writing “1”
Now we want to select the programming language of preference for this Microservice. In our case it was Java with MicroProfile/Java EE  (option 1) due to better knowledge of it.
Finally, we write the name of the project that will be created
From the beginning you will have:
- Dockerfile and Dockerfile-tools – Used to create the docker container
- Jenkinsfile – Used to configure the Jenkins deployment of the microservice
- cli-config.yml – File that contains the current configuration of the project
- manifest.yml – Used when building the application
- pom.xml – Contains all of the required dependencies to build the application
- manifests/kube.deploy.yml – Contains details for the kubernetes deployment of the application
- src/main/java/application/rest/v1/Example.java – REST Template to start implementation of the microservice application

### Run and Build:

To build the project you need to use the command `bx dev build` which will utilize maven to build the project
If no issues appear then you can use `bx dev run` to run the application. The application should be running in http://localhost:9080 with the default configuration. To stop it just use Ctrl+C in the terminal.

### Deployment:

It is recommended to use Kubernetes because the project is already pre-configured to work with it thanks to the files in the chart and manifests directories.
Also, it is recommended to create an organization for the application and repositories for each Microservice to be able to use a pipeline
A pipeline can be configured to be able to deploy your application automatically when it detects changes in the Microservice repository while using the Jenkinsfile it contains with Jenkins.

### Testing and Development:

When we need to test new development changes that involves only one Microservice then we build and run the application using the bluemix commands mentioned in the “Run and Build” section. In the case of testing an endpoint that had an interaction between Microservices then we push the code into the remote repository and wait until the pipeline detects the changes and for Jenkins to build the newer Microservice in the IBM Cloud Private environment. In other words, we test in the deployment.
