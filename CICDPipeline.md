# Guide: Deploying to VPS Using GitHub Actions

### .github/workflows/deploy.yml

This guide outlines the process of deploying a Node.js application to a DigitalOcean VPS using GitHub Actions. Follow the steps below to configure the CI/CD pipeline and deploy your application.

## 1. Prerequisites

### On the VPS
- Ensure you have Node.js and npm installed.
- Install and configure [PM2](https://pm2.keymetrics.io/) to manage your application process.
- Set up your project directory on the VPS (e.g., `/var/www/your-project-name`).
- Add your SSH public key to the `~/.ssh/authorized_keys` file for the `root` user account used to SSH into the VPS.

### On GitHub
- Create a repository for your project.
- Add the following secrets to your repository under **Settings > Secrets and variables > Actions**:
  - **HOST**: The IP address of your VPS.
  - **USER**: The username (e.g., `root`) used for SSH access.
  - **SSH_PRIVATE_KEY**: The private SSH key corresponding to the public key added to the VPS.

### Add the Public Key to `authorized_keys`
Append the public key to the `authorized_keys` file on the VPS:

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

### View the Private Key and Add to Your GitHub Secret

Use this command to display the private key (needed for GitHub secrets):
```bash
cat ~/.ssh/id_rsa
```
### Set Proper Permissions
Ensure the `~/.ssh` directory and its files have the correct permissions to prevent access issues:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### Install Required Dependencies
Ensure the VPS has the following installed:

```bash
sudo apt update
sudo apt install git
npm install -g pm2
```

## 2. GitHub Actions Workflow

### Create a `.github/workflows/deploy.yml` file in your repository with the following content:
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

### Now copy and paste it on `deploy.yml`
```bash
HOST = VPS IP
```
```bash
USER = root
```
```bash
SSH_PRIVATE_KEY = -----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEAqcyb0ioAT1wAHo1wzdqd+3eAr7pGspx+AlBX4l4eMKPbjQMMTyfY
-----END OPENSSH PRIVATE KEY-----
```


```bash
name: Deploy to VPS

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build

  deploy:
    runs-on: ubuntu-latest

    needs: build

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup SSH and Deploy
        uses: appleboy/ssh-action@v1.2.0
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          debug: true
          script: |
            #!/bin/bash
            set -e

            # Load NVM (if needed for future compatibility)
            export NVM_DIR="$HOME/.nvm"
            [ -s "$NVM_DIR/nvm.sh" ] && source "$NVM_DIR/nvm.sh"

            # Navigate to project directory
            cd /var/www/[project-name]

            # Stop the application using PM2
            pm2 stop [project-name]

            # Pull the latest changes
            git pull origin main

            # Install dependencies
            npm install

            # Build the application
            npm run build

            # Start or restart the application using PM2
            pm2 restart [project-name]
```
