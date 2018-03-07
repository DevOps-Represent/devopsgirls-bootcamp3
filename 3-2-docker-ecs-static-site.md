# Module 2: ECS (CS)

## Learning Objectives

● Learn about Containers technologies and understand their appeal to developers and how 
to deploy, host and scale containers using AWS tools.  
● Use Cloudformation to create the prerequisite resources required by ECS. This includes
creating a Cluster, a Service, and Tasks (using Cloudformation)    
● This module will go over creating a Docker container (using resources from S3),
uploading it to a Registry.  
● Modify the pipeline created above to also deploy the site to the ECR cluster.  
● Recognize the convenience and power of containers & appreciate the benefits of using
this approach.

## Material & Tools Required
● Laptop  
● AWS account  
● Same site files from previous module  

## 2.- Deploy a static website using a Docker container on AWS ECS

In this module you will deploy a static website using a [Docker container](https://www.docker.com/what-container) on Amazon Elastic Container Service ([Amazon ECS](https://aws.amazon.com/ecs/)). The main advantage of this method is that you can use it to run Docker applications on a scalable cluster that allows you, for instance, to run the applications easily by eliminating the need for you to install, operate, and scale your own cluster management infrastructure or to scale your containers up or down to meet your application's capacity requirement.

If you are imagining how it could be, it should look like this:

![ECS_Docker_Container_Diagram](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/ECS_Docker_Container.png?raw=true)

### 2.1 Introduction to the Dockerfile definition

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using **docker build** users can create an automated build that executes several command-line instructions in succession.

*For further information about Dockerfile please visit [this link](https://docs.docker.com/engine/reference/builder/).

2.1.1 In this exercise you will use a *Dockerfile* that already has the required steps for our container definition and also is ready to be used in the [CloudFormation](https://aws.amazon.com/cloudformation/) template that will create all the infrastructure for you, such as the container definition, the task definition, the service and the cluster configuration.

The *Dockerfile* for this exercise looks like this:

```javascript
FROM httpd:alpine

COPY ./index.html /usr/local/apache2/htdocs/index.html
```

### 2.2 Creating the Elastic Container Service (ECS) using a Cloudformation template

As we mentioned in the previous step, you will create the whole infrastructure using the AWS Cloudformation service which allows you to use a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications.  
Also, in the template is included a docker container definition that contain a web server to include an HTML static web page. This web page is going to be the first check point of the exercise, since in the next part you will configure a **Codepipeline** to deploy a new release of the web page.

2.2.0 Right click on the the following [link](https://raw.githubusercontent.com/DevOps-Girls/devopsgirls-bootcamp3/master/templates/module2-ecs-static-site.yaml) and save the *module2-ecs-static-site.yaml* on your local machine / computer / laptop.

2.2.1 Login with your credential to the AWS account, you have to do the same steps as in Module 1:

2.2.1.1 Open a web browser and go to https://xxxxxx.signin.aws.amazon.com/console. Log in using your supplied account credentials.  

2.2.1.2 Ensure you are in the right AWS region: on the top-right side, make sure you select Sydney.

2.2.1.3 Once you have successfully logged in, under the AWS services search field, search for *Cloudformation*, and click on the **Cloudformation** auto drop down element.


2.2.2 In the screen's upper left corner click on the blue button 'Create Stack'.

![3-2-2-2-create-cf-stack](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-2-create-cf-stack.png?raw=true)

2.2.3 In the **Select Template** page, under the section *Choose a template* provide the Cloudformation template that you have already downloaded in step 2.2.0 (*module2-ecs-static-site.yaml*). Then click Next.

![3-2-2-3-select-template](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-3-select-template.png?raw=true)


2.2.4 Under the **Specify Details** page, in the *Stack name* field enter your first name and last name without a space, followed by the **-cf-ecs** string. Under *Parameters* > *SharedParentStack*, enter "DevOpsGirlsVPC". Leave the rest of the information with the default values. Then click Next.

![3-2-2-4-specify-datails](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-4-specify-datails.png?raw=true)

2.2.5 In the **Options** page specify the following tags:

![3-2-2-5-options-page](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-5-options-page.png?raw=true)

|Key|Value|
|---|---|
|Creator|*your first and last name*|
|Module|2|
|Workshop|DevopsGirls|
|Resource|ECS Cloudformation|

Once you have completed the tags section, leave the remain information with the default values, and click next.

2.2.6 In the **review** page check out if everything is good according to the instructions, in the case that you need to modify something click the Previous button, or if everything is **ok** click the Create button to create the ECS cluster using the Cloudformation template.  
Also, at the end of the page you have to **check** the box *"I acknowledge that AWS CloudFormation might create IAM resources."*

![3-2-2-6-review.png](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-6-review.png?raw=true)

2.2.7 The Stack creation steps, is going to take about 5 - 8 minutes. 

![3-2-2-8-Creating_Stack](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-8-Creating_Stack.png?raw=true)


2.2.8 Once it has been created and ready you have to see something like this:

![3-2-2-9-Stack_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-9-Stack_Created.png?raw=true)

2.2.9 Another important step when the stack is ready is to check the resources or elements that it has created. To do that, go to the **Resources** tab and you will see all the resources that have been created using the **Cloudformation template** in the new **Stack**

![3-2-2-10-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-Resources_Created.png?raw=true)


2.2.10 Another way to do that (and maybe a more user-friendly way) is check the template in the **designer** application and it will show you a diagram with all the resources that have been created. To do it, check the Stack Name box that you want to review, and in the **Actions button** choose the *View/Edit template in Designer*:


![3-2-2-10-b-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-b-Resources_Created.png?raw=true)


2.2.11 In the designer page you can check out the Cloudformation template definition as well as the diagram that shows you the elements that have been created using the template.


![3-2-2-10-c-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-c-Resources_Created.png?raw=true)

In this case the diagram looks like this:

![3-2-2-10-c-Resources_Created](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-10-cf-template-diagram.png?raw=true)

2.2.12 Now is the time to check the ECS that has being created in the stack using the Cloudformation template. To do that, under the AWS services search field, search for *ECS*, and click on the *Elastic Container Service* auto drop down element.

![3-2-2-11-a-Searching_ECS_Cluster](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-a-Searching_ECS_Cluster.png?raw=true)


2.2.13 In the **Cluster** page your will be able to see your cluster that contain the name of the stack followed by a random sequence number

![3-2-2-11-ECS_Cluster](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Cluster.png?raw=true)

2.2.14 Click on the *cluster name* and you will be redirected to the cluster details configuration page. Here you can see information, for instance, about the number of containers configured in the cluster, as well as the status of the cluster, service(s) and task(s).   
(A service allows you to run and maintain tasks (containers) in a cluster.)

![3-2-2-11-ECS_Service](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service.png?raw=true)

2.2.15 Click on your *Service Name* and the **Service** page will show up. Here you can see more information about the service configuration.  
Now you need to find the IP or URL of the container running the web server, which, in turn, contain the static web page.

![3-2-2-11-ECS_Service_Task](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service_Task.png?raw=true)

2.2.16 Under the **Tasks** tab, click on the task that is associated to your service in the **Task** column.   
Now you are almost there ;) ! The container we deployed using the Cloudformation template is called **simple-app**, so look for this in the **Containers** section at the bottom of the page.

![3-2-2-11-ECS_Service_Task_Container](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-11-ECS_Service_Task_Container.png?raw=true)

2.2.17 Expand the **simple-app** line to reveal all the details about the container. The URL you need is under **External Link**.

![3-2-2-12-ECS_Task_Container_info](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-12-ECS_Task_Container_info.png?raw=true)

2.2.18 Click on the **External Link** information and congratulations ! :) you will see the static web page that you have built using the Cloudformation template.

![3-2-2-13-Container_web](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-2-13-Container_web.png?raw=true)

### 2.3 Creating the AWS CodePipeline, Continuous integration and Continuous Delivery service.

Now it's time to create the AWS CodePipleline for continuous integration and continuous delivery of your application. You can use CodePipeline to fully model and automate your software release processes.

2.3.0 Download `Dockerfile` and `buildspec.yml` from [DevopsGirls' public S3 bucket](https://s3.console.aws.amazon.com/s3/buckets/devopsgirls1):

![3-2-3-0-Update-CodeCommit-Repository](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-0-Update-CodeCommit-Repository.PNG?raw=true)

And push them to the **CodeCommit** repository that you have created in [Module 1](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/3-1-CodeStar-EC2.md). To do that, follow the steps described in section [5. Push changes to CodeCommit Repo](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/3-1-CodeStar-EC2.md#5-push-changes-to-codecommit-repo)

2.3.1  Under the AWS services search field, search for codepipeline, and click on the **CodePipeline** auto drop down element

2.3.2 Click on the **Create Pipeline** button. In the **Step 1:Name** fill in **Pipeline name** with *your First Name*, *Your Last Name* followed by *-Pipeline*. If there's already one with this name, append *-Pipeline2* instead.

![3-2-3-2-CodePipeline_Creation.png](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-2-CodePipeline_Creation.png?raw=true)

2.3.3 In the **Step 2:Source**, under the **Source Provider** choose *AWS CodeCommit* and in the **Repository** field provide the repository that you have used in [Module 1](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/3-1-CodeStar-EC2.md), branch "master. Leave the remaining information as default.

![3-2-3-3-CodePipeline_Creation_Source](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-3-CodePipeline_Creation_Source.png?raw=true)

2.3.4 Under the **Step 3: Build** choose *AWS CodeBuild* as a **Build provider** and in the **Configure your project** section choose *Create a new build project* and name it *your First Name*, *Your Last Name* followed by *-CodeBuild*.  
Fill the sections **Environment: How to build** and **Cache** as per the screenshot.  
For **AWS CodeBuild service role**, choose the build role that was created in our ECS Cloudformation stack (you can review the stacks's resources as in **2.2.9**, it's the value of the **Logical ID** *Module2CodeBuildRole*).  
In the next section **VPC**, you have to specify the VPC, the private subnets associated in the cloudformation creation also the Security group (if you have any doubt about how to get those information double check it with one of the coaches).   
And finally in the **Advanced** section, you have to create four *Environment variables*:

|Name|Value|Type|
|---|---|---|
|IMAGE_REPO_NAME|*The name of your ECR repository (**Logical ID** ECRRepository)*|Plaintext|
|IMAGE_TAG|*latest*|Plaintext|
|AWS_ACCOUNT_ID|*The IdNumber associated to your account*|Plaintext|
|AWS_DEFAULT_REGION|*The region where you are working in ("ap-southeast-2" for Sydney)*|Plaintext|

Once you have provided the above information you can click on **Save build project** button and click **Next**.

![3-2-3-4-a-CodePipeline_Creation_Build](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-4-a-CodePipeline_Creation_Build.png?raw=true)
![3-2-3-4-b-CodePipeline_Creation_Build](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-4-b-CodePipeline_Creation_Build.png?raw=true)
![3-2-3-4-c-CodePipeline_Creation_Build](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-4-c-CodePipeline_Creation_Build.png?raw=true)

2.3.5 In the **Step 4: Deploy** page provide the information according your *ECS* Cluster configuration as per the screenshot. The **Image file name** must be *imagedefinitions.json*

![3-2-3-5-CodePipeline_Creation_Deploy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-5-CodePipeline_Creation_Deploy.png?raw=true)

2.3.6 Under the final configuration step **AWS Service Role** in the *Role name* field you have to provide the pipeline role that was created in our ECS Cloudformation template (**Logical ID** *Module2CodePipelineRole*).

![3-2-3-6-CodePipeline_Creation_ServiceRole](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-6-CodePipeline_Creation_ServiceRole.png?raw=true)

2.3.7 Now is the time to double check your configuration, and if everything looks as expected, click on the **Create pipeline** button.

![3-2-3-7-CodePipeline_Creation_Review](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-7-CodePipeline_Creation_Review.png?raw=true)


2.3.8 Now the CodePipeline that you have just created will connect to the CodeCommit repo specified and automatically start deploying a new release of the **simple-app** Docker container that runs the web server with the static web page. 


![3-2-3-8-a-CodePipeline_Running](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-8-a-CodePipeline_Running.png?raw=true)

You will need to wait for the pipeline to go through all three stages (it could take about 10 min) and once it's ready, connect to the IP of your web server again and a new web page should be deployed ;).

![3-2-3-8-b-CodePipeline_Completed](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-8-b-CodePipeline_Completed.png?raw=true)

CONGRATULATIONS, you have deployed a new release using AWS CodePipeline !

![3-2-3-8-b-CodePipeline_Completed](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-2-docker-ecs-static-site/3-2-3-8-c-CodePipeline_NewDepoyment.png?raw=true)





