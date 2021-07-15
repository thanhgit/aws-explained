# Code build

## Overview
- ### Fully managed build service
- ### Alternative to other build tools such as Jenkins
- ### Continuous scaling (no servers to manage or provision - no build queue)
- ### Pay for usage: the time it takes to complete the builds
- ### Secure: integration with `KMS` for encryption of build artifacts, `IAM` for build permissions and `VPC` for network security, `CloudTrail` for API calls logging

## Componenents
- ### Source code from `Github`, `CodeCommit`, `S3`
- ### Build instructions can be defined in code (`buildspec.yml` file)
- ### Output logs to `AWS S3 & AWS CloudWatch logs`
- ### Use CloudWatch Alarms to detect failed builds and triggers notifications
- ### CloudWatch Event and Lambda as a glue
- ### SNS notifications
- ### Builds can be defined within CodePipeline or CodeBuild itself

## How it works
![image](https://user-images.githubusercontent.com/21302811/125722507-fbf63a72-4e50-4a38-9bcb-fbc30a92a06b.png)

## In-depth `buildspec.yml` file
- ### Define environment variables
  - ### Plaintext
  - ### Secure secrets: use `SSM Parameter Store`
- ### Phases (specify commands to run)
  - ### Install: `install dependencies`
  - ### Pre build: `commands to execute before build`
  - ### Build: `actual build commands`
  - ### Post build: `such as zip output`
- ### Artifacts: `What to upload to S3`
- ### Cache: `file to cache to S3 for future build speedup`
