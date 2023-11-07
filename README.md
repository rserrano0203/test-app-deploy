# [test-app-deploy] Deploy in GitHub

## Deployed @ https://rserrano0203.github.io/test-app-deploy/

## Getting started

# Github Deployment

## Static HTML deployment in GitHub

Deploying a static HTML website to GitHub Pages is a straightforward process. GitHub Pages allows you to host static websites directly from your GitHub repository. Here's a step-by-step guide on how to deploy your static HTML website to GitHub:

### Step 1: Create a GitHub Repository

If you don't already have a GitHub account, sign up for one. Once you have an account, follow these steps:

1. Log in to GitHub.
2. Click on the '+' sign in the top right corner and select "New repository."
3. Choose a repository name (e.g., my-website).
4. Make the repository public or private, depending on your preference.
5. Initialize the repository with a README file (optional, but recommended).
6. Click the "Create repository" button.

### Step 2: Upload Your Static HTML Files

You can either create a new directory and upload your HTML files directly to the repository, or you can clone the repository to your local machine and add the files using Git. Here's how to do it using Git:

1. Open your terminal.
2. Clone the GitHub repository to your local machine:

```bash
git clone https://github.com/yourusername/my-website.git
```

Replace yourusername with your GitHub username and my-website with your repository name.

3. Navigate to the cloned directory:

```bash
cd my-website
```

4. Copy or move your HTML files, CSS, JavaScript, images, and other assets into this directory.

### Step 3: Commit and Push Your Changes

In the same terminal window, commit your changes and push them to your GitHub repository:

```bash
git add .
git commit -m "Initial commit"
git push origin master

```

This pushes your static website files to the GitHub repository.

### Step 4: Configure GitHub Pages

Now that your HTML files are on GitHub, you can set up GitHub Pages to host your static website:

1. Go to your GitHub repository on the GitHub website.
2. Click on the "Settings" tab.
3. Scroll down to the "GitHub Pages" section.
4. In the "Source" drop-down menu, select the branch where your HTML files are located (usually "main" or "master").
5. Click the "Save" button.

GitHub Pages will automatically build your website from the selected branch and provide a URL where your website is hosted. This URL will be in the format https://yourusername.github.io/my-website, where yourusername is your GitHub username, and my-website is your repository name.

### Step 5: Access Your Website

Your static HTML website is now live on GitHub Pages. You can access it by visiting the URL provided in the "GitHub Pages" section of your repository settings.

---

---

## Node.JS deployment in GitHub

Deploying a Node.js application to GitHub typically involves setting up a continuous integration/continuous deployment (CI/CD) pipeline and deploying your application to a server or a hosting platform. Here's a step-by-step guide on how to deploy a Node.js application to GitHub using GitHub Actions for CI/CD:

### Prerequisites:

1. A GitHub repository for your Node.js application.
2. A Node.js application with its dependencies managed via npm or yarn.
3. A server or hosting environment where you want to deploy your Node.js application.

### Step 1: Set Up Your Node.js Project

Ensure your Node.js project is ready for deployment. Make sure all the required dependencies are defined in the package.json file.

### Step 2: Create a GitHub Repository

If you haven't already, create a GitHub repository for your Node.js application. Initialize a local Git repository and link it to your GitHub repository using the following commands:

```bash
git init
git remote add origin <repository-url>
```

Replace `<repository-url>` with the actual URL of your GitHub repository.

### Step 3: Create GitHub Actions Workflow

GitHub Actions is used for setting up a CI/CD pipeline. Create a YAML file in the .github/workflows directory of your project's repository to define your workflow. Here's an example of a basic GitHub Actions workflow for a Node.js application:

```yaml
name: Node.js CI/CD

on:
  push:
    branches:
      - main # Change this to the branch you want to deploy from

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Install Node.js and dependencies
        run: |
          npm install
          # Or use 'yarn' if you prefer
          # yarn install

      - name: Build and test
        run: |
          # Replace this with your build and test commands
          npm run build
          # Or use 'yarn' if you prefer
          # yarn build

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to server
        run: |
          # Replace this with your deployment commands
          # Example: scp or rsync to your server
```

- Make sure to adjust the on.push.branches section to specify the branch from which you want to deploy.

### Step 4: Configure Server and Deployment

You'll need to set up your server or hosting environment for deployment. This typically involves:

- Installing Node.js and npm on your server.
- Configuring a reverse proxy (e.g., Nginx or Apache) if your application is using a web server.
- Setting up environment variables for your application.
  Preparing your server for hosting Node.js applications.

You may also want to use deployment tools or scripts (e.g., SSH, rsync) to automate the deployment process.

### Step 5: Configure Deployment Scripts

In the `deploy` stage of your GitHub Actions workflow, replace the placeholder comment with the actual deployment commands that will copy your application to the server and perform other deployment-related tasks.

Here's an example of using SCP for deployment:

```yaml
deploy:
  needs: build
  runs-on: ubuntu-latest

  steps:
    - name: Deploy to server
      run: |
        scp -r ./ your-server:/path/to/deploy
```

Replace your-server and /path/to/deploy with your actual server details.

### Step 6: Push to GitHub

Push your changes to the GitHub repository. This will trigger the GitHub Actions workflow, which will build and deploy your Node.js application according to the defined pipeline.

### Step 7: Monitor and Troubleshoot

Monitor the GitHub Actions workflow for any errors or issues. If there are problems, review the workflow logs for details on what went wrong and adjust your configuration accordingly.

## Laravel deployment in GitHub

### Prerequisites:

1. A GitHub account and a repository for your Laravel application.
2. A Laravel application with its dependencies managed via Composer.
3. A server or hosting environment where you want to deploy your Laravel application.

### Step 1: Set Up Your Laravel Project

Make sure your Laravel project is ready for deployment. Ensure that all your dependencies are defined in the composer.json file.

### Step 2: Configure GitHub Repository

Create a GitHub repository for your Laravel application if you haven't already. Initialize a local Git repository and link it to your GitHub repository using the following commands:

```bash
git init
git remote add origin <repository-url>
```

Replace `<repository-url>` with the actual URL of your GitHub repository.

### Step 3: Set Up Continuous Integration/Continuous Deployment (CI/CD)

You can use GitHub Actions to set up your CI/CD pipeline. Create a `.github/` workflows directory in your Laravel project, and then create a YAML file for your CI/CD workflow. Here's a basic example named `laravel-ci-cd.yml`:

```yaml
name: Laravel CI/CD

on:
  push:
    branches:
      - main  # Change this to the branch you want to deploy

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.0'

      - name: Install Composer dependencies
        run: composer install

      - name: Generate Laravel application key
        run: php artisan key:generate

      - name: Cache Laravel config and routes
        run: php artisan config:cache
        run: php artisan route:cache

  deploy:
    runs-on: ubuntu-latest

    needs: build

    steps:
      - name: Deploy Laravel application
        run: |
          ssh user@your-server "cd /path/to/your/app && git pull origin main"
          ssh user@your-server "cd /path/to/your/app && composer install"
          ssh user@your-server "cd /path/to/your/app && php artisan migrate"

```

In the deploy section, replace user, your-server, and /path/to/your/app with your actual server and project details.

### Step 4: Configure Server and Deployment

Set up your server or hosting environment for deployment, as mentioned in the prerequisites section. This typically involves:

- Installing a web server like Nginx or Apache.
- Configuring your web server to point to your Laravel project's public directory.
- Installing PHP and any necessary extensions.
- Setting up a database if your application requires one.
- Configuring environment variables in the .env file for your server.

Additionally, you might want to use deployment tools like Deployer, Envoy, or simply a script to automate deployment tasks on your server.

### Step 5: Push and Trigger CI/CD

Push your changes to the GitHub repository's main branch. This will trigger the GitHub Actions CI/CD workflow.

### Step 6: Monitor and Troubleshoot

Monitor the workflow for any errors or issues. If there are problems, review the workflow logs for details on what went wrong and adjust your configuration accordingly.

### Ref: https://pages.github.com/

### GitLab Deployment Link: https://gitlab.com/rserrano0203/test-app-deploy/-/tree/main?ref_type=heads

```json
{
  "createdBy": {
    "author": "RSerrano",
    "date": "11/02/2023",
    "version": "1.0"
  }
}
```
