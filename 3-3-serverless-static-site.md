# Serverless Website

## Recap

- In Module 1, you deployed a static website to EC2 instances. You had to choose an instance size based on how popular your site is going to be, choose an operating system for the EC2 instance, install and configure nginx and deploy the static site. 
- In Module 2, you deployed a container which served your static web site to an ECS cluster. You had to pool some EC2 instances into an ECS cluster, choose CPU and memory for the container(s), build and package the containers, and deploy them to the cluster

## Key Concepts

- When you use EC2 instances, or containers, you have to make some decisions on capacity when you are provisioning. You also have to pay for that capacity, and manage and operate those resources. For example in case of EC2, you have to keep the OS and software configured and kepts up to date.

- What is Serverless? Do not think of the word literally, as there are servers under the hood. You dont manage those servers, and do not have to take decisions on the capacity you might need. Also when you dont use the capacity, you dont pay anything for it.

- In our case of hosting a static website, we do not perform any dynamic computation. We store some files, and the computing capacity is used to handle web requests and serve the static pages.

- Simple Storage Service, S3 is an object store. In simple terms, you have the filename which is your key, and the contents of the file is the object. S3 provides you a durable file storage, and provides an API to GET, PUT and DELETE objects. It can act as a web server and serve objects on HTTP GET requests.

## What we are going to do

In this practical session, we will

- Create a S3 bucket and configure it as a web server.
- Load the static pages of our website to the bucket.
- Set up a pipeline to publish any changes to the code to S3.

## Create S3 bucket as a web server

### 1.) Creating an S3 Bucket
Go to Services > S3, then click on "Create Bucket"

![Create Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-1-create-s3-bucket.png?raw=true)

### 2.) Choose a name for S3 Bucket
Enter a unique bucket name and click "Next". The bucket name has to be globally unique. So use something like `yourname-devopsgirls-site`

![Create Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-2-create-s3-bucket.png?raw=true)

### 3.) Accept defaults
Click "Next" without making any changes.

![Create Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-3-create-s3-bucket.png?raw=true)

### 4.) Make bucket public
In the "Manage public permissions" drop down choose "Grant public read access to this bucket". Note that you will have a warning, but since we want to host a public website in this bucket, it is ok. Click "Next".

![Create Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-4-create-s3-bucket.png?raw=true)

### 5.) Confirm bucket creation
Review the inputs, and click "Create Bucket"

![Create Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-5-create-s3-bucket.png?raw=true)

## Configure S3 bucket as a webserver

### 1.) Choose the S3 bucket just created
Go to Services > S3, then click on the bucket you just created

![Configure Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-6-configure-s3-bucket.png?raw=true)

### 2.) Modify bucket properties
Click on the "Properties" tab, and click on "Static Website Hosting"

![Configure Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-7-configure-s3-bucket.png?raw=true)

### 3.) Configure bucket to host a website
Choose "Use this bucket to host a website", enter "index.html" in the "Index Dcoument" text box, and click "Save".

![Configure Bucket](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-8-configure-s3-bucket.png?raw=true)

## Copy the static website files to S3 bucket and make them public

### 1.) Copy the static files to S3 bucket
Change working directory to website_files, and copy the files to the S3 bucket created above.

```
$ cd website_files
$ aws s3 sync . s3://`yourname-devopsgirls-site`
```

### 2.) Confirm files have been uploaded
Navigate to the S3 bucket in the AWS console, and confirm all the files in the website_files directory are listed there.

### 3.) Make files public
Choose all the files, click on "More" and choose "Make Public". When prompted, confirm by clicking "Make Public" again

![Make Public](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-9-make-files-public.png?raw=true)


### 4.) Access the website
Note down the public URL of the S3 bucket and click on URL

![Website](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-10-s3-public-endpoint.png?raw=true)

## Set up continuous deployment of the site

### 1.) Go to Services > CodeBuild, then click on "Create Project"

### 2.) In the section "Configure your Project", enter a unique "Project name". Something like `yourname-module3-devopsgirls`, and enter a "Description". In the section "Source: What to build", choose `AWS CodeCommit` as the "Source provider", and choose the CodeCommit repository you created in Module 1. 

![CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-11-create-codebuild-project.png?raw=true)

### 3.) In the following sections, choose/enter the following values for the fields, and click "Continue"
- Environment: How to build
  - Environmnet image - `Use an image managed by AWS CodeBuild`
  - Operating System - `Ubuntu`
  - Runtime - `Base`
  - Runtime version - `aws/codebuild/ubuntu-base:14.04`
  - Privileged - Do not select check box
  - Build specification - `Insert build commands`
  - Build commands - `aws s3 sync --acl public-read . s3://yourname-devopsgirls-site/ --exclude *.git*` [Update the command with the bucket name of your site you created in Section 1]
  - Certificate - `Do not install any certificate`
- Artifacts: Where to put the artifacts from this build project
  - Type - `No artifacts`
- Cache
  - Type - `No cache`
- Service role
  - Choose `Create a service role in your account`, and choose a unique role name like `yourname-module3-devopsgirls`
- VPC
  - VPC - `No VPC`

![CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-12-create-codebuild-project.png?raw=true)

### 4.) In the "Review and Build" page, click on "Save" to save the project

![CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-13-create-codebuild-project.png?raw=true)

### 5.) Go to Services > IAM, then click on "Policies", and search for `yourname-module3-devopsgirls`, and click on the policy. We are now going to update the policy and give CodeBuild the permissions to copy your website files to your S3 bucket.

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-14-modify-codebuild-iam-policy.png?raw=true)

### 6.) Click on "Edit policy"

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-15-modify-codebuild-iam-policy.png?raw=true)

### 7.) Click on "Add additional permissions"

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-16-modify-codebuild-iam-policy.png?raw=true)

### 8.) In "Select a service below", type S3, and choose the S3 service

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-17-modify-codebuild-iam-policy.png?raw=true)

### 9.) In "Specify the actions allowed in S3" choose `PutObject`, `PutObjectAcl` and `PutObjectVersionAcl` as shown in the image below

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-18-modify-codebuild-iam-policy.png?raw=true)

### 10.) In the "Resources" section, choose the "Specific" radio button and click on "Add ARN". In the popup, for "Bucket name" enter the name of the bucket of your site you created in Section 1, and click on "Add"

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-19-modify-codebuild-iam-policy.png?raw=true)

### 11.) Leave the field "Request conditions" at its defailt, and click on "Review policy"

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-20-modify-codebuild-iam-policy.png?raw=true)

### 12.) Review the changes and click on "Save changes"

![CodeBuild IAM Policy](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-21-modify-codebuild-iam-policy.png?raw=true)

### 13.) Go back to Services > CodeBuild, then click on the Build Project you created in the steps above.

![Run CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-22-run-codebuild-project.png?raw=true)

### 14.) Click on "Start Build"

![Run CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-23-run-codebuild-project.png?raw=true)

### 15.) In the "Start new build" page, leave the default values for "Project name", "Source provider", "Repository" and "Git clone depth". For "Branch" choose `master`, and leave "Source version" at the default value populated for your branch. Click on "Start Build"

![Run CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-24-run-codebuild-project.png?raw=true)

### 16.) The build should run successfully, and you should be able to see build logs like shown below

![Run CodeBuild Project](https://github.com/DevOps-Girls/devopsgirls-bootcamp3/blob/master/images/3-3-serverless-static-site/3-3-25-run-codebuild-project.png?raw=true)

### 17.) Go to the public URL of your S3 bucket, and you should see your web site.

### 18.) Your website is now set to be continuously deployed. If you make a modification to your website files, and push it to your CodeCommit repository, you should see a new run of your CodeBuild project which should successfully deploy the change to S3.