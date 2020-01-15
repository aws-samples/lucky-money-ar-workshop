---
title: "Prepare 8th Wall Account"
chapter: false
weight: 14
---

## Create 8th wall project

1. Open [8th Wall](https://www.8thwall.com/), register a new account and and log in with it. 

1. After authenticated,  click "Start a new Project" with a name
![](/images/introduction/create-8th-project.png)  

## Upload image target to 8th Wall

8th Wall Web can detect and track 2D image files, allowing you to bring static content to life.

In this workshop, the image target is the product(coke cola) image. It will render 3D content Once it found the target from the camera streaming.

{{%attachments title="Image Targets" pattern=".*(png)"/%}}

1. Download the above images to your local desktop

1. Click the Image Target icon in the left navigation or the **Manage Image Targets** link on the Project Dashboard
![](/image/8th-wall-console-appkey-imagetargets.jpg)

1. Drag your image ( .jpg, .jpeg or .png ) to the **Upload Image** panel, or click **Upload Image** and use your file browser to select your image. 

1. On the Image Target Upload screen, set orientation and use the zoom slider to select region of interest

1. Give your image target a name by editing the field at the top left of the window

1. Click **Upload**

1. Make sure that **Load automatically** is selected
![](/images/introduction/load-automatically.png)

1. Click **Save**
![](/images/introduction/image-targets.png?width=30pc)


## Device Authorization

{{% notice warning %}}
For iPhone user, you may receive a "Device Not Authorized" error message in later steps. We recommend you to **follow [these steps](https://www.8thwall.com/docs/web/#device-not-authorized) from 8th Wall FAQ and avoid this issue.**
{{% /notice %}}

Authorizing a device allows it to view 8th Wall project under development. Authorizing a device installs a Developer Token (cookie) into its web browser, allowing it to view any app key within the current workspace.

{{% notice note %}}
Projects created on the Free plan may not be viewed publicly. Device authorization is required.
{{% /notice  %}}

1. Go back to the Project Dashboard and click **Device Authorization** to expand the device authorization pane.
![](/images/introduction/device-authorization.png)

1. Authorize your device. **Scan** the QRCode with your mobile and **open the website**
![](/image/8th-console-developer-mode-qr.jpg)

1. Click the **Authorize browser** button to authorize your development desktop

## Save 8th Wall APP KEY

1. Click **Settings** on the left navigation bar

1. Copy the **MY APP KEY** and save it for later steps where you will need the key.