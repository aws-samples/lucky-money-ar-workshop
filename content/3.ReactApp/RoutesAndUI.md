---
title: "React Routes & Material UI"
chapter: false
weight: 32
---

In this step, we will create the stub of the application. This will include the previously mentioned components, but without any live data at this point. The following is the navigation diagram. 
![](/images/reactApp/react_routes.jpg)

The following components will be created:

* src/components/AR.js - displays the AR scene created in Amazon Sumerian  
* src/components/Sharing.js - displays the lucky money shared from the user's friends
* src/components/Ranking.js - displays user's lucky money balance and a ranking list, the friends with the highest balance are dislayed at the top

## Create React Components and download assets

Run the following commands to create some files, and download necessary assets:
{{< highlight shell >}}
cd ~/environment/ako2020-lucky-money
# create components folder
mkdir src/components/
# create components files
touch src/components/AR.js
touch src/components/Sharing.js
touch src/components/Ranking.js
# create assets folder
mkdir public/images
# download the lucky money image
wget {{< param codeRepo >}}/blob/master/static/images/reactApp/red_envolope.jpg -O public/images/red_envolope.jpg
{{< /highlight >}}

## Update React Component 

1. Edit the **src/components/Ranking.js** file and replace the contents with the following code, save the file 

{{< highlight javascript >}}
import React from 'react'
import { makeStyles } from '@material-ui/core/styles';
import { List, ListItem, ListItemText } from '@material-ui/core';

const useStyles = makeStyles(theme => ({
  root: {
    width: '100%',
    maxWidth: 360,
    backgroundColor: theme.palette.background.paper,
  },
}));

function Ranking() {
  const classes = useStyles();

  const myBalance = 19.2

  const users = [
    {
      username: 'qiaoshi@amazon.com',
      balance: 20
    },
    {
      username: 'sss@amazon.com',
      balance: 12.3
    },
    {
      username: 'nice@amazon.com',
      balance: 4
    }
  ]

  return (
    <div>
      <h2>Your Balance: { myBalance }</h2>
      <List dense className={classes.root}>
        {users.map(user => {
          const labelId = `checkbox-list-secondary-label-${user.username}`;
          return (
            <ListItem key={user.username} button>
              <ListItemText id={labelId} primary={`${user.username}`} />
              <ListItemText edge="end" primary={`$ ${user.balance}`} />
            </ListItem>
          );
        })}
      </List>
    </div>
  )
}

export default Ranking;
{{< /highlight >}}

1. Edit the **src/components/AR.js** file and replace the contents with the following code, save the file. 

{{< highlight javascript >}}
import React from 'react'
import { AppBar, Toolbar, Typography, IconButton } from '@material-ui/core'
import { ArrowBack } from '@material-ui/icons'

class AR extends React.Component {
  render() {
    return (
      <div>
        <AppBar position="static">
          <Toolbar>
            <IconButton edge="start" color="inherit" aria-label="menu" onClick={this.moveToMain.bind(this)}>
              <ArrowBack />
            </IconButton>
            <Typography variant="h6" >
            Find Lucky Money
            </Typography>
          </Toolbar>
        </AppBar>
        <div id="sumerian-scene-dom-id" style={ {height: '100vh'} }>
            <p id="loading-status">AR.....</p>
          </div>
      </div>
    );
  }
};

export default AR;
{{< /highlight >}}

1. Update **src/components/Sharing.js**

{{< highlight javascript >}}
import React from 'react'
import { makeStyles } from '@material-ui/core/styles';
import { Card, CardActionArea, CardMedia, CardContent, Typography, Grid} from '@material-ui/core'

const useStyles = makeStyles({
  card: {
    maxWidth: 345,
    marginTop: 10
  },
  media: {
    height: 140,
  },
});

function RedPacketCard(props) {
  const luckyMoney = props
  const classes = useStyles()

  return (
    <Card className={classes.card}>
      <CardActionArea>
        <CardMedia
          className={classes.media}
          image="/images/red_envolope.jpg"
          title="Contemplative Reptile"
        />
        <CardContent>
          <Typography gutterBottom variant="h5" component="h2">
            Lucky Money (Hongbao)
          </Typography>
          <Typography variant="body2" color="textSecondary" component="p">
            Shared from {luckyMoney.owner}
          </Typography>
        </CardContent>
      </CardActionArea>
    </Card>
  )
}

function Sharing() {
  return (
    <Grid container justify={"center"}>
      <RedPacketCard owner="nice@amazon.com" adsId="111"/>
    </Grid>
  )
}

export default Sharing
{{< /highlight >}}

1. Update **src/App.js**

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

export default App
{{< /highlight >}}


1. Update **src/index.js**

{{< highlight javascript >}}
import React from 'react';
import ReactDOM from 'react-dom';
import 'typeface-roboto';
import './index.css';
import { Route, BrowserRouter as Router } from 'react-router-dom';
import App from './App';
import Ranking from './components/Ranking';
import AR from './components/AR';
import Sharing from './components/Sharing';
import * as serviceWorker from './serviceWorker';

const routing = (
  <Router>
    <div>
      <Route exact path='/' component={App} />
      <Route path='/ar/' component={AR} />
      <Route path='/ranking' component={Ranking} />
      <Route path='/sharing' component={Sharing} />
    </div>
  </Router>
)

ReactDOM.render(routing, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
{{< /highlight >}}

1. Now, we have a React web application with a couple of pages. Return to the preview window for your application and you should see the skeleton of Lucky Money web app.
![](/images/reactApp/application_stub.png)

You can now continue adding Authentication feature in next step using AWS Amplify.