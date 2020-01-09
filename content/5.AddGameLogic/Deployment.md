---
title: "Redeployment"
chapter: false
weight: 54
---

## Commit code

Run the following code to push changes to CodeCommit.
```bash
git add amplify/backend/backend-config.json public/images/red_envolope.jpg \
src/App.js src/aws-exports.js src/components/AR.js src/components/Ranking.js \
src/components/Sharing.js src/index.js .graphqlconfig.yml amplify/backend/api/ \
amplify/backend/function/ src/graphql/
git commit -m "add game logic"
git push
```

Since we have connect the AWS Amplify and CodeCommit branch, once you push changes to CodeCommit, AWS Amplify will start to build automatically. Visit [AWS Amplify Console](https://us-west-2.console.aws.amazon.com/amplify/home) to verify the CI/CD works.

