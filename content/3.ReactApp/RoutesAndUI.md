---
title: "React Routes & Material UI"
chapter: false
weight: 32
---

In this step, we will create the stub of the application. This will include the previously mentioned components, but without any live data at this point. The following is the navigation diagram. 
![](/images/reactApp/react_routes.jpg)

The following components will be created:

* **src/components/AR.js** - displays the AR scene created in Amazon Sumerian  
* **src/components/Sharing.js** - displays the lucky money shared from the user's friends
* **src/components/Ranking.js** - displays user's lucky money balance and a ranking list, the friends with the highest balance are dislayed at the top

## Create React Components and download assets

Run the following commands to create some files, and download necessary assets:
{{< highlight shell >}}
cd ~/environment/lucky-money-ar-workshop
# create components folder
mkdir src/components/
# create components files
touch src/components/AR.js
touch src/components/Sharing.js
touch src/components/Ranking.js
touch src/components/Toast.css
# create assets folder
mkdir public/images
# download the lucky money image
wget https://github.com/{{< param codeRepoName >}}/raw/master/public/images/red_envolope.jpg -O public/images/red_envolope.jpg
wget https://{{< param codeRepoName >}}/raw/master/public/images/redpacket.png -O public/images/redpacket.png

{{< /highlight >}}

## Update React Component 

1. Run ``cd src/components``. There are four files in this folder. We will update them one by one.
1. Edit the **Toast.css**. Replace the contents with the following code, save the file
{{< highlight css >}}
#snackbar {
    visibility: hidden;
    min-width: 250px;
    background-color: #333;
    color: #fff;
    text-align: center;
    border-radius: 2px;
    padding: 16px;
    position: fixed;
    z-index: 1;
    left: 0;
    right: 0;
    margin-left: 10%;
    margin-right: 10%;
    bottom: 20%;
    font-size: 15px;
}

#snackbar.show {
    visibility: visible;
    -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
    animation: fadein 0.5s, fadeout 0.5s 2.5s;
}

@-webkit-keyframes fadein {
    from {
        bottom: 0;
        opacity: 0;
    }
    to {
        bottom: 30px;
        opacity: 1;
    }
}

@keyframes fadein {
    from {
        bottom: 0;
        opacity: 0;
    }
    to {
        bottom: 30px;
        opacity: 1;
    }
}

@-webkit-keyframes fadeout {
    from {
        bottom: 30px;
        opacity: 1;
    }
    to {
        bottom: 0;
        opacity: 0;
    }
}

@keyframes fadeout {
    from {
        bottom: 30px;
        opacity: 1;
    }
    to {
        bottom: 0;
        opacity: 0;
    }
}
{{< /highlight >}}
1. Edit the **AR.js** file and replace the contents with the following code, save the file. 
{{< highlight javascript >}}
import React from 'react'
import { AppBar, Toolbar, Typography, IconButton } from '@material-ui/core'
import { ArrowBack } from '@material-ui/icons'
import './Toast.css'

class AR extends React.Component {
  constructor() {
    super()
    this.state = {
      user: {},
      toastText: "aaaaaa"
    }
  }
  
  moveToMain() {
    window.location.href = "/";
  };

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
            <p id="loading-status">AR...</p>
          </div>
        <div id="snackbar">{this.state.toastText}</div>
      </div>
    );
  }

};

export default AR;
{{< /highlight >}}
1. Edit the **Ranking.js** file and replace the contents with the following code, save the file
{{< highlight javascript >}}
import React from 'react'
import { List, ListItem, ListItemText } from '@material-ui/core';

class Ranking extends React.Component {
  constructor() {
    super()
    this.state = {
      balance: 0,
      users: []
    }
  }

  componentDidMount() {
    const topUsers = [
      {
        UserEmail: 'a@amazon.com',
        Balance: 1090
      },
      {
        UserEmail: 'b@amazon.com',
        Balance: 890
      },
      {
        UserEmail: 'c@amazon.com',
        Balance: 450
      }
    ]
    this.setState({users: topUsers, balance: 4.50})
  }
  render() {
    return (
      <div>
      <h2>Your Balance: { this.state.balance }</h2>
      <List dense>
        {this.state.users.map(user => {
          const labelId = `checkbox-list-secondary-label-${user.UserEmail}`;
          return (
            <ListItem key={user.UserEmail} button>
              <ListItemText id={labelId} primary={`${user.UserEmail}`} />
              <ListItemText edge="end" primary={`$ ${user.Balance/100}`} />
            </ListItem>
          );
        })}
      </List>
    </div>
    )
  }
}

export default Ranking
{{< /highlight >}}
1. Replace **Sharing.js** with the following code.
{{< highlight javascript >}}
import React from 'react'
import { makeStyles } from '@material-ui/core/styles';
import { Card, CardActionArea, CardMedia, CardContent, Typography, Grid} from '@material-ui/core'
import Dialog from '@material-ui/core/Dialog';
import DialogActions from '@material-ui/core/DialogActions';
import DialogContent from '@material-ui/core/DialogContent';
import DialogContentText from '@material-ui/core/DialogContentText';

const useStyles = makeStyles({
  card: {
    maxWidth: 345,
    marginTop: 10
  },
  media: {
    height: 140,
  },
  dialog: {
    backgroundImage:`url(${'/images/redpacket.png'})`,
    height: 320,
    width: 247,
    borderRadius: '21px'
  },
  modalText:{
    textAlign: "center"
  }
});

function RedPacketCard(props) {
  const classes = useStyles()
  const [open, setOpen] = React.useState(false);
  const [luckyMoneyValue] = React.useState('$ 1.2');
  const [luckyMoneyText] = React.useState('Lucky Money Shared from q@amazon.com')
  const luckyMoney = props
  
  const handleClose = () => {
    setOpen(false);
  };
  
  const openLuckyMoney = async () => {
    setOpen(true)
  }

  return (
    <div>
    <Card className={classes.card}>
      <CardActionArea onClick={() => openLuckyMoney()}>
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
    <Dialog
      open={open}
      onClose={handleClose}
      aria-labelledby="responsive-dialog-title"
      classes={{paper: classes.dialog}}
    >
      <DialogContent>

        <DialogContentText style={{color:'#efbc3c'}} className={classes.modalText}>
          {luckyMoneyText}
        </DialogContentText>
        <DialogContentText style={{color:'#efbc3c',fontSize: '3rem'}} className={classes.modalText}>
          {luckyMoneyValue}
        </DialogContentText>
      </DialogContent>
      <DialogActions onClick={handleClose} style={{height: '120px'}}>
      </DialogActions>
    </Dialog>
    </div>
    
  )
}

class Sharing extends React.Component {

  constructor() {
    super()
    this.state = {
      luckyMoneys: []
    }
  }

  componentDidMount() {
    const luckyMoneys = [
        {
            UserEmail: "a@amazon.com",
            ProductType: "1"
        },
        {
            UserEmail: "b@amazon.com",
            ProductType: "1"
        }
    ]
    this.setState({
        luckyMoneys: luckyMoneys
    })
  }

  render() {
    return (
      <Grid container justify={"center"}>
        {this.state.luckyMoneys.map(luckyMoney => {
          const keyId = `shared-lucky-money-id-${luckyMoney.UserEmail}`;
          return (
            <RedPacketCard key={keyId} owner={luckyMoney.UserEmail} adsId={luckyMoney.ProductType} />
          )
        })}
      </Grid>
    )
  }
}

export default Sharing
{{< /highlight >}}
1. Run ``cd ..`` to go the **src** folder. Edit **App.js** and replace it with the following code
{{< highlight javascript >}}
import React from 'react';
import './App.css';
import AR from './components/AR';
import Ranking from './components/Ranking';
import Sharing from './components/Sharing';

import { Route, Switch, withRouter, Link } from "react-router-dom";
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

const exclusionArray = [
  '/ar',
  '/ar/',
]

const App=({location}) => {
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
          <Link to="/">Sign out</Link>
        </ListItem>
      </List>
    </div>
  );

  const runAR = () => event => {
    window.location.href = "/ar/";
  };

  const Header = () => {
    return (
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
      </div>
    )
  }

  return (
      <div>
        {exclusionArray.indexOf(location.pathname) < 0 && <Header/>}
        <Switch>
          
          <Route exact path="/">
            <Ranking />
          </Route>
          <Route path="/ranking">
            <Ranking />
          </Route>
          <Route path="/sharing">
            <Sharing />
          </Route>
          <Route path="/ar">
            <AR />
          </Route>
          
        </Switch>
      </div>

  )
};

export default withRouter(App);
{{< /highlight >}}
1. Edit **index.js** under **src** and replace with the following code.    
{{% notice warning %}}
**[Important]** If you use VIM to paste, be sure to run ``:set paste`` before you paste the following code otherwise part of the codes will be commented automatically.
{{% /notice %}}
{{< highlight javascript >}}
import React from 'react';
import ReactDOM from 'react-dom';
import 'typeface-roboto';
import './index.css';
import { BrowserRouter as Router } from 'react-router-dom';
import App from './App';

import * as serviceWorker from './serviceWorker';

const routing = (
  <Router>
    <App />
  </Router>
)

ReactDOM.render(routing, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
{{< /highlight >}}

Now, we have a React web application with a couple of pages. Return to the preview window for your application and you should see the skeleton of Lucky Money web app.
![](/images/reactApp/application_stub.png)

You can now continue adding Authentication feature in next step using AWS Amplify.
