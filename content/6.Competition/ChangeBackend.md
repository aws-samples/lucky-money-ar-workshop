---
title: "Change Backend & Redeploy"
chapter: false
weight: 61
---

To play together, everyone needs to join the same backend.

In this step, we will revise the configuration file ```aws-exports.js``` to change the backend information from your personal account to the workshop account.


## Change the Configuration File

1. Find the ```aws-exports.js``` file in your project under xxx path.
1. Revise the file to the following information
```
AWS_ACCOUNT_ID = XXXXXX 
```

## Redeploy
1. Run ```amplify push``` to finish the redeployment. 

Now you are ready to compete, read the next page for usage guide.

+++++++++ Joe adds steps to this part +++++++++