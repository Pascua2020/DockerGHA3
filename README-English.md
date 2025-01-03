##### Hashtags #️⃣ : #devops #docker #linux #automation #ci #github-actions #dokku #java-springboot #nginx

##### English-Version README.Para leer la Versión en Español,ve a README.md

## ✅️ DockerGHA3


![Build Status](https://github.com/Pascua2020/DockerGHA/actions/workflows/main.yml/badge.svg)
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Docker](https://img.shields.io/badge/container-Docker-blue?logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-blue?logo=githubactions&logoColor=white)
![Dokku](https://img.shields.io/badge/deployment-Dokku-blueviolet?logo=dokku)
![No License](https://img.shields.io/badge/license-None-red) 
![English](https://img.shields.io/badge/language-english-blue?style=flat-square&logo=google-translate)

( Docker, GitHub Actions, Java Spring Boot, and Dokku )

![DevOps Logo](https://globalittrainers.com/wp-content/uploads/2021/06/Devops-logo1.png)

```diff
- This project demonstrates how to integrate Docker, GitHub Actions, Java Spring Boot, and Dokku to create an automated development and deployment workflow.

- The application is a basic Spring Boot service running inside a Docker container, automated with GitHub Actions, and deployed using Dokku.
```

## 1️⃣🟥 Features

#### *⚡️ Docker :*

![Docker Logo](https://dwglogo.com/wp-content/uploads/2017/09/Docker_container_engine_logo.png)

Packages the Spring Boot application in a container to ensure it runs consistently in any environment.

#### *⚡️ GitHub Actions :*

![GHA Logo](https://miro.medium.com/v2/resize:fit:1075/0*w5Fsp29pbWIUpW7Q.png)

Automates the build, test, and deployment processes for the application with each change in the repository.

#### *⚡️ Java Spring Boot :*

![Java SB Logo](https://miro.medium.com/v2/resize:fit:720/format:webp/1*MvUFlFTbiU40ae1SK69-Jg.png)

Backend framework for web application development.

#### *⚡️ Dokku :*

![Dokku Logo](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6mNvZ4G3tFQpY1qcVDQdWGSVUW5ljKhyUxfgTeFAZUX5r48Xm8M6mMf55h3IkCw1DC3ERygHIWgsvguq1cYntoluBXdW4-7W_Uhw8JHrvQIeW5T1lIGOuk7WTvkP5O-M_XR4J-6W9Gg-vfhG6B-Q6w75EaJ_eHlGvjxcbEGB3_xckw6OnTwxuBWsL-TRQ/s2800/A%20Deep%20Dive%20with%20Dokku.webp)

A Heroku-like deployment platform that uses Docker containers to manage applications easily.

### ☢️ Differences between DockerGHA 1, 2, 3, and 4 :

#### ⚠️ *All Dockerfiles are identical :*

- Use the base image busybox:latest.

- Copy a run.sh script into the container, which prints the current time in the console in an infinite loop.

- Configure the run.sh script as the container's entry point.


#### ⚠️ *Main.yml - General differences :*

*ℹ️ 1. Repositories :*


- 1 and 2 push images only to Docker Hub.

- 3 pushes only to GHCR.

- 4 pushes to both registries (Docker Hub and GHCR).


*ℹ️ 2. Automation :*

- Repositories 2, 3, and 4 use docker/metadata-action for automatic tagging, while 1 does not.


*ℹ️ 3. Image names :*

- Repository 1 has a fixed name: clockbox:latest.

- The other repositories use dynamic or specific configurations.


## 2️⃣🟧 Project Structure
```
DockerGHA3/
│
├── .github/
│   └── workflows/
│       └── main.yml             # GitHub Actions workflow
├── Dockerfile                   # Dockerfile for containerizing the app
├── README.md                    # Project documentation
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └──  DominioApplication.java   # Spring Boot main class
│   │   └── resources/
│   │       └── application.properties         # App configuration
├── target/                       # Build directory (generated by Maven/Gradle)
├── pom.xml                       # Maven configuration file (if using Maven)
└── .gitignore                    # Files and directories to ignore in Git
```
#### *💾 Dockerfile :*

Defines how to build the Docker image for the Spring Boot project.

#### *💾 main.yml :*

GitHub Actions configuration file to automate building, testing, and deployment.

#### *💾 src/ :*

Contains the source code for the Spring Boot application.

#### *💾 pom.xml :*

Maven configuration file for dependencies and project build.

#### *💾 dokku-deploy.sh :*

Script that automates the deployment process of the application to a remote server using Dokku.

## 3️⃣🟩 Objectives of the Project

The purpose of this repository is to demonstrate the use of Docker, GitHub Actions, and Dokku in a unified DevOps workflow for building, testing, and deploying applications.

This workflow is highly modular and follows standard practices to ensure scalability and automation:

Containerize the application using Docker.

Implement CI/CD pipelines using GitHub Actions.

Automate deployment processes via Dokku.

## 4️⃣🟨 Technologies Used

#### ▫️ *Backend :*

*💡Java Spring Boot :*

Provides a framework for developing microservices and REST APIs.

#### ▫️ *Containers :*

*💡Docker :*

Used to create, deploy, and run the application in containers, ensuring consistency across environments.

#### ▫️ *Automation :*

*💡GitHub Actions :*

Configured to automate the build, test, and deployment processes.

#### ▫️ *Deployment :*

*💡Dokku :*

Used to manage and deploy the application to a virtualized environment, simplifying application hosting.

#### ▫️ *Others :*

*💡Nginx :*

Used as a reverse proxy for the application to enhance performance and security.

## ⬜️ Code

#### 💡 *Dockerfile*
```
# syntax=docker/dockerfile:1
FROM busybox:latest
COPY --chmod=755 <<EOF /app/run.sh
#!/bin/sh
while true; do
  echo -ne "The time is now $(date +%T)\\r"
  sleep 1
done
EOF

ENTRYPOINT /app/run.sh
```

This Dockerfile creates a lightweight container image using the busybox:latest base image.

*🔷️ Steps :*

▫️ 1. Specify Base Image :

The image is built using the busybox:latest image, which is a minimal Linux distribution optimized for small size and basic functionality.

▫️ 2. Create and Copy a Shell Script :

A shell script is written inline directly into the Dockerfile using a heredoc syntax (<<EOF).

The script (run.sh) is copied into the /app directory inside the container.

The script is given executable permissions (chmod=755).

▫️ 3. Shell Script Functionality :

The script continuously:

Prints the current time (HH:MM:SS) to the terminal.

Overwrites the previous output (\r ensures the same line is updated).

Pauses for 1 second between updates (sleep 1).

▫️ 4. Set the Entrypoint :

The container is configured to execute /app/run.sh automatically when it starts.

### *🔑 Purpose :*

This Dockerfile creates a container that continuously displays the current time in real-time. It demonstrates how to create a minimal, self-contained application using a simple base image like busybox.

#### 💡 *Main.yml*
```
#
name: Create and publish a Docker image

# Configures this workflow to run every time a change is pushed to the branch called `release`.
on:
  push:
    branches: ['main']

# Defines two custom environment variables for the workflow. These are used for the Container registry domain, and a name for the Docker image that this workflow builds.
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

# There is a single job in this workflow. It's configured to run on the latest available version of Ubuntu.
jobs:
  build-and-push-image:
    runs-on: ubuntu-latest
    # Sets the permissions granted to the `GITHUB_TOKEN` for the actions in this job.
    permissions:
      contents: read
      packages: write
      # 
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      # Uses the `docker/login-action` action to log in to the Container registry registry using the account and password that will publish the packages. Once published, the packages are scoped to the account defined here.
      - name: Log in to the Container registry
        uses: docker/login-action@65b78e6e13532edd9afa3aa52ac7964289d1a9c1
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      # This step uses [docker/metadata-action](https://github.com/docker/metadata-action#about) to extract tags and labels that will be applied to the specified image. The `id` "meta" allows the output of this step to be referenced in a subsequent step. The `images` value provides the base name for the tags and labels.
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
      # This step uses the `docker/build-push-action` action to build the image, based on your repository's `Dockerfile`. If the build succeeds, it pushes the image to GitHub Packages.
      # It uses the `context` parameter to define the build's context as the set of files located in the specified path. For more information, see "[Usage](https://github.com/docker/build-push-action#usage)" in the README of the `docker/build-push-action` repository.
      # It uses the `tags` and `labels` parameters to tag and label the image with the output from the "meta" step.
      - name: Build and push Docker image
        uses: docker/build-push-action@f2a1d5e99d037542a71f64918e516c093c6f3fc4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

This main.yml file defines a GitHub Actions workflow that builds and publishes a Docker image to a container registry (GitHub Container Registry).

*Workflow Overview :*

*🔷️ 1. Name :*

The workflow is titled Create and publish a Docker image.

*🔷️ 2. Trigger :*

The workflow runs whenever there is a push event to the main branch.

*🔷️ 3. Environment Variables :*

REGISTRY: Specifies the container registry domain (ghcr.io for GitHub Container Registry).

IMAGE_NAME: Dynamically assigns the Docker image name based on the repository name (${{ github.repository }}).

*🔷️ Job Details:*

Job Name:

build-and-push-image.

Runs On:

Executes on the latest version of Ubuntu (ubuntu-latest).

Permissions:

Grants read access to the repository content and write access to the package registry using the GITHUB_TOKEN.

*🔷️ Steps :*

▫️ 1. Checkout Repository :

Uses the actions/checkout action to fetch the repository files into the workflow environment.

▫️ 2. Log In to the Container Registry :

Uses docker/login-action to authenticate with the container registry (ghcr.io) using:

username: The GitHub actor performing the action.

password: The GITHUB_TOKEN secret, which allows access to the registry.

▫️ 3. Extract Metadata :

Utilizes docker/metadata-action to generate metadata for the Docker image, including tags and labels.

The id: meta allows the output of this step (tags and labels) to be reused in later steps.

The images input defines the base name for the Docker image.

▫️ 4. Build and Push Docker Image :

Uses docker/build-push-action to:

Build the Docker image using the Dockerfile in the repository.

Tag the image with metadata from the meta step.

Push the built image to the GitHub Container Registry (ghcr.io).

### *🔑 Purpose :*

This workflow automates the process of creating and publishing a Docker image whenever changes are pushed to the main branch. The published image is uploaded to GitHub Packages, where it can be accessed by other users or workflows.

## 5️⃣🟦 Project Status

```
☑️ Completed.
```

## 6️⃣👤 Collaboration

This project is for personal use and is not open to external contributions.
However, if you find it interesting or have any questions, I’ll be happy to hear them! Feel free to contact me on my GitHub profile.

## 7️⃣🟪 License

This project does not have an assigned license. Without an explicit license, all rights are reserved. If you wish to use this project, please contact me.

## 8️⃣🟫 Authors

- *Pascua2020* (https://github.com/Pascua2020)

- *UTN*


## 9️⃣📒 Official Documentation :

*- Docker :*
https://docs.docker.com

*- GitHub Actions :*
https://docs.github.com/en/actions

*- Dokku :*
https://dokku.com/docs/getting-started/installation/

## 🔟🔄 Notes

🍪 *Security Considerations*

- This project uses secret variables (DOCKER_USERNAME, DOCKER_PASSWORD, GITHUB_TOKEN).

- Make sure to configure the secrets in the "Settings > Secrets and Variables > Actions" section of your repository.


🍪 *Project Limitations*

- This project does not include advanced orchestration setups like Kubernetes.

- It is designed for simple deployments in Docker-compatible environments.


🍪 *Additional Notes*

- This project is an educational example. 

- It is not recommended for use in production environments without making specific adjustments.
