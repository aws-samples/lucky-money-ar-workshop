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
touch src/components/Toast.css
# create assets folder
mkdir public/images
# download the lucky money image
wget {{< param codeRepo >}}raw/master/public/images/red_envolope.jpg -O public/images/red_envolope.jpg
wget {{< param codeRepo >}}raw/master/public/images/redpacket.png -O public/images/redpacket.png
{{< /highlight >}}

## Update React Component 

1. Edit the **src/component/Toast.css** and replace the contents with the following code, save the file
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

1. Edit the **src/components/AR.js** file and replace the contents with the following code, save the file. 

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

1. Edit the **src/components/Ranking.js** file and replace the contents with the following code, save the file 

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
  const [luckyMoneyValue, setLuckyMoneyValue] = React.useState('$ 1.2');
  const [luckyMoneyText, setLuckyMoneyText] = React.useState('Lucky Money Shared from q@amazon.com')
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


1. Update **src/components/Sharing.js**

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
  const [luckyMoneyValue, setLuckyMoneyValue] = React.useState('');
  const [luckyMoneyText, setLuckyMoneyText] = React.useState('')
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
      setLuckyMoneyValue('$ ' + detail.money/100)
      setLuckyMoneyText("Lucky Money (Hongbao)\n Shared from "+ luckyMoney.owner)
      // set popup modal open
      setOpen(true)
    } catch (err) {
      // Already opened
      setLuckyMoneyText("You have already opened it")
      setLuckyMoneyValue("")
      setOpen(true)
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
          {/* Lucky Money from  */}
          {luckyMoneyText}
        </DialogContentText>
        {/* <DialogContentText style={{color:'#efbc3c'}} className={classes.modalText}>
        {luckyMoney.owner}
        </DialogContentText> */}
        <DialogContentText style={{color:'#efbc3c',fontSize: '3rem'}} className={classes.modalText}>
          {/* Random Balance write in here */}
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

1. Update **src/App.js**

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


1. Update **src/index.js**

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

1. Now, we have a React web application with a couple of pages. Return to the preview window for your application and you should see the skeleton of Lucky Money web app.
![](/images/reactApp/application_stub.png)

You can now continue adding Authentication feature in next step using AWS Amplify.