---
title: "Setup Cloud9 Environment"
chapter: false
weight: 12
---

To begin, you will need to login to your AWS account and set the Cloud9 environment.

{{% notice note %}}
AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. 
{{% /notice  %}}

## Create IDE environment
1. From the AWS Console, ensure you are using the us-west-2 (Oregon) region, and then select Cloud9 under Services.
![](/image/select_cloud9.png)

2. Create an IDE environment. After that, click Open IDE.

3. Once your Cloud9 environment is loaded, the recommendation is to change the theme to AWS Cloud9 Classic Dark Theme, but you can set your own preferences.
![](/image/cloud9_theme.png)


## Install pre-requisites tools 

In your Cloud9 environment, run the following commands to install the pre-requisites tools.  These commands will take a few minutes to complete. You may receive some warnings and error messages, that is typically ok.
``` 
# Update pip
sudo pip install --upgrade pip
# Update NPM
npm install -g npm
# Update the AWS CLI
pip install --user --upgrade awscli
# Install the AWS Amplify CLI
npm install -g @aws-amplify/cli
# Install create-react-app
npm -g i create-react-app

```

## Setup the credential profile 

Next, you must create the file ~/.aws/config file. This step will allow us to use the AWS default credentials that are included as a profile on the Cloud9 instance. You can do this by running this block of code in your Cloud9 terminal.
```
# Set Region Variable
region=$(curl http://169.254.169.254/latest/dynamic/instance-identity/document|grep region|awk -F\" '{print $4}')
# create default config file
cat <<END > ~/.aws/config
[default]
region=${region}
END

```

