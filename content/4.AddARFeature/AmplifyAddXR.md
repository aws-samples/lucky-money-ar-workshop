---
title: "Add AR Feature"
chapter: false
weight: 41
---

## Load 8th Wall

1. Go to Cloud9, open **public/index.html**, copy paste the following code and place in **\<header\>** tag. Replace the **APP_KEY** with your own value. Remember to **save the file** before you move on.
```html
    <script async src="https://apps.8thwall.com/xrweb?appKey=APP_KEY"></script>
```
![](/images/addAR/8th_header.png)

- If you didn't copy the key before and don't know where to find it, here are the steps.
   1. Open your [8th Wall Console](https://www.8thwall.com/), and find your project create in previous steps.
   1. Click the **Setting Button** on the left navigation bar, and find **MY APP KEY**
   ![](/images/addAR/8th_app_key.png)

  
## Export Sumerian configuration file

1. Open [Sumerian Console](https://us-west-2.console.aws.amazon.com/sumerian/home/start), select your created Sumerian Scene
1. Click the **Publish** button on the top right corner
![](/images/addAR/sumerian_publish.png)
1. Select **Host privately**, and then click **Publish**
1. Click the **Download JSON configuration**, and save the file to your local drive. 
![](/images/addAR/sumerian_private_publish.png)


## Add AR Feature to Amplify Project

1. Rename the Sumerian JSON configuration file from **sumerian_exports_xxxxx.json** to `sumerian_exports.json`
1. Upload the Sumerian configuration file to Cloud9, put it in **project root directory** (under `ako2020-lucky-money`). You can drag and upload the file. 
1. Run the following command to add AR feature
```bash
# change  to the project's root directory
cd ~/environment/ako2020-lucky-money
amplify add xr
```
1. Press **Enter** when ready
1. For **Provide a name for the scene**, input `LuckyMoneyAR`
1. For **Enter the path to the downloaded JSON configuration fil**, input `./sumerian_exports.json`
1. Select **N** when asked whether allow unauthenticated users access to the Sumerian project
![](/images/addAR/amplify_add_xr.png)
1. Push changes to AWS
```bash
amplify push
```
1. Enter **Y** to continue, and wait for completion
![](/images/addAR/amplify_xr_push.png)

## Add AR Feature to web APP

Open **src/components/AR.js**, and replace with the following code
{{% notice warning %}}
**[IMPORTANT]**  If you paste the following code with the vim editor, be sure to run ``:set paste`` before you paste, otherwise a large part of the code will be commented automatically in a wrong way. 
{{% /notice %}}

{{< highlight javascript >}}
import React from 'react'
import { withAuthenticator } from 'aws-amplify-react'
import {XR as awsXR, Auth} from 'aws-amplify'
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
        <div id="snackbar">{this.state.toastText}</div>
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

    var showToast = function(){
      let script = document.createElement('script')
      script.setAttribute("id","snackbar-script")
      if (document.getElementById('snackbar-script') != null) document.getElementById('snackbar-script').remove();
      script.text = 'var x = document.getElementById("snackbar");x.className = "show";setTimeout(function() {x.className = x.className.replace("show", "");}, 3000);'
      document.getElementById('snackbar').appendChild(script)
    }
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


{{% notice note %}}
You will need a mobile device to test the AR feature.
{{% /notice %}}

So far, you have added AR feature to the web application. In the next session, we will deploy this web application via AWS Amplify Console.
