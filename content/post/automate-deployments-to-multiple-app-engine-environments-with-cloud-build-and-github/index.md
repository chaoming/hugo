+++
author = "Chaoming Li"
title = "Automate Deployments to Multiple App Engine Environments with Cloud Build and GitHub"
description = "If you want to securely automate deployments of your App Engine application to multiple environments such as staging and production with a simple Git commit or a pull request, this article will outline the steps for you."
date = "2019-07-16"
categories = [
    "DevOp"
]
tags = [
    "github",
    "cloud build",
    "deploy",
    "golang",
    "appengine",
    "google cloud platform"
]
image = "1_UzYiagjfQwOZTlIwmCqKOQ.png"
+++

If you want to securely automate deployments of your App Engine application to multiple environments such as staging and production with a simple Git commit or a pull request, this article will outline the steps for you.

<!--more-->

## Why You Should Do This:

Good reasons to automate the deployment process are:

- Reduce the impact of human error on a deployment
- Improve security by not including any credentials or sensitive configuration settings in the source code
- Restrict permission to deploy to certain environments (e.g. production) with a process
- Streamline deployment process and gain the benefits automation (e.g. testing)
