# Simple-React-application
Docker 

 we need to check if we have node js to check npm -v if version there not then install
````
node --version
npm --v
curl https://nodejs.org/dist/v20.11.0/node-v20.11.0-linux-x64.tar.xz
ls
````
now to create react app will use below command 

npx create-react-app frontend

## Creating the Dev Dockerfile

- now we have to name file inside frontend so 

````
ls
cd frontend
code .

in vs code create docker file
Dockerfile.dev

````

- now we name the file docker.dev so

````
 docker build .                      #will no work
docker build -f Dockerfile.dev .    # -f means we are spicifing file to be build 
  
````

- we have removed the duplicate wo copies of dependencies, and we really do not need two.The easiest solution here is to delete the "node modules" folder inside of our working directory .which I'm gonna do right now.so I'll delete that entire folder by moving it to trash.

````
docker run e79c31d7f07d249031b1dfa08458dcb5a673eb96de07595a0a5c0a4 container id
- Now if we run the contianer it will not work we need to open or expose the port

docker run -p 3000:3000 e79c31d7f07d249031b1dfa08458dcb5a673eb96de07595 container id  
````
web localhost:3000

## Docker Volumes

To be able to update the src code we have to make configration so every time when we chnage the src code we dont have to rebuild the image

With a Docker volume, we're essentially setting up a **mapping** from a **folder inside the container** to a folder **outside the container**. Now you might be wondering, why did we not just make use of volumes before if this kind of solves the issue.


to run docker volume we have to execute long cmds 

1) -v Means volume $(pwd)  we're essentially saying get the present working directory or the path to it, and take this folder, like this front end folder and everything inside of it, and map it up to the app folder

2) earlier we have deleted node modules folder hence this 1 is not working hence we are provide path

3) when we use {:} colun that mean copy the path and paste it to now we dont want to copy we just want to execute
 
````
docker run -p 3000:3000 -v $(pwd):/app 5906c40a635ff25c69a33c239195db9eca6ae7788a46175798c9cf3  #this cmd will not work

docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app 5906c40a635ff25c69a33c239195db9eca6ae7788a46175798c9cf3 this is not working too its not good way it use long cmd  
````

4) with the help of docker compse we can execute multiple cmds
   
## docker-compse.yml

````
docker-compose up
````
5) now we are able make  chnages on src code without building contianer  again.

## Executing Tests

````
docker build -f Dockerfile.dev

docker run c2e9788812f39f7120abf76e7de077c969b3 npm run test  
#When we run Docker run by default we get a connection to standard out inside the container.

So will use -it flag
docker run -it c2e9788812f39f7120abf76e7de077c969b3 npm run test

now with nice view we can now able to execute

````

copy and pastes the same two times 

````

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
````
now we made changes in app.test.js and hit enter it still getting only 1 test 

We've got a container that's been created specifically to run some tests. When we created that container, we essentially took a snapshot of all of our working files and folders, and put that inside the container. So, this very temporary container that we've made just to run our tests, does not have all that volume stuff set up. That is the issue. And so, without any of those volumes set up, we are using old and outdated files inside of our container.

so it only taking old chnages 


## NGINX 

We not able to start our same Dev Server in Prod Enivorment becoz we are not making any chnage to java code so will create the same container in prod env to run we will use ngnix

NGINX is an extremely popular web server. It doesn't have a lot of logic tied to it. It's really just about taking incoming traffic and somehow routing it or somehow responding to it with some static files, which is exactly what you and I are going to use it for. So we are going to create a separate Docker file


we are going to craete seprate docker file 

## Multi-Step Docker Builds

We will USE 2 Images in one file bcoz we can not use same **Alpine** image for **Nginx**

**BUILD PHASE** AND** RUN PHASE** 
Now when we copy it over to the run phase, everything else that occurred during that build phase, like the use of the node alpine image, the dependencies that we installed, all that additional stuff gets dropped,



## We execute some number of commands inside that set of layers and then outta that set of layers, we're just copying over just the bare minimum, just the stuff we care about. So when we do this copy step right here we're essentially dumping everything else that was created while this set of configuration was executed.

````
FROM node:16-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

#/app/build all the stuff for run phase copy we are coping file from builder phase and paste it to /app/build and to run anything in 
#nginx we have to paste the path which is /usr/share/nginx/html we dont have to metion start nginx default start 


FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html

````


we have created Dockerfile will run as defult nginx port is 80

```
code .
ls
docker build .
docker run -p 8080:80 12afef371358354f5a832e24693d98cfaaff6671a9fa
````
## Section 07

## Github Setup

We're gonna set up first a GitHub repository, that's going to house all of our code. We're then gonna set up a local Git repository. We're gonna connect the local Git repository on your computer to the GitHub remote that we're gonna set up. And then we're gonna take all of our local work and push it up to GitHub.

````
 git init
 git add .
 git commit -m "initial commit"
 git config user.name [ roshanwaghmare ]
 git config --global user.email [ wroshan960@gmail.com ]
 git commit -m "initial commit"
 git remote add origin https://github.com/roshanwaghmare/docker-react.git
 git push origin master
 git remote set-url origin https://classictoken@github.com/roshanwaghmare/docker-react
 git push origin master

````
## TRAVIS CI 

- The entire idea of Travis CI is to watch for any time that you and I push some changes or some amount of code to the GitHub project that we just created. Anytime that we push up some new code GitHub is essentially gonna tap on the shoulder of Travis and it's gonna say, Hey this person just updated their GitHub repository, there's some new code here or some new version of their application. Travis CI will then automatically pull down all of the code inside of our GitHub repository, and at that point it essentially gives us the ability to do some work.

- goto Travisci.org
- connect with guthub
- we have to give Travis a little bit of direction, meaning we have to tell it how to essentially start up Docker, how to run our test suite and how to interpret the results.

- we will create yaml.file in travix for direction we not gonna make any test to dockerfile it's for prod we will test** Dockerfile.dev**

- 88

- 
## Elastic Beanstalk Setup and Configuration
Please use the following instructions to create an Elastic Beanstalk environment. Since the AWS UI and options change frequently, these are provided as a lecture note instead of a video lecture.

These instructions contain four important sections - please do not skip any of them.

1. Creating EC2 Instance Profile

2. Elastic Beanstalk Environment Creation

3. S3 Bucket Configuration

4. Required Updates for Docker Compose

Creating EC2 IAM Instance Profile
The first thing we need to do is to create an EC2 IAM Instance Profile. This is something that AWS used to generate automatically when creating the environment but now must be created manually.

1. Go to AWS Management Console

2. Search for IAM and click the IAM Service.


3. Click Roles under Access Management in the left sidebar.


4. Click the Create role button.


5. Select AWS Service under Trusted entity type.


Then select EC2 under common use cases and click Next.


6. Search for AWSElasticBeanstalk and select the AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier and AWSElasticBeanstalkMulticontainerDocker policies. Click the Next button.

7. Give the role the name of aws-elasticbeanstalk-ec2-role

8. Click the Create role button.

Elastic Beanstalk Environment Creation
After you've created this Instance Profile, you will need to create an Elastic Beanstalk environment.

1. Go to AWS Management Console

2. Search for Elastic Beanstalk and click the Elastic Beanstalk service.


3. If you've never used Elastic Beanstalk before you will see a splash page. Click the Create Application button. If you have created Elastic Beanstalk environments and applications before, you will be taken directly to the Elastic Beanstalk dashboard. In this case, click the Create environment button.


4. You will need to provide an Application name, which will auto-populate an Environment Name.


5. Scroll down to find the Platform section. You will need to select the Platform of Docker. This will auto-select several default options. Change the Platform branch to Docker running on 64bit Amazon Linux 2. The new 2023 branch currently has issues with single-container deployments.


6.  Scroll down to the Presets section and make sure that free tier eligible has been selected:


7. Click the Next button.

8. You will be taken to a Service Access configuration form.

Select Create and use new service role and name it aws-elasticbeanstalk-service-role. You will then need to set the EC2 instance profile to the aws-elasticbeanstalk-ec2-role created earlier (this will likely be auto-populated for you):


9. Click the Skip to Review button.

10. Click the Submit button and wait for your new Elastic Beanstalk application and environment to be created and launch.

S3 Bucket Configuration
After you have created an Elastic Beanstalk environment, you will need to modify the S3 bucket.

1. Go to AWS Management Console

2. Search for S3 and click the S3 service.


3. Find and click the elasticbeanstalk bucket that was automatically created with your environment (Your bucket region will likely be different)


4. Click Permissions menu tab


5. Find Object Ownership and click Edit


6. Change from ACLs disabled to ACLs enabled. Change Bucket owner Preferred to Object Writer. Check the box acknowledging the warning:


7. Click Save changes.

Required Updates for Docker Compose
This new Amazon Linux platforms will conflict with the project we have built since it will look for a docker.compose.yml file to build from by default instead of a Dockerfile.

To resolve this, please do the following:

1. Rename the development Compose config file

Rename the docker-compose.yml file to docker-compose-dev.yml. Going forward you will need to pass a flag to specify which compose file you want to build and run from:
docker-compose -f docker-compose-dev.yml up
docker-compose -f docker-compose-dev.yml up --build
docker-compose -f docker-compose-dev.yml down

2. Create a production Compose config file

Create a docker-compose.yml file in the root of the project and paste the following:

version: '3'
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '80:80'
Elasticbeanstalk will see a file named docker-compose.yml and use it to build the single container application.

------------------------------------------------------------
- 
## AWS Configuration Cheat Sheet

## AWS Configuration Cheat Sheet
This lecture note is not intended to be a replacement for the videos, but to serve as a cheat sheet for students who want to quickly run thru the AWS configuration steps or easily see if they missed a step. It will also help navigate through the changes to the AWS UI since the course was recorded.

Docker Compose config Update

1. Rename the development docker-compose.yml file to docker-compose-dev.yml.
2. Create a production docker-compose.yml file in the root of the project and paste the following:

version: '3'
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '80:80'


Create EC2 IAM Instance Profile

1. Go to AWS Management Console

2. Search for IAM and click the IAM Service.

3. Click Roles under Access Management in the left sidebar.

4. Click the Create role button.

5. Select AWS Service under Trusted entity type. Then select EC2 under common use cases.

6. Search for AWSElasticBeanstalk and select the AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier and AWSElasticBeanstalkMulticontainerDocker policies. Click the Next button.

7. Give the role the name of aws-elasticbeanstalk-ec2-role

8. Click the Create role button.



Create Elastic Beanstalk Environment

1. Go to AWS Management Console

2. Search for Elastic Beanstalk and click the Elastic Beanstalk service.

3. If you've never used Elastic Beanstalk before you will see a splash page. Click the Create Application button. If you have created Elastic Beanstalk environments and applications before, you will be taken directly to the Elastic Beanstalk dashboard. In this case, click the Create environment button. There is now a flow of 6 steps that you will be taken through.

5. You will need to provide an Application name, which will auto-populate an Environment Name.

6. Scroll down to find the Platform section. You will need to select the Platform of Docker. This will auto-select several default options. Change the Platform branch to Docker running on 64bit Amazon Linux 2. The new 2023 branch currently has issues with single-container deployments.

7. Scroll down to the Presets section and make sure that free tier eligible has been selected:

8. Click the Next button to move to Step #2.

9. You will be taken to a Service Access configuration form.

Select Create and use new service role and name it aws-elasticbeanstalk-service-role. You will then need to set the EC2 instance profile to the aws-elasticbeanstalk-ec2-role created earlier (this will likely be auto-populated for you).

10. Click the Skip to Review button as Steps 3-6 are not applicable.

11. Click the Submit button and wait for your new Elastic Beanstalk application and environment to be created and launch.

12. Click the link below the checkmark under Domain. This should open the application in your browser and display a Congratulations message.



Update Object Ownership of S3 Bucket

1. Go to AWS Management Console

2. Search for S3 and click the S3 service.

3. Find and click the elasticbeanstalk bucket that was automatically created with your environment.

4. Click Permissions menu tab

5. Find Object Ownership and click Edit

6. Change from ACLs disabled to ACLs enabled. Change Bucket owner Preferred to Object Writer. Check the box acknowledging the warning.

7. Click Save changes.



Add AWS configuration details to .travis.yml file's deploy script

1. Set the region. The region code can be found by clicking the region in the toolbar next to your username.

eg: 'us-east-1'

2. app should be set to the Application Name (Step #4 in the Initial Setup above)

eg: 'docker'

3. env should be set to the lower case of your Beanstalk Environment name.

eg: 'docker-env'

4. Set the bucket_name. This can be found by searching for the S3 Storage service. Click the link for the elasticbeanstalk bucket that matches your region code and copy the name.

eg: 'elasticbeanstalk-us-east-1-923445599289'

5. Set the bucket_path to 'docker'

6. Set access_key_id to $AWS_ACCESS_KEY

7. Set secret_access_key to $AWS_SECRET_KEY



Create an IAM User

1. Search for the "IAM Security, Identity & Compliance Service"

2. Click "Create Individual IAM Users" and click "Manage Users"

3. Click "Add User"

4. Enter any name you’d like in the "User Name" field.

eg: docker-react-travis-ci

5. Click "Next"

6. Click "Attach Policies Directly"

7. Search for "beanstalk"

8. Tick the box next to "AdministratorAccess-AWSElasticBeanstalk"

9. Click "Next"

10. Click "Create user"

11. Select the IAM user that was just created from the list of users

12. Click "Security Credentials"

13. Scroll down to find "Access Keys"

14. Click "Create access key"

15. Select "Command Line Interface (CLI)"

16. Scroll down and tick the "I understand..." check box and click "Next"

Copy and/or download the Access Key ID and Secret Access Key to use in the Travis Variable Setup.



Travis Variable Setup

1. Go to your Travis Dashboard and find the project repository for the application we are working on.

2. On the repository page, click "More Options" and then "Settings"

3. Create an AWS_ACCESS_KEY variable and paste your IAM access key from step #13 above.

4. Create an AWS_SECRET_KEY variable and paste your IAM secret key from step #13 above.



Deploying App

1. Make a small change to your src/App.js file in the greeting text.

2. In the project root, in your terminal run:

git add.
git commit -m “testing deployment"
git push origin main
3. Go to your Travis Dashboard and check the status of your build.

4. The status should eventually return with a green checkmark and show "build passing"

5. Go to your AWS Elastic Beanstalk application

6. It should say "Elastic Beanstalk is updating your environment"

7. It should eventually show a green checkmark under "Health". You will now be able to access your application at the external URL provided under the environment name.






