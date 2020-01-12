---
title: "Redeployment"
chapter: false
weight: 54
---

## Commit code

Run the following code to push changes to CodeCommit.
```bash
cd ~/environment/ako2020-lucky-money/
# Git Add
git add amplify/backend/backend-config.json \
public/images/red_envolope.jpg \
src/App.js src/aws-exports.js \
src/components/AR.js \
src/components/Ranking.js \
src/components/Sharing.js \
src/index.js \
.graphqlconfig.yml \
amplify/backend/api/ \
amplify/backend/function/ \
src/graphql/
# Git Commit
git commit -m "add game logic"
git push
```

Since we have connect the AWS Amplify and CodeCommit branch, once you push changes to CodeCommit, AWS Amplify will start to build automatically. Visit [AWS Amplify Console](https://us-west-2.console.aws.amazon.com/amplify/home) to verify the CI/CD works.

## Test the application
1. Click the **AR** button to enter AR mode
1. Scan the product / product image
![](/images/cola.png?width=30pc)
1. Open the AR Red Packet, you will get some 'lucky money'
1. Share Or Close the Red Packet. If you share, you will get some bonus
1. Click the **Menu** button and select **Red Packets**
1. Open red packets shared from others.