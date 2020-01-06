---
title: "Prepare 8th Wall WebAR"
chapter: false
weight: 13
---


## Create 8th wall project

1. From the Homepage (logged in) or Workspace Dashboard, click "Start a new Project"     
![](/image/8th-wall-console-workspace-start-project.jpg)

2. Select the workspace for this project.     

3. Enter Basic info for the project: Please provide a Title, URL, Description (optional) and Cover Image (optional). All of these fields, except URL, can be edited later in the Project Settings page.      

4. Select a Project Type. In this workshop, we will use **Non-Commercial** for non-commercial demos.
![](/image/8th-wall-gettingstarted-new-project-info.jpg)

## Upload image target to 8th Wall

1. Click the Image Target icon in the left navigation or the "Manage Image Targets" link on the Project Dashboard.
![](/image/8th-wall-console-appkey-imagetargets.jpg)

2. Drag your image (.jpg, .jpeg or .png) to the "Upload Image" panel, or click "Upload Image" and use your file browser to select your image. Make sure you **meet all the requirements for the target image** which are listed below.

3. On the Image Target Upload screen, set orientation and use the zoom slider to select region of interest.

4. Give your image target a name by editing the field at the top left of the window.

5. Click Upload.

6. Test your image target: On the “Image Target Editor” screen, scan the QR code to open our “Target Tester” web app, then scan your image.

7. Optional: Add metadata to your image, in either Text or JSON format.

8. Click Load automatically if you want the image target to be enabled automatically as the WebAR project loads. Up to 5 image targets can be loaded automatically without writing a single line of code. More targets can be loaded programnatically through the Javascript API.

9. Click Save

#### Image Target Requirements

* **File Types** :  .jpg, .jpeg or .png
* **Aspect Ratio** :  3:4
* **Dimensions** :
   * Minimum: 480 x 640 pixels
   * Maximum length or width: 2048 pixels.

> Note: If you upload something larger, the image is resized down to a max length/width of 2048, maintaining aspect ratio.)

## Device Authorization

Authorizing a device allows it to view 8th Wall project under development. Authorizing a device installs a Developer Token (cookie) into its web browser, allowing it to view any app key within the current workspace.

{{% notice note %}}
Projects created on the Free plan may not be viewed publicly. Device authorization is required.
Upgrade to an Agency or Business plan, you gain the ability to host your project publicly on internet (and view them without device authorization).
There is no limit to the number of devices that can be authorized, but each device needs to be authorized individually. Views of your web application from an authorized device count toward your monthly usage total.
{{% /notice  %}}

1. Go back to the Project Dashboard and click Device Authorization to expand the device authorization pane.

2. Select 8th Wall Engine version to use during development. To use the latest stable version of 8th Wall, select release. To test against a pre-release version, select beta.
![](/image/8th-console-developer-mode-channel.jpg)

3. Authorize your device.  You could either authorize your device from desktop or from mobile.

#### From Desktop 

If you are logged into the console on your laptop/desktop, Scan the QR code from the device you wish to authorize. This installs an authorization cookie on the device.

Note: A QR code can only be scanned once. After scanning, you will receive confirmation that your device has been authorized. The console will then generate a new QR code that can be scanned to authorize another device.

* Before: 
   ![](/image/8th-console-developer-mode-qr.jpg)

* After: 
   ![](/image/8th-console-developer-mode-qr-result-desktop.jpg)

#### From Mobile 

If you are logged into 8thwall.com directly on the mobile device you wish to authorize, simply click Authorize browser. Doing so installs an authorization cookie into your mobile browser, authorizing it to view any project within the current workspace.

* Before:
   ![](/image/8th-console-developer-mode-mobile.jpg)

* After：
   ![](/image/8th-console-developer-mode-mobile-authorized.jpg)