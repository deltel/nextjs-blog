---
title: "Configure AWS nginx request size limit - Elastic Beanstalk"
date: "2025-03-26"
---

By default the request size limit on the nginx server is **1MB**.
If we wish to increase this limit we have to let AWS know.
We do this by adding a config file in a particular folder structure to the source bundle.

Source bundle here refers to everything needed for the deployment to be done on AWS.
In our case, this is really just the jar file and the folder structure containing the config file we want AWS to apply to the nginx server all zipped together.

The config file needs to be placed in this folder structure: **.platform/nginx/conf.d/**
AWS will then automatically pick up the config file and update the nginx server's configuration.
In our case, the filename is client_max_body_size.conf, so the final path to the file that is added to the source bundle is **.platform/nginx/conf.d/client_max_body_size.conf**

So the contents of our source bundle is:

1. .platform/nginx/conf.d/client_max_body_size.conf
2. application.jar

where **application.jar** is the compiled jar file for the source code.

~/workspace/deploy

- .platform
  - nginx
    - conf.d
      - client_max_body_size.conf
- application.jar

The contents of **client_max_body_size.conf** is a single line: `client_max_body_size 5M;`. The 5M represents 5 MB. And finally we zip the entire folder to create the source bundle and we name it **deploy.zip**.
