# Module 2: ECS (CS)

## Learning Objectives

● Learn about Containers technologies and understand their appeal to developers and how
to deploy, host and scale containers using AWS tools.  
● This module will go over creating a Docker container (using resources from S3 ),
uploading it to a Registry.  
● Use Cloudformation to create the prerequisite resources required by ECS . This includes
creating a Cluster , a Service , Tasks (using Cloudformation)  
● Modify the pipeline created above to also deploy the site to the ECR cluster.  
● Recognize the convenience and power of containers & appreciate the benefits of using
this approach.

## Material & Tools Required
● Laptop  
● AWS account  
● Same site files from previous module  

## 2.- Deploy a static website using a Docker container on AWS ECS

In this module you will deploy a static website using a [Docker container](https://www.docker.com/what-container) on Amazon Elastic Container Service ([Amazon ECS](https://aws.amazon.com/ecs/)). The main aim of this configuration is that you can use it to run Docker applications on a scalable cluster that allows you, for instance, to run the applications easily by eliminating the need for you to install, operate, and scale your own cluster management infrastructure or to scale your containers up or down to meet your application's capacity requirement.

If you are imagining how it could be, it should look like this:

![ECS_Docker_Container_Diagram](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/2-1-docker-ecs-static-site/ECS_Docker_Container.png?raw=true)



### 2.1 Introduction to the Docherfile definiton

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using **docker build** users can create an automated build that executes several command-line instructions in succession.

*For further information about Dockerfile please visit [this link](https://docs.docker.com/engine/reference/builder/).

2.1.1 In this exercise you will use a *Dockerfile* that it has already the necessaries steps that the container definition needs and also is ready to be use it in the [CloudFormation](https://aws.amazon.com/cloudformation/) template that will create all the infrastructure for you, as the container definition, the task definition, the service and the cluster configuration.

The *Dockerfile* looks like this:

```javascript
FROM httpd:2.4

WORKDIR /usr/local/apache2/htdocs

COPY s3://devopsgirls1/index.html .
```

### 2.2 Creating the ECS using Cloudformation template

As we mention in the previous step, you will create the whole infrastructure using the AWS Cloudformation service which allows you to use a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications.

2.2.0 Right click on the the following [link](https://raw.githubusercontent.com/DevOps-Girls/devopsgirls-bootcamp3/master/templates/module2-ecs-static-site.yaml) and save the *module2-ecs-static-site.yaml*  in your local machine / computer / laptop.

2.2.1 Login with your credential to the AWS account

2.2.2  Under the AWS services search field, search for *Cloudformation*, and click on the **Cloudformation** auto drop down element.

2.2.2 In the screen's upper left corner click on the blue button 'Create Stack'.

2.2.3 In the **Select Template** page, under the section *Choose a template* provide Cloudformation templeate that you have already downloaded in the step 2.2.0 (*module2-ecs-static-site.yaml*)

2.2.4 Under the **Specify Details** page, in the *Stack name* field enter your first name, and last name without a space following the **-cf-ecs** string, and leave the rest of the information with the default values.

2.2.5 In the **Options** page specify the following tags:

|Key|Value|
|---|---|
|Creator|*your first and last name*|
|Module|2|
|Workshop|DevopsGirls|
|Resource|ECS Cloudformation|

and in the permissions sections, choose the default existing IAM role and leave the rest information with the default values, and click next.

2.2.6 In the **review** page check out if everything is good according with the instructions, in the case that you need to modify something click in the previous button, or if everything is ok click in the create one to create the ECS cluster using the Cloudformation template.
