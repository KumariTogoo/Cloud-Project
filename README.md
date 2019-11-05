# Cloud-Project

### Implementing Infrastructure as code using AWS Cloudformation
### Run contanarized Dockerfile on Amazon ECS using AWS CloudFormation & CLI/AWS Console

Create and run docker container on Amazon ECS using CloudFormation and CLI.

- Containerize node.js application
- Use AWS CLI to create Amazon ECR repository
- Build docker image and push to ECR
- CloudFormation stack to create VPC, Subnets, InternetGateway etc
- CloudFormation stack to create IAM role
- CloudFormation stack to create ECS Cluster, Loadbalancer & Listener, Security groups etc
- CloudFormation stack to deploy docker container

### Code

```
mkdir ~/projs
git clone https://bitbucket.org/network-international/node-js-getting-started/src/master/ node-app
cd node-app
```

### Dockerize Application

```
# Run on local
docker build -t cloud-assignment ./app/
docker run -it -p 5000:5000 --rm cloud-assignment:latest
open http://localhost:5000/
```

### Push Docker Image to ECR

```
aws ecr create-repository --repository-name cloud-project
aws ecr get-login --no-include-email | sh
IMAGE_REPO=$(aws ecr describe-repositories --repository-names cloud-project --query 'repositories[0].repositoryUri' --output text)
docker tag cloud-project $IMAGE_REPO:v1
docker push $IMAGE_REPO:v1
```

### Create CloudFormation Stacks

```
Created a project on github Cloud-Project
https://github.com/KumariTogoo/Cloud-Project.git
Added files vpc.yml, iam.yml, app-cluster.yml and app.yml and moved the content of node-app to current clone(just to keep the dockerfile).

aws cloudformation create-stack --template-body file://$PWD/infra/vpc.yml --stack-name vpc

aws cloudformation create-stack --template-body file://$PWD/infra/iam.yml --stack-name iam --capabilities CAPABILITY_IAM

aws cloudformation create-stack --template-body file://$PWD/infra/app-cluster.yml --stack-name app-cluster

# Edit the app.yml to update Image tag/URL under Task > ContainerDefinitions and,
aws cloudformation create-stack --template-body file://$PWD/infra/app.yml --stack-name app
```
(OR)

```
Login to AWS Console -> Go to Services -> Cloudformation
Create Stack -> Upload file to create vpc, iam, app-cluster, app
```

Copy the `WebAppEndpoint` value from `app` stack output on AWS Management Console. Make a request to that URL on browser.
