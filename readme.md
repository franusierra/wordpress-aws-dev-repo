# Development repository

Development repository for the wordpress practice project.

We will use the latest wordpress and mysql versions.

This repository is a "demo" repository for the development team to make changes over the application that will be deployed.

## Requirements

The minimal requirements for this repository are:

* A proper `.gitignore` file
* A proper `.dockerignore` file
* A `Dockerfile` containing the wordpress base
* A `docker-compose` file that will let the developer uses a full functional version of the service
* A `pre-commit` config file that use at least the following hooks:
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
* Avoid the use of cloud environment configuration files such as for `.env` or `wp-config.php`
