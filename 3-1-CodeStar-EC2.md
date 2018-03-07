# Deploy a static website to EC2 using CodeStar

## Learning Aims
### 1. Introduction to Continuous Integration, Delivery, and Deployment
### 2. Introduction to AWS CodeStar, CodeCommit, and CodeDeploy

## Key concepts
- "*Continuous Integration*  is a software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible." - Martin Fowler, Chief Scientist, ThoughtWorks.
- "*Continuous Delivery* is the natural extension of Continuous Integration, an approach in which teams ensure that every change to the system is releasable, and release any version with the push of a button. Continuous Delivery aims to make releases boring, so that we can deliver frequently and get quick feedback on what users care about." - Thoughtworks.
- "*Continuous Deployment* is the next step of continuous delivery: Every change that passes the automated tests is deployed to production automatically. Continuous deployment should be the goal of most companies that are not constrained by regulatory or other requirements." - Puppet Labs

## Key technologies used
- AWS CodeStar: https://aws.amazon.com/codestar/
- AWS EC2: https://aws.amazon.com/ec2/
- AWS CodeCommit: https://aws.amazon.com/codecommit/
- AWS Code: https://aws.amazon.com/codedeploy/
- Git: https://git-scm.com/

## Module Overview
- Setup and a deploy a new static website project in AWS CodeStar
- Download static files from an AWS S3 bucket. These files will be used to customise the static website
- Commit and push downloaded static files to CodeCommit repository using Git
- Observe CodeDeploy trigger a deployment from the detected Git push
- Marvel at your website available for the world to see  

![Git to EC2](/images/3-1-ec2-static-site/CCtoEC2.svg) 

## Prerequisites
You will need the following software installed on your workstation:  
- [Git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

**How to Install Git Client**

- For **Windows** download the latest installer [here](https://gitforwindows.org/) . Once the file has been downloaded successfully, double click on the downloaded file to start the installation process, configuring it with the default options.  
 
- For **macOS** download the latest installer from [here](https://sourceforge.net/projects/git-osx-installer/files/). Once the file has been downloaded successfully, start the installation process configuring it with the default options.  

- For **Debian/Ubuntu** using apt-get, open a terminal command line and run:

```javascript
sudo apt-get update
sudo apt-get install git
```

- For **all platforms**, confirm the installation was successful by typing:

```javascript
git --version
```

**Note**: If you would like to know more about how to use git [here is a link to a book](https://git-scm.com/book/en/v2) to understand everything about **git**. Enjoy it ;)!

Also you will need a **Text editor** such as [Sublime](www.sublimetext.com/3) or [Visual Studio Code](https://code.visualstudio.com/download). 

## You'll be a CodeStar in no time
### 1. Log in to AWS
1.1. Open a web browser and go to https://console.aws.amazon.com/console/home. Log in using your supplied account credentials.
![AWS Signin](/images/3-1-ec2-static-site/1_awssignin.png)  
### 2. Ensure you're in the right AWS region
2.1. On the top-right side, make sure you select *Sydney*.  
![AWS Signin](/images/3-1-ec2-static-site/region.png)  
### 3. Create CodeStar Project
3.1. Under the AWS services search field, search for *codestar*, and click on the CodeStar auto drop down element  
3.2. Click on the '+ Create a new project' link  
![CodeStar Create Project](/images/3-1-ec2-static-site/2_createcodestarproj.png)  
3.3. On the left hand side, under Application Category, select *Static Website*  
3.4. Under *Choose a project template*, click on 'HTML - Static Website - Amazon EC2'  
3.5. Under the *Project name* field enter your first name, and last name without a space. For example *sallytran*  
3.6. Select *AWS CodeCommit* as your source code repository  
3.7. Under the *Repository name* field, enter the same value you did in step 3.5. I.e. *firstnamelastname*  
3.8. Click the *Next* button  
![CodeStar Project Name](/images/3-1-ec2-static-site/3_projectname.png)  
3.9.1. On the *Review project details* page, click the *Edit Amazon EC2 Confirguration link*. Choose a t2.micro instance. Choose the VPC ID, and Subnet ID by referencing the table below. To find the correct VPC ID, look in the AWS Account ID column, and find the row that contains the AWS Account ID that has been supplied to you via email. For the Subnet, you can choose either Public Subnet 1 or Public Subnet 2 from the row that features your supplied AWS Account ID.  

| AWS Account ID | VPC ID | Public Subnet 1 ID | Public Subnet 2 ID |
| ------------ | ------------ | --------------- | --------------- |
| 522028331501 | vpc-d83c9ebf | subnet-2cdeb14b | subnet-cc56ce85 |
| 976007552049 | vpc-a33c9ec4 | subnet-e9dfb08e | subnet-604cd429 |
| 214953911892 | vpc-c53a98a2 | subnet-f74bd3be | subnet-6ec3ac09 |

![Edit EC2 Options](/images/3-1-ec2-static-site/5_editec2options.png)  
3.9.2. On the *Review project details* page, observe the AWS tools that will be utilised and connected by CodeStar. In this case, you should see that:
* CodeCommit is being used for Source Control Management
* CodeDeploy is being used for Application Deployment, and
* CloudWatch is being used for Monitoring

3.10. Click on the *Create Project* button  
![Review Project](/images/3-1-ec2-static-site/4_reviewproject.png)  
3.11. When you see the *Choose an Amazon EC2 Key Pair*, select *devops_girls* from the dropdown list, tick the acknowledgement box, and click the *Create Project* button (Normally you would make very sure you have access to this key in case you ever needed access to the EC2 instance)  
![Choose EC2 Key](/images/3-1-ec2-static-site/6_chooseec2key.png)  
3.12. On the *It looks like this is your first time here.* screen, enter your name in the *Display Name* field, and an accessible email address in the *Email* field  
3.13 On the *Pick how you want to edit your code* choose *Command line tools* and click the *See instructions* link  
![Pick your tools](/images/3-1-ec2-static-site/7_picktools.png)   
3.14 On the *Connect to your tools*, choose the Operating System that applies to you. For example Windows  
3.15 Choose *HTTPS* as the Connection Method  
3.16. Right click on the Generate Git Credentials *here* link and open the link in a new browser tab  
![Connect your tools](/images/3-1-ec2-static-site/8_connecttools.png)  
3.17. Under the *HTTPS Git credentials for AWS CodeCommit* click the Generate button. Save your login details somewhere securely such as Keepass. For the purposes of this exercise, a text file will be okay  
![Generate CodeCommit HTTPS Credentials](/images/3-1-ec2-static-site/9_generategitcreds.png)  
![CodeCommit HTTPS Credentials](/images/3-1-ec2-static-site/10_gitcredscreated.png) 
3.18. Go back to your other browser tab, and click the *copy* link next to the CodeCommit repository URL  
3.19. Open up a command line terminal and navigate to a directory where you wish to clone the new repository into  
3.20. Paste the git clone command and press enter  
3.21. Enter your CodeCommit credentials as previously saved  
![git clone](/images/3-1-ec2-static-site/gitclone.png)
3.22. Click the *Back* button on the AWS Choose your tools page  
3.23. On the *Pick how you want to edit your code*, press the *Skip* button  
3.24. Now CodeStar will start deploying your website via CodeDeploy  
3.25. Refresh the CodeStar Dashboard to see what is happening  
3.26. Have a look at the Commit history panel, the Continuous Deployment, and the Application endpoint panel  
3.27. Right click on the endpoint URL, and open the link in a new tab. You will see the standard AWS Blueprint template website  
![CodeDeploy in action](/images/3-1-ec2-static-site/13_codedeploy.png)

### 4. Download custom static HTML file from AWS S3
4.1. Open your web browser and paste in the following URL, https://s3-ap-southeast-2.amazonaws.com/devopsgirls1/index.html  
4.2. Click on the new tab, then right click anywhere on the webpage and select *Save as...*  
4.3. Change the filename to be index.html and save it in the webpage directory of your cloned git project repository on your workstation. For example, sallytran/webpage/index.html  
![Save index.html file](/images/3-1-ec2-static-site/11_saveindex.png)  
4.4 In a similar manner, save https://s3-ap-southeast-2.amazonaws.com/devopsgirls1/DevOpsGirls.png and https://s3-ap-southeast-2.amazonaws.com/devopsgirls1/Thumbs-Up.svg. Make sure you don't change their names.  
4.5 Create an `index_files` subdirectory and move DevOpsGirls.png and Thumbs-Up.svg into it. `index_files` should be at the same level as `index.html`, i.e. you should end up with this file structure:
```
          └── webpage
              ├── index.html
              ├── index_files
              │   ├── DevOpsGirls.png
              │   └── Thumbs-Up.svg
              ├── (other things...)
```
    
### 5. Push changes to CodeCommit Repo
5.1. In your command terminal, navigate to the home (root) directory of your CodeCommit repository on your workstation. For example:
`cd C:\sallytran`  
5.2. 'Add' the modified files to be ready for git commit. Enter this command:
`git add .`  
5.3. Commit the changes to your local git repository, including a comment. Enter this command:
`git commit -m "Replaced the index.html file"`  
5.4. Push the changes to the AWS CodeCommit hosted repository:
`git push`  
5.5. Enter your CodeCommit Git username  
5.6. Enter your CodeCommit Git password  
![git push](/images/3-1-ec2-static-site/12_gitpush.png)

### 6. Observe CodeDeploy redeploy your website with the changes
6.1. Go back to your web browser and open up the AWS CodeStar Dashboard for your project, and refresh the page (Ctrl-F5)
6.2. Have a look at the Commit history panel, the Continuous Deployment , and the Application endpoint panel  
6.3. Right click on the endpoint URL, and open the link in a new tab. You will see your newly customised website has been deployed to the cloud!
![CodeDeploy](/images/3-1-ec2-static-site/13_codedeploy.png)





