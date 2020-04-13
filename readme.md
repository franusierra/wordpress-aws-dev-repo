# Development repository

Development repository for the wordpress practice project.

We will use the latest wordpress and mysql versions.

This repository is a "demo" repository for the development team to make changes over the application that will be deployed.

## Requirements

The minimal requirements for this repository are:

* A proper `.gitignore` file.
* A proper `.dockerignore` file.
* A `Dockerfile` containing the wordpress base.
* A `docker-compose` file that will let the developer uses a full functional version of the service.
* A `Jenkinsfile` file with a _declarative pipeline_ that will build the docker image from the dockerfile and push to ECR. Once the image is pushed to ECR a new pipeline will be triggered from the first one to deploy to **staging** environment for the develop branch and deploy to **production** for the master branch. Consider the automated tag of the image before push to ECR using, for example, the commit hash ID. Use a **multibranch** pipeline.
* A `pre-commit` config file that use _at least_ the following hooks:
  * check-added-large-files
  * check-case-conflict
  * check-executables-have-shebangs
  * check-json
  * check-merge-conflict
  * trailing-whitespace
  * remove-tabs
  * markdownlint
  * shellcheck
  * yamllint
* Avoid the use of cloud environment configuration files such as for `.env` or `wp-config.php`. Only local development ones are allowed.
* Avoid the use of cloud environment credentials. Use EC2 instance profile for jenkins to gain the necessary permissions.
* Use `github workflow` to validate pre-commit hooks in the remote repository on pull request and push events.
* Use webhooks to trigger jenkins pipeline.
