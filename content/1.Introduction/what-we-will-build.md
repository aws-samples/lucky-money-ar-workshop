---
title: "What we will build"
chapter: false
weight: 12
---

## Architecture
The following shows the architect of our application. The frontend is static React web application, and the server side is built with a couple of AWS services.

![](/images/introduction/arch.png)

* Amazon Cognito: Authentication
* AWS AppSync: GraphQL APIs     
* AWS Amplify: React Components, SDK, Libraries and AWS CLI toolchain     
* Amazon DynamoDB: Data Storage     
* Amazon Sumerian: create AR/VR experience on web  
* Amazon Lambda: AppSync custom resolver. Read and write data to DynamoDB and exports to AppSync

## User Interactions

The users will need to login into the application. After logging into this application, users will see a **Ranking list**. In this application developing process, the list is empty or only has one item for the user itself. 
In the competition session, all other users will be shown after you change config file and use the same backend.

You will click the top left **Menu** button to navigate between different pages, or click the top right **AR** button to enter AR camera mode.
![](/images/introduction/screenshot-1.jpg)

Once you entered the **AR** mode, find a real product and scan with the camera. 
An AR Lucky Money Red Packet will be displayed on the screen. Click the AR Lucky Money to get the money. 
In real world application, you will see certain sponsor's product ads here, for simplicity, we omit it in this workshop.
 
If you choose to **share** it with your friends, you will get some extra bonus. After you click the share button or the close button, you will be **routed back** to the Ranking List.

![](/images/introduction/screenshot-2.jpg)

Click the Menu button and switch to the **Sharing Page**. It displays all the remaining lucky money shared by others. Click and open them to try to gain more. 

![](/images/introduction/screenshot-3.jpg)
   
  






