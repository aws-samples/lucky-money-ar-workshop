---
title: "Init Amplify Project"
chapter: false
weight: 33
---

## Initialize Amplify Project

In this step, we will begin adding AWS Components using the Amplify framework. To start, let’s initialize amplify in our project directory.

{{% notice note %}}
Amplify is an opinionated framework. To make relevant and automated decisions, the Amplify CLI will ask you a series of questions.
{{% /notice %}}

1. Run the following command to initilize an empty Amplify project
```bash
# Change to the React project directory
cd ~/environment/ako2020-lucky-money
# Initialize the project configuration for Amplify
amplify init
```
1. For the name of the project, you can use the default name. For the **environment name**, enter `dev`
1. For the **default editor**, you can choose `None`, accept all the default values. Amplify CLI attempts to identify the application and programming frameworks that you are using. Typcially this means that the correct values are already selected. 
![](/images/reactApp/amplify_init.png)
1. When asked choose the **profile** you want to use, select `default`
![](/images/reactApp/amplify_profile.png)


{{% notice note %}}
If you make a mistake during the Q/A portion, you can use CTRL-C to safely start over.
{{% /notice %}}
