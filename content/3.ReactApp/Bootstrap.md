---
title: "Bootstrap React APP"
chapter: false
weight: 31
---

We will now create the React Application. This will include the following components:

* src/index.js - The entry point of React application
* src/App.js - the single page application

## Create the React APP

Run the following command to create the React applicaiton:

{{% notice warning %}}
The command to revert to react-scripts version 3.2.0 is due to a security error that occurs when running in debug mode with Cloud9 on version 3.3.0. If you are not using Cloud9, you may be able to use 3.3.0 but it is not required for this workshop.
{{% /notice %}}

```
# Create the initial app
npx create-react-app ako2020-lucky-money
# Revert to react-scripts version 3.2.0 due to security error with Cloud9 and 3.3.0
cd ako2020-lucky-money
npm i react-scripts@3.2.0
```

Run the following commands to install the dependencies for our project.
```
# Install Amplify
npm i aws-amplify
# Install Amplify React
npm i aws-amplify-react
# Install Material UI
npm i @material-ui/core
npm i @material-ui/icons
npm i typeface-roboto
# Install React Router Dom
npm i react-router-dom
```

## Test the web application
To ensure the newly created application works in our environment, run the following:
```
# Run npm start
npm start
```

After this, verify that the development server started successfully with this message:
![npm verify](/images/reactApp/npm_verify.png)

In the Cloud9 environment, choose **Preview Running Application** from the **Preview** menu:
![preview app](/images/reactApp/preview_running_app.png)

This will load a browser window within Cloud9. Ensure the default application is working correctly:
![view app](/images/reactApp/view_running_app.png)

Going forward, you will probably want to launch the preview window in a separate browser window. Click the button in the top right to do this:
![open in new tab](/images/reactApp/open_in_new_tab.png)

{{% notice note %}}
You can keep this terminal open and open another one in a new tab. Youd don't have to quit this terminal and run `npm start` again.
{{% /notice %}}
![](/images/reactApp/new_terminal.png)

