# docker
docker notes and projects

# notes
docker files > series of commands, copy, install, running main process (starting point)

docker image > code, runtime, libraries, env. variables, config files (executable package); inherit from a base image containing pre-installed dependencies that you can build on
- publicly available images available 

containers > runnable instance of an image
- deploy on your local machine or cloud provider

container orchestration system > docker swamp, k8s, ECS; if you need containers to run constantly 

# Project_1: following the YouTube video giving a docker tutorial (https://www.youtube.com/watch?v=1_AlV-FFxM8&t=579s)
confirmed at docker was installed correctly: `docker --version`

initiated docker to build the dependencies necessary to build a container: `docker init`

building the container:`docker build -t first-time .`

I was able to get the container created, but the application is not running. I am attempting to troubleshoot using AI (gemini). Successfully able to run the container and verify the app was running. Screenshots. 

running the container: `docker run -p 127.0.0.1:8080:8080 first-time`

volumes: 

## Mapping a folder from my local machine to the container
 `docker run -v C:\Users:/container_folder first-time`

## Creating a shared volume
`docker volume create shared_volume`
`docker run -v shared_volume:/my_path first-time`

## Deploying multiple containers using docker compose
I had the biggest issue here with the code. I keep getting an error message with container stack only half deployed. The issue is the password.txt file is not being read by properly by Docker. Trying to use AI to troubleshoot, but no luck. I will move past and revisit

## Deploying container to aws 

1. Logged into ECR via docker terminal: `aws ecr get-login-password --region <region>| docker login --username AWS --password-stdin <awsaccountname>.dkr.ecr.<region>.amazonaws.com`

2. Tagged the image: `docker tag first-time:latest <awsaccountname>.dkr.ecr.<region>.amazonaws.com/first-time:latest`

3. Created a repository in the AWS console

4. Pushed the image to ECR `docker push <awsaccountname>.dkr.ecr.<region>.amazonaws.com/first-time:latest`

5. Created a cluster "cluster-demo-2". The first time I tried to create the cluster is failed. After researching, I learned that I needed to create an IAM for cluster. In the console, I went to IAM > Roles > Create role > Trusted entity type: AWS service > Use case: Elastic Container Service. I gave the role a name and created it. Upon retrying the cluster, it was a success. 

6. Created the task definition "flask-definition"

7. Assigned service name to deploy the container: flask-app-service-u8oap0gc

8. Container is running, but I had to configure a security group to allow access from the port my container was listening on. Success!
