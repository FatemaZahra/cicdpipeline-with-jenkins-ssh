# CICD with Jenkins

![Screenshot](images/Screenshot%202022-09-01%20at%2011.01.13.png)

## CI

The "CI" in CI/CD always refers to continuous integration, which is an automation process for developers. Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other.

## CD

Continuous delivery usually means a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code.

Continuous deployment (the other possible "CD") can refer to automatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery. It builds on the benefits of continuous delivery by automating the next stage in the pipeline.

![Screenshot 2022-09-01 at 11 22 32](https://user-images.githubusercontent.com/102330725/187892064-e451b24f-8bf0-4433-bcbc-7c0e676724fb.png)

## Jenkins

Jenkins offers a simple way to set up a continuous integration or continuous delivery (CI/CD) environment for almost any combination of languages and source code repositories using pipelines, as well as automating other routine development tasks. While Jenkins doesn’t eliminate the need to create scripts for individual steps, it does give us a faster and more robust way to integrate our entire chain of build, test, and deployment tools than we can easily build yourself.

## Webhook

A Webhook is a mechanism to automatically trigger the build of a Jenkins project in response to a commit pushed to a Git repository.

Refer to the [link](https://docs.github.com/en/developers/webhooks-and-events/webhooks/creating-webhooks)

### Steps to add a WebHook on GitHub

- Go to Settings on your repo

  ![img](images/Screenshot%202022-09-01%20at%2016.20.33.png)

- Under Code and automation, Select WebHooks

  ![img](images/Screenshot%202022-09-01%20at%2018.02.21.png)

- Go to Add WebHooks

  - Enter the Payload URL as `http://jenkins_ip:8080/github-webhook/`
  - Click on Add Webhook

    ![img](images/Screenshot%202022-09-01%20at%2017.59.49.png)

- Once added, the WebHook appears as:

  ![img](images/Screenshot%202022-09-01%20at%2018.00.19.png)

## Connection between Jenkins and GitHub

### 1. Generate a new key in your local host

- Go to your terminal
- `cd` into the SSH folder
- Generate a key following the SSH Setup till Step 6 in [link](https://github.com/FatemaZahra/github-ssh-setup)

### 2. Add key to Github Repo

- Go to your repo on Github
- Go to Settings of your repo

![Screenshot](images/Screenshot%202022-09-01%20at%2016.20.33.png)

- Go to Deploy Keys under Security

![DeployKey](images/Screenshot%202022-09-01%20at%2016.38.06.png)

- Click on Add Deploy key, Give the key a title and copy the public key generated in Step 1, paste it under Key

**Tick the checkbox for Allow write access**

![key](images/Screenshot%202022-09-01%20at%2016.53.01.png)

## Create a job in Jenkins

- Click on **New Item** under Jenkins

![new](images/Screenshot%202022-09-01%20at%2017.17.15.png)

- Enter a name and Select **Freestyle project** then Click on OK

![freestyle](images/Screenshot%202022-09-01%20at%2017.22.51.png)

- Under the **General** column
  - Add a description
  - Check **Discard old builds**, and keep the max # of builds as 3
  - Check the box for GitHub Project and add the **https** link of your repo

![img](images/Screenshot%202022-09-01%20at%2017.25.23.png)

- In **Office 365 Connector**

  - Tick the checkbox to **restrict where this project can be run**

  ![img](images/Screenshot%202022-09-01%20at%2017.30.09.png)

- Under **Source Code Management**

  - Select **Git**
  - Add the SSH URL of the repository and then add then click on Add to add the private SSH key generated when Setting up SSH connection.
  - When working on the main branch add **Branch specifier** as `*/main`

  ![img](images/Screenshot%202022-09-01%20at%2017.31.45.png)

  - When working on another branch add the branch name under **Branch Specifier** and then add Additional Behaviours

  ![img](images/Screenshot%202022-09-01%20at%2017.36.19.png)

- Under **Build Triggers**, Check the Tickbox for **GitHub hook trigger for GITScm Polling**

  ![img](images/Screenshot%202022-09-01%20at%2017.42.52.png)

- Under **Build Environment**, tick the Checkbox to **Provide Node and npm bin/folder to PATH**
  ![img](images/Screenshot%202022-09-01%20at%2017.40.26.png)

- Under **build**, select **Execute shell** and add the Commands
  ![img](images/Screenshot%202022-09-01%20at%2017.43.21.png)

- When not working on the main branch

  - Go to **Post-build Actions**
  - Select **Git Publisher**
  - Add Check on the tickboxes for **Push Only if build succeeds** and **Merge Results**
  - Save

  ![img](images/Screenshot%202022-09-01%20at%2017.44.25.png)

Once the job is created, click on Build Now

![img](images/Screenshot%202022-09-01%20at%2017.47.49.png)

Once the build is complete, click on the blue circle

![img](images/Screenshot%202022-09-01%20at%2017.49.09.png)

This would take you to the Console Output

![img](images/Screenshot%202022-09-01%20at%2017.51.04.png)

## To trigger the next job

- Go to **Post-build actions**
- Select **Build other projects**
- Add the name of the project to be triggered

![img](images/Screenshot%202022-09-02%20at%2014.54.58.png)

## Steps for CD and CDE

- Create an EC2 instance

- Create Security group
  - Add port 22 and 8080 with the jenkins IP
  - Allow port 22 for your IP
  - Port 80 for all IP
  - Port 3000 for all IP

![IMG](images/Screenshot%202022-09-02%20at%2017.55.48.png)

- Create a 3rd job in jenkins: Get the code from main branch and copy (SCP) to the EC2
- Run the script to install node with any other required dependencies
- The 3rd job must only be triggered if the second job was successful
- First iteration: run npm install and npm start manually (delivery)
- 4th job launch the app
- `pm2 kill all` - Create a 5th job to create DB_HOST=db-ip
- `npm start`
