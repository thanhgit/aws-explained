# Code pipeline
Code pipeline same as continuous delivery

![image](https://user-images.githubusercontent.com/21302811/125705749-68a11118-00a4-4ed4-a45e-a3b4588530f1.png)


## Benefits
- ### Visual workflow
- ### Source: `Github`, `CodeCommit`, `S3`
- ### Build: `CodeBuild`, `Jenkins CI`, ...
- ### Load Testing: 3rd party tools
- ### Deploy: `CodeDeploy`, `Beanstalk`, `CloudFormation`, `ECS`, ...

## CodePipeline artifacts
- ### Each pipeline stage can create `artifacts`
- ### Artifacts are passed stored in Amazon S3 and passed on to the next stage
![image](https://user-images.githubusercontent.com/21302811/125705976-43c91202-7e71-45ad-9a21-0156767cacbb.png)
