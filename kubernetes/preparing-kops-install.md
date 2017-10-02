## Preparing local management machine

Download the Kops command line utility from github: https://github.com/kubernetes/kops/releases
  - I've used this one: **[kops 1.7.0](https://github.com/kubernetes/kops/releases/download/1.7.0/kops-linux-amd64)**

Change the permissions to make it executable:

- ```sudo chmod +x kops-linux-amd64```

Move it into your PATH:

- ```sudo mv kops-linux-amd64 /usr/local/bin/kops```
  - **we are renaming the binary to kops for ease of use, you don't want to be typing kops-linux-amd64 every time!!**

Install Python Pip in preperation for installing the AWS CLI:

- ```yum install python-pip``` **or** ```apt-get install python-pip```

Install the AWS command line utility:

- ```sudo pip install awscli```


## Preparing AWS:

- Log into your AWS account

- Go to Sevices > Security, Identity & Compliance > IAM

- Go to Users

- Add user
  - User name: kops
  - Access type*: Programmatic Access


- Next > add the user to the administrator group
- Next > Hit Create user button

- Go into the user and ensure it has **AdministratorAccess** permissions and that it is in the **administrator group**

- Select the Security credentials tab and **create access key**
  - Take a note of the **Access key ID** + most importantly the **Secret access key** as this is a one time thing, you won't be able to see it again, (or download the csv)


- From your local management machine, login to AWS using the above generated Access key ID and Secret access key
  - From the command line run: ```aws configure```
    - Feel free to leave the other options defaulted to None (or set them if you wish)

#### Now setup S3 storage for kops state file storage:

On AWS, go to Serverices > Storage > S3

- Create bucket

- Give it a bucket name with a random number on the end as this needs to be unique
  - example: kops-state-b33445566

- Select the region you prefer

**Note:**

This is where all of your configuration files for kubernetes will be stored.

## DNS

We need a subdomain - purchase one (you can do all of this through route53 on AWS)

- Services > Networking & Content Delivery > Route53

- Create a hosted zone

Kops will manage the DNS for us using Route53


#### Download and install kubectl

```wget https://storage.googleapis.com/kubernetes-release/release/v1.6.1/bin/linux/amd64/kubectl```

```chmod +x kubectl```
```sudo mv kubectl /usr/local/bin```


Now create ssh keys if you don't already have keys you'd wish to use:

```ssh-keygen -t rsa```

- Accept the defaults

This creates
  - ```~/.ssh/id_rsa```
  - ```~/.id_rsa.pub```

Kops uses these by default
