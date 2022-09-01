# CICD with Jenkins

![Screenshot](Screenshot%202022-09-01%20at%2011.01.13.png)

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
