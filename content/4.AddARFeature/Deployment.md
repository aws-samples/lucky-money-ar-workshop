---
title: "Publish the web APP"
chapter: false
weight: 42
---

## Create CodeCommit Repositories

1. Run the following command in Cloud9 to create CodeCommit repositories
```bash
aws codecommit create-repository --repository-name ako2020-lucky-money
```

1. Run the following commands to configure the AWS CLI credential helper for HTTPS connections:
```bash
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

## Push changes to CodeCommit

1. Commit the changes 
```bash
# Commit added files
git add -f src/aws-exports.js
git add .gitignore package-lock.json package.json public/index.html src/App.js src/index.js amplify/ public/images/ src/components/
git commit -m "initial commit"
```

1. Add CodeCommit remote origin, and push to CodeCommit
```bash
# Add remote origin
git remote add origin https://git-codecommit.us-west-2.amazonaws.com/v1/repos/ako2020-lucky-money
# Push to origin master branch
git push --set-upstream origin master
```
![](/images/addAR/git_push.png)

## Configure AWS Amplify Console

We have create the AWS backend using Amplify CLI. Now we only use the AWS Amplify Console to deploy the frontend environment.

1. Go to [Amplify Console](https://us-west-2.console.aws.amazon.com/amplify/home?region=us-west-2#/), select the Amplify project, it should be named **ako2020-lucky-money**
1. Select **AWS CodeCommit** as the source code Git provider, and click **Connect branch**
![](/images/addAR/amplify_codecommit.png)
1. Select **ako2020-lucky-money** from the repositories, and **master** branch, click **Next** to continue
![](/images/addAR/amplify_codecommit_select.png)
1. **Unselect** the checkbox before **Deploy updates to backend resources with your frontend on every code commit**
![](/images/addAR/amplify_ignore_backend.png)
1. Click **Next** button, and click **Save and deploy** button to start deployment
1. After the deployment started, click the **Rewrites and redirects** on the left bar, Click **Edit**
1. Edit the existing rule or create a new one if there is no default
  * **Source address**: `</^[^.]+$|\.(?!(css|gif|ico|jpg|js|png|txt|svg|woff|ttf|map|json)$)([^.]+$)/>`
  * **Target address**: `/index.html`
  * **Type**: `200(Rewrite)`
![](/images/addAR/amplify_rewrites.png)
1. If the deployment fails, follow the below **Troubleshooting** instructions to fix.


## Troubleshooting
You may encounter the following building error. Follow the error message and click the **Re-authenticate app** button will **NOT** fix this issue.
![](/images/addAR/amplify_build_error.png)

Follow the steps to fix this issue:

1. Go to [AWS IAM Console](https://console.aws.amazon.com/iam/home?region=us-west-2#)
1. Select the **Roles** on the left navigation bar
1. Click **Create Role**
1. Select **Amplify**, and click **Next:Permissions** to continue, accepts the defaults
1. On the Review page, input a name for this role and click **Create role**
1. Go back to [AWS Amplify Console](https://us-west-2.console.aws.amazon.com/amplify/home?region=us-west-2#/), and select the Amplify project
1. Select **General** on the left navigation bar, and click the **Edit** button
![](/images/addAR/amplify_update_edit.png)
1. Update the **Service role**, and click **Save** button
1. On the left navigation bar, select the Amplify project, click **master** to enter the connected branch.
1. Click the **Redeploy this version** button
![](/images/addAR/amplify_redeploy.png)
1. After the deployment, You can find the amplify domain link under **Domain**.
![](/images/addAR/amplify_link.png)



