---
title: "Prepare 8th Wall Account"
chapter: false
weight: 14
---

## Create 8th wall project

1. Open [8th Wall](https://www.8thwall.com/), and login with your own account

1. After authenticated,  click "Start a new Project"     
![](/image/8th-wall-console-workspace-start-project.jpg)

2. Select the workspace for this project     

3. Enter Basic info for the project: Please provide a Title, URL, Description (optional) and Cover Image (optional). All of these fields, except URL, can be edited later in the Project Settings page.      

4. Select a Project Type. In this workshop, we will use **Non-Commercial** License.
![](/image/8th-wall-gettingstarted-new-project-info.jpg)

## Upload image target to 8th Wall

The image target is a product image. Once it found the target from the camera streaming, it will render 3D content.

{{%attachments title="Image Targets" pattern=".*(png)"/%}}

1. Download the above images to your local desktop

1. Click the Image Target icon in the left navigation or the "Manage Image Targets" link on the Project Dashboard
![](/image/8th-wall-console-appkey-imagetargets.jpg)

1. Drag your image (.jpg, .jpeg or .png) to the "Upload Image" panel, or click "Upload Image" and use your file browser to select your image. Make sure you **meet all the requirements for the target image** which are listed below

1. On the Image Target Upload screen, set orientation and use the zoom slider to select region of interest

1. Give your image target a name by editing the field at the top left of the window

1. Click **Upload**

1. Select **Load automatically**

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
![](/images/introduction/8th-wall-device-authorization.png)

1. Authorize your device. **Scan** the QRCode with your mobile and **open the website**
![](/image/8th-console-developer-mode-qr.jpg)

1. Click the **Authorize browser** button to authorize your development desktop

## Save 8th Wall APP KEY

1. Click **Settings** on the left navigation bar

1. Copy the **MY APP KEY** and paste to somewhere, you will need this key in later steps.