---
title: "Add Authentication"
chapter: false
weight: 34
---

## Create Authentication Component

Next, you will add authentication capabilities for our application using Amazon Cognito. 

1. Run the following command to add an authentication component to this Amplify project
```bash
# Add authentication
amplify add auth
```
1. Select `Default configuration` for the first choice. 
![](/images/reactApp/auth_default_configuration.png)
1. Select `Email` when asked **How do you want users to be able to sign in?**. Use the arrow button on your keyboard to move up and down to select choice
![](/images/reactApp/auth_signin.png)
1. For **Do you want to configure advanced settings?**, choose `No, I am done`
1. You can use `amplify status` to show the execution plan
![](/images/reactApp/auth_amplify_status.png)

## Push changes to AWS

The **amplify add auth** command only saved files locally. We need to run the **amplify push** command to deploy our changes to our AWS account.

1. To push our changes to AWS, run the following
```bash
# push changes to AWS
amplify push
```
1. When prompted to complete the changes. Enter `Y` and the deployment will begin
1. You will see the updating progress, wait for this progress finished. This will take a few minutes.
![]()

## Update Authentication to web APP

Replace the **src/App.js** with the following code.
{{< highlight javascript >}}
import React from 'react';
import './App.css';
import AR from './components/AR';
import Ranking from './components/Ranking';
import Sharing from './components/Sharing';

import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import { makeStyles } from '@material-ui/core/styles';
import { AppBar, Toolbar, Typography, IconButton, Button, List, ListItem, 
  ListItemIcon, SwipeableDrawer, Divider} from '@material-ui/core'
import  { Menu as MenuIcon, 
  PermContactCalendar as PermContactCalendarIcon,
  LocalDrink as LocalDrinkIcon,
  ExitToApp as ExitToAppIcon
 } from '@material-ui/icons'
 
 import Amplify from 'aws-amplify'
 import Aws_exports from './aws-exports.js'
 import { withAuthenticator } from 'aws-amplify-react';
 
 Amplify.configure(Aws_exports)

const useStyles = makeStyles(theme => ({
  root: {
    flexGrow: 1,
  },
  menuButton: {
    marginRight: theme.spacing(2),
  },
  title: {
    flexGrow: 1,
  },
}));

function App() {
  const classes = useStyles();
  const [state, setState] = React.useState({
    top: false,
    left: false,
    bottom: false,
    right: false
  });

  const toggleDrawer = (side, open) => event => {
    if (event.type === 'keydown' && (event.key === 'Tab' || event.key === 'Shift')) {
      return;
    }
    setState({ ...state, [side]: open });
  };

  const sideList = side => (
    <div className={classes.list} role="presentation" onClick={toggleDrawer(side, false)} 
    onKeyDown={toggleDrawer(side, false)}>
      <List>
        <ListItem button>
          <ListItemIcon><PermContactCalendarIcon/></ListItemIcon>
          <Link to="/">Ranking</Link>
        </ListItem>
        <ListItem button>
          <ListItemIcon><LocalDrinkIcon /></ListItemIcon>
          <Link to="/sharing">Red Packets</Link>
        </ListItem>
        <Divider />
        <ListItem>
          <ListItemIcon><ExitToAppIcon /></ListItemIcon>
          Sign Out
        </ListItem>
      </List>
    </div>
  );

  const runAR = () => event => {
    window.location.href = "/ar/";
  };


  return (
    <Router>
      <div>
      <AppBar position="static">
          <Toolbar>
            <IconButton edge="start" className={classes.menuButton} color="inherit" aria-label="menu" onClick={toggleDrawer('left', true)}>
              <MenuIcon />
            </IconButton>
            <Typography variant="h6" className={classes.title}>
            Lunar New Year
            </Typography>
            <Button color="inherit" onClick={runAR()}>AR</Button>
          </Toolbar>
        </AppBar>
        <SwipeableDrawer open={state.left} onClose={toggleDrawer('left', false)} onOpen={toggleDrawer('left', true)}>
          {sideList('left')}
        </SwipeableDrawer>
        <Switch>
          <Route exact path="/">
            <Ranking />
          </Route>
          <Route path="/ar">
            <AR />
          </Route>
          <Route path="/sharing">
            <Sharing />
          </Route>
        </Switch>
      </div>
    </Router>
  );
};

export default withAuthenticator(App, { includeGreetings: false });
{{< /highlight >}}

In the above code, we make the following changes:

* Import **aws-amplify** and **aws-exports.js** configuration file
![](/images/reactApp/amplify_imports.png)
* Wrap the web application with build-in **Authenticator** 
![](/images/reactApp/amplify_auth_wrapper.png)

## Test The Application

Now you can return to the preview tab for your application and verify that you can sign up and login to your application.

1. Go to the root URL of your application
1. A default Sign In web page will be displayed
1. Create **Create account**
![](/images/reactApp/auth_create_account.png?width=20pc)
1. Input the details. DO INPUT *Email* in the **username** field.
![](/images/reactApp/auth_signup.png?width=20pc)
1. You will receive an activation code in your email, find the code and input on the confirmation page
![](/images/reactApp/auth_confirm.png?width=20pc)

{{% notice note %}}
We choose to use Email for the username when add authentication feature, so do input your email address in the **userename** field.
{{% /notice %}}

At this point, authentication is in place and working in our React app. Now, on to some more features… adding the Sumerian AR.