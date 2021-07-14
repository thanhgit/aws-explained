# AWS CI/CD with code commit, code artifact, code build, code deploy, code pipeline


## Code deploy
- ### There are several ways to handle deployments using open source such as ansible, terraform, chef, puppet, ...
- ### We can use the managed service AWS CodeDeploy
### Use case:
- #### Deploy your apps automatically to many EC2 instances
  - Each EC2 machine must be running the CodeDeploy agent
  - CodeDeploy sends `appspec.yml` file
  - Application is pulled from github or S3
  - EC2 will run the deployment instructions
  - CodeDeploy agent will report of success/ failure of deployment on the instance 
  ![image](https://user-images.githubusercontent.com/21302811/125422291-092fe3ba-e185-4600-a80b-0e88fff5b443.png)
- ### Deploy your apps to deployment group with tags (dev / test / prod)
  - Code deploy can be chained into `Code pipeline` and use artifacts from there
  - Using `Beanstalk`. For example. https://dev.to/frosnerd/deploying-an-http-api-on-aws-using-elastic-beanstalk-5dh7
  - ![image](https://user-images.githubusercontent.com/21302811/125701165-48a86ee9-db6e-437f-9157-2c132b5f507f.png)
  - Note: `Blue / Green` only works with EC2 instances (not on premise)
  - Support for AWS Lambda deployments 
### Primary components
- ### Application: `unique name`
- ### Compute platform: `EC2/ On-premise or Lambda`
- ### Deployment configuration: `deployment rules for success / failures`
  - ### EC2/ On-premise: you can specify the minimum number of healthy instances for the deployments
  - ### AWS Lambda: specify how traffic is routed to your updated lambda function versions
- ### Deployment group: `group of tagged instances (allows to deploy gradually)`
- ### Deployment type: `in-place deployment of Blue/ Green deployment`
- ### IAM instance profile: `need to give EC2 the permissions to pull from S3 / Github`
- ### Application revision: application code + `appspec.yml` file
- ### Service role: `role for code deploy to perform what it needs`
- ### Target revision: `target deployment application version`

#### In-depth appspec.yml file
- #### File section: how to source and copy from S3 / Github to filesystem
- #### Hooks: set of instructions to do to deploy the new version
  - #### ApplicationStop
  - #### DownloadBundle
  - #### BeforeInstall
  - #### AfterInstall    
  - #### ApplicationStart
  - #### ValidateService: `really important`
  - ![image](https://user-images.githubusercontent.com/21302811/125702310-62ca1bac-39ab-4491-bce1-620988cc4608.png)

#### In-depth deployement
- #### In place deployment - `Half at a time`
![image](https://user-images.githubusercontent.com/21302811/125702526-d68a93d1-a998-4119-a8b8-9ac034a02632.png)
- #### Blue green deployemnt 
![image](https://user-images.githubusercontent.com/21302811/125702708-31adcd32-5876-4f85-9650-0cdcfc50b8f6.png)

