# 2.- Deploy a static website using a Docker container on AWS ECS

In this module you will deploy a static website using a [Docker container](https://www.docker.com/what-container) on Amazon Elastic Container Service ([Amazon ECS](https://aws.amazon.com/ecs/)). The main aim of this configuration is that you can use it to run Docker applications on a scalable cluster that allows you, for instance, to run the applications easily by eliminating the need for you to install, operate, and scale your own cluster management infrastructure or to scale your containers up or down to meet your application's capacity requirement.

If you are imagining how it could be, it should look like this:

![ECS_Docker_Container_Diagram](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/2-1-docker-ecs-static-site/ECS_Docker_Container.png?raw=true)



## 2.1 Introduction to the Docherfile definiton

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using **docker build** users can create an automated build that executes several command-line instructions in succession.

*For further information about Dockerfile please visit [this link](https://docs.docker.com/engine/reference/builder/).

2.1.1 In this exercise you will use a *Dockerfile* that it has already the necessaries steps that the container definition needs and also is ready to be use it in the [CloudFormation](https://aws.amazon.com/cloudformation/) template that will create all the infrastructure for you, as the container definition, the task definition, the service and the cluster configuration.

The *Dockerfile* looks like this:

```javascript
FROM httpd:2.4

WORKDIR /usr/local/apache2/htdocs

COPY s3://devopsgirls1/index.html .
```

## 2.2 Creating the ECS using Cloudformation template

As we mention in the previous step, you will create the whole infrastructure using the AWS Cloudformation service which allows you to use a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications.

2.2.1 