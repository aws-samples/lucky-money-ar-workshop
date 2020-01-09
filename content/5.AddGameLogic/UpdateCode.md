---
title: "Update Application Code"
chapter: false
weight: 53
---

update **src/App.js**
{{< highlight javascript >}}
import React from 'react';
import './App.css';
import { withAuthenticator } from 'aws-amplify-react';
import {Auth} from 'aws-amplify'
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

import Amplify from 'aws-amplify';
import Aws_exports from './aws-exports';

Amplify.configure(Aws_exports);

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

const signout = () => {
  Auth.signOut()
}

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
          <Link to="/" onClick={signout}>Sign out</Link>
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

export default withAuthenticator(withRouter(App), { includeGreetings: false });
{{< /highlight >}}

update `src/component/Ranking.js`
{{< highlight javascript >}}
import React from 'react'
import { withAuthenticator } from 'aws-amplify-react';
import { List, ListItem, ListItemText } from '@material-ui/core';
import * as queries from '../graphql/queries';
import {API, graphqlOperation, Auth} from 'aws-amplify';

class Ranking extends React.Component {
  constructor() {
    super()
    this.state = {
      balance: 0,
      users: []
    }
  }

  componentDidMount() {
    this.init().then()
  }

  async init() {
    const currentUser = await Auth.currentUserInfo()
    const userRes = await API.graphql(graphqlOperation(queries.getUser, {UserEmail: currentUser.attributes.email}))
    if (userRes.data.getUser) {
      const myBalance = userRes.data.getUser.Balance
      this.setState({balance: myBalance/100})
    }

    const listUsersRes = await API.graphql(graphqlOperation(queries.usersByBalance, {Group: "AKO2020", sortDirection: "DESC", limit: 10}))
    const topUsers = listUsersRes.data.usersByBalance.items
    if (topUsers) {
      this.setState({users: topUsers})
    }
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

export default withAuthenticator(Ranking);
{{< /highlight >}}

Update `src/components/AR.js`
{{< highlight javascript >}}
import React from 'react'
import { withAuthenticator } from 'aws-amplify-react'
import {XR as awsXR, Auth} from 'aws-amplify'
import { AppBar, Toolbar, Typography, IconButton } from '@material-ui/core'
import { ArrowBack } from '@material-ui/icons'
import * as mutations from '../graphql/mutations';
import {API, graphqlOperation} from 'aws-amplify';

class AR extends React.Component {
  constructor() {
    super()
    this.state = {
      user: {}
    }
  }

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
            <p id="loading-status">Loading...</p>
          </div>
      </div>
    );
  }

  moveToMain() {
    window.location.href = "/";
  };

  componentDidMount() {
    const self = this
    this.loadAndStartScene();
    
    Auth.currentUserInfo().then(user => {
      this.setState({user: user})
    })
    
    var receiveMessage = function(event)
    {
      switch (event.data){
        case "sumerian-open-packet":
          API.graphql(graphqlOperation(mutations.openPrivateRedPacket, 
            {
              UserEmail: self.state.user.attributes.email,
              ProductType: "1"
            })).then(luckyMoney => {
              console.log(luckyMoney)
              // TODO: Add a notice message to info user how much they earned.
            }).catch(err => {
              console.error(err)
              // TODO: add an error message
            })
          break;
        case "sumerian-close-packet":
          window.location.href = "/"; // 
          break;
        case "sumerian-share-packet":
          API.graphql(graphqlOperation(mutations.shareRedPacket, {
            UserEmail: self.state.user.attributes.email, 
            ProductType: "1"
          })).then(luckyMoney => {
            console.log("shared a lucky money")
            // TODO: Add a notice message to info user how much they earned for extra
            window.location.href = "/";
          }).catch(err => {
            // TODO: Add an error message to show
            console.error(err)
          })
          break;
        default:
          console.log(event.data)
      }
    }
    window.addEventListener("message", receiveMessage, false);
  }

  async loadAndStartScene() {
    await awsXR.loadScene('LuckyMoneyAR', 'sumerian-scene-dom-id');

    const world = awsXR.getSceneController('LuckyMoneyAR').sumerianRunner.world;

    window.sumerian.SystemBus.addListener('xrerror', (params) => {
      // Add error handling here
    });

    window.sumerian.SystemBus.addListener('xrready', () => {
      // Both the Sumerian scene and XR8 camera have loaded. Dismiss loading status
      const loadingStatus = window.document.getElementById('loading-status');
      if (loadingStatus && loadingStatus.parentNode) {
        loadingStatus.parentNode.removeChild(loadingStatus);
      }

      window.document.getElementById('sumerian').style.position = "inherit";
    });

    window.XR8.Sumerian.addXRWebSystem(world);

    window.sumerian.SystemBus.addListener('doshare', () => {
      // Add error handling here
      console.log ('DoShare is clicked');
    });

    window.sumerian.SystemBus.addListener('doclose', () => {
      // Add error handling here
      console.log ('DoClose is clicked');
    });

    awsXR.start('LuckyMoneyAR');
  }
};

export default withAuthenticator(AR);

{{< /highlight >}}

update `src/components/Sharing.js`
{{< highlight javascript >}}
import React from 'react'
import { makeStyles } from '@material-ui/core/styles';
import { Card, CardActionArea, CardMedia, CardContent, Typography, Grid} from '@material-ui/core'
import Dialog from '@material-ui/core/Dialog';
import DialogActions from '@material-ui/core/DialogActions';
import DialogContent from '@material-ui/core/DialogContent';
import DialogContentText from '@material-ui/core/DialogContentText';
import { withAuthenticator } from 'aws-amplify-react';

import * as queries from '../graphql/queries';
import * as mutations from '../graphql/mutations';
import {API, graphqlOperation, Auth} from 'aws-amplify';

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
  const [luckyMoneyValue, setLuckyMoneyValue] = React.useState(false);
  const luckyMoney = props
  
  const handleClose = () => {
    setOpen(false);
  };
  
  const openLuckyMoney = async () => {
    // try catch here
    const currentUser = await Auth.currentUserInfo()
    try {
      const shardRedPacketRes = await API.graphql(graphqlOperation(mutations.openSharedRedPacket, {
        ProductType: props.adsId, 
        UserEmail: props.owner, 
        FriendUserEmail: currentUser.attributes.email
      }))
      console.log(shardRedPacketRes.data.openSharedRedPacket)
      const details = JSON.parse(shardRedPacketRes.data.openSharedRedPacket.RPShareDetails)
      console.log(details)
      const detail = details.redPackets.find(detail => detail.friend === currentUser.attributes.email)
      // set the display value
      setLuckyMoneyValue(detail.money)
      // set popup modal open
      setOpen(true)
    } catch (err) {
      console.error(err)
    }    
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
          Lucky Money from 
          
        </DialogContentText>
        <DialogContentText style={{color:'#efbc3c'}} className={classes.modalText}>
        {luckyMoney.owner}
        </DialogContentText>
        <DialogContentText style={{color:'#efbc3c',fontSize: '3rem'}} className={classes.modalText}>
          {/* Random Balance write in here */}
          $ {luckyMoneyValue/100}
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
    this.init().then()
  }

  async init() {
    const luckyMoneysRes =  await API.graphql(graphqlOperation(queries.redPacketsByProductType, 
      { 
        ProductType: "1", 
        filter: { 
          SharedDoneFlag: { eq: false }
        }
      }))

    const luckyMoneys = luckyMoneysRes.data.redPacketsByProductType.items
    if (luckyMoneys) {
      this.setState({luckyMoneys: luckyMoneys})
    }
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

export default withAuthenticator(Sharing)
{{< /highlight >}}

update `src/index.js`
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

