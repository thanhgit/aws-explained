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
