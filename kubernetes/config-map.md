# Config Map

Config parameters that are not secrets can be put in a **configMap**.

The input is key-value pairs

Can be used by the app using:
  - environment variables
  - container commandline arguments in the pod Configuration
  - using volumes

A **configMap** can also contain full configuration Files, for example a web server config file.

This file can then be mounted using volumes where the application expects its config Files, which allows you to inject configuration settings into containers without changing the container itself

An example of a **configMap** file is as follows:

```
driver=jdbc
database=mysql
lookandfeel=1
paramx=10
paramy=20
user=test
IP=192.168.0.1
```
