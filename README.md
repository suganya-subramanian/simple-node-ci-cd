# Simple Node.js CI/CD Pipeline

This project demonstrates a **Continuous Integration (CI) and Continuous Deployment (CD)** pipeline for a simple Node.js application using **Docker** and **GitHub Actions**. The pipeline automates the process of building, testing, and deploying a Node.js application in a consistent and efficient manner.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation-setup)
- [CI/CD Workflow](#cicd-workflow)
- [Deployment](#deployment)
- [License](#license)

## Project Overview

This repository contains the code for a basic Node.js application. The primary goal of this project is to set up a fully automated CI/CD pipeline using **GitHub Actions** to streamline the development and deployment process. The workflow includes the following key steps:

1. **Checkout Code**: The workflow automatically checks out the latest version of the code from the GitHub repository.
2. **Docker Image Build**: A Docker image is created for the Node.js application.
3. **Push to Docker Hub**: The built image is pushed to Docker Hub, where it can be deployed to any cloud platform or server.
4. **Deployment (Optional)**: The pipeline optionally supports deploying the application to cloud platforms like **AWS**, **DigitalOcean**, etc.

## Prerequisites

Before setting up this project, make sure you have the following prerequisites:

- **Docker**: Installed on your local machine or CI/CD environment.
- **GitHub Account**: To create the GitHub repository and manage the CI/CD pipeline.
- **Docker Hub Account**: For hosting Docker images.

Additionally, you will need to set up **GitHub Secrets** to securely store your Docker Hub credentials.

### GitHub Secrets

1. **DOCKER_USERNAME**: Your Docker Hub username.
2. **DOCKER_PASSWORD**: Your Docker Hub password or an access token.

Follow these steps to add the secrets to your GitHub repository:

1. Go to your repository on GitHub.
2. Click on **Settings** > **Secrets** > **New repository secret**.
3. Add the secrets with the names `DOCKER_USERNAME` and `DOCKER_PASSWORD`.

## Installation & Setup

Follow the steps below to set up this project on your local machine:

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/suganya-subramanian/simple-node-ci-cd.git
   cd simple-node-ci-cd

    Create a .gitignore file to exclude unnecessary files:

node_modules/
*.log
.dockerignore

Create a Dockerfile for the Node.js application:

# Use official Node.js image as a base
FROM node:14

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json .
RUN npm install

# Copy the rest of the application files
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the Node.js application
CMD ["node", "app.js"]

Push the Project to GitHub.

Make sure to commit and push the changes to your repository:

    git add .
    git commit -m "Add Dockerfile and setup CI/CD pipeline"
    git push origin main

CI/CD Workflow

The CI/CD pipeline is defined using GitHub Actions in the .github/workflows/docker.yml file. The pipeline is triggered on every push to the main branch.
GitHub Actions Workflow

name: Docker Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build the Docker image
        run: |
          docker build -t your-dockerhub-username/simple-node-ci-cd .

      - name: Push the Docker image
        run: |
          docker push your-dockerhub-username/simple-node-ci-cd

      - name: Deploy Application (Optional)
        run: |
          echo "Deployment step (e.g., to AWS, DigitalOcean, etc.)"

This workflow contains the following steps:

    Checkout Code: The action checks out the latest code from the repository.

    Docker Login: It logs in to Docker Hub using the credentials stored in GitHub Secrets.

    Build Docker Image: It builds the Docker image based on the Dockerfile in the repository.

    Push Docker Image: The built image is pushed to Docker Hub for deployment.

    Deployment: (Optional) A step for deploying the application to a cloud platform.
