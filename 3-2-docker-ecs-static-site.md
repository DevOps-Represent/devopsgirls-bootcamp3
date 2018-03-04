# Module 2: ECS (CS)

## Learning Objectives

● Learn about Containers technologies and understand their appeal to developers and how
to deploy, host and scale containers using AWS tools.
● Use Cloudformation to create the prerequisite resources required by ECS . This includes
creating a Cluster , a Service , Tasks (using Cloudformation)    
● This module will go over creating a Docker container (using resources from S3 ),
uploading it to a Registry.  
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

![ECS_Docker_Container_Diagram](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/ECS_Docker_Container.png?raw=true)

### 2.1 Introduction to the Dockerfile definiton

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using **docker build** users can create an automated build that executes several command-line instructions in succession.

*For further information about Dockerfile please visit [this link](https://docs.docker.com/engine/reference/builder/).

2.1.1 In this exercise you will use a *Dockerfile* that it has already the necessaries steps that the container definition needs and also is ready to be use it in the [CloudFormation](https://aws.amazon.com/cloudformation/) template that will create all the infrastructure for you, as the container definition, the task definition, the service and the cluster configuration.

The *Dockerfile* looks like this:

```javascript
FROM httpd:2.4

WORKDIR /usr/local/apache2/htdocs

COPY s3://devopsgirls1/index.html .
```

### 2.2 Creating the Elastic Container Service (ECS) using a Cloudformation template

As we mention in the previous step, you will create the whole infrastructure using the AWS Cloudformation service which allows you to use a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications.  
Also, in the template is included a docker container definition that contain a web server to include an HTML static web page. This web page is going to be the first check point of the exercise, since after that in the next part you will configure a **Codepipeline** to deploy a new release of the web page.

2.2.0 Right click on the the following [link](https://raw.githubusercontent.com/DevOps-Girls/devopsgirls-bootcamp3/master/templates/module2-ecs-static-site.yaml) and save the *module2-ecs-static-site.yaml*  in your local machine / computer / laptop.

2.2.1 Login with your credential to the AWS account

2.2.2  Under the AWS services search field, search for *Cloudformation*, and click on the **Cloudformation** auto drop down element.


2.2.2 In the screen's upper left corner click on the blue button 'Create Stack'.

![3-2-2-2-create-cf-stack](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-2-create-cf-stack.png?raw=true)

2.2.3 In the **Select Template** page, under the section *Choose a template* provide Cloudformation templeate that you have already downloaded in the step 2.2.0 (*module2-ecs-static-site.yaml*)

![3-2-2-3-select-template](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-3-select-template.png?raw=true)


2.2.4 Under the **Specify Details** page, in the *Stack name* field enter your first name, and last name without a space following the **-cf-ecs** string, and leave the rest of the information with the default values.

![3-2-2-4-specify-datails](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-4-specify-datails.png?raw=true)

2.2.5 In the **Options** page specify the following tags:

![3-2-2-5-options-page](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-5-options-page.png?raw=true)

|Key|Value|
|---|---|
|Creator|*your first and last name*|
|Module|2|
|Workshop|DevopsGirls|
|Resource|ECS Cloudformation|

Once you have completed the tags section, leave the rest information with the default values, and click next.

2.2.6 In the **review** page check out if everything is good according with the instructions, in the case that you need to modify something click in the previous button, or if everything is ok click in the create one to create the ECS cluster using the Cloudformation template.
And at the end of the page you have to check the box about "I acknowledge that AWS CloudFormation might create IAM resources."

![3-2-2-6-review.png](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-6-review.png?raw=true)

2.2.7 The Stack creation steps, is going to take about 5 - 8 minutes. 

![3-2-2-8-Creating_Stack](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-8-Creating_Stack.png?raw=true)


2.2.8 One it has been created and ready you have to see something like this:

![3-2-2-9-Stack_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-9-Stack_Created.png?raw=true)

2.2.9 Another important steps when the stack is ready is check the resources or elements that it has created. To do this, go to the **Resources** tab and you will be able to know all about the resources that have been created using the **Cloudformation template** in the new **Stack**

![3-2-2-10-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-Resources_Created.png?raw=true)


2.2.10 Another way to do that (and maybe a more userfriendly way) is check the template in the **designer** application and it will show you a diagram with all the resources that have been created. To do it, check the Stack Name box that you want to review, and in the **Actions button** choose the *View/Edit template in Designer*:


![3-2-2-10-b-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-b-Resources_Created.png?raw=true)


2.2.11 In the designer page you can check out the Cloudformation template definition as well as the diagram that shows you the elements that have been created using the template.


![3-2-2-10-c-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-c-Resources_Created.png?raw=true)

In this case the diagram looks like this:

![3-2-2-10-c-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-cf-template-diagram.png?raw=true)

2.2.12 Now is the time to check the ECS that has being created in the stack using the Cloudformation template. To do that, under the AWS services search field, search for *Elastic Container Service*, and click on the *Elastic Container Service* auto drop down element.

![3-2-2-11-a-Searching_ECS_Cluster](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-a-Searching_ECS_Cluster.png?raw=true)


2.2.13 In the **Cluster** page your will be able to see your cluster that contain the name of the stack following a random sequence number

![3-2-2-11-ECS_Cluster](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Cluster.png?raw=true)

2.2.14 Click on the *cluster name* and you will be redirect to the cluster details configuration page. Here you can see information, for instance, about the number of the containers that are configured in the cluster, as well as, the status of cluster, service and task(s).   
(A service allows you to run and maintain tasks (containers) in a cluster.)

![3-2-2-11-ECS_Service](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service.png?raw=true)

2.2.15 Click on your *Service Name* and the **Service** page will show up. Here you can see more information about the service configuration.  
In this moment your aim is find the place where you can get the IP or URL of the container that contain the web server, which, in turn, contain the static web page.

![3-2-2-11-ECS_Service_Task](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service_Task.png?raw=true)

2.2.16 Under the task tab, click on the task that is associated to your service in the **Task** column.   
Now you are almost there ;)!, remeber that you are looking for the web server information that is configured in a container, so in the bottom of the page you will find a container called **simple-app** and it will give you the information that you are looking for. 

![3-2-2-11-ECS_Service_Task_Container](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service_Task_Container.png?raw=true)

2.2.17 In the **Task** page, under the **Container** section click over the **simple-app**, you will see all the details about the container, and also the one that you are looking for that correspond to the value providing in the **External Link** information.

![3-2-2-12-ECS_Task_Container_info](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-12-ECS_Task_Container_info.png?raw=true)


2.2.18 Click on the **External Link** information and congratulation! :) you will see the static web page that you have built using the Cloudformation.

![3-2-2-13-Container_web](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-13-Container_web.png?raw=true)