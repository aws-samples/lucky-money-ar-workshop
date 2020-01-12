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

The users will need to login into the application. After login, they be to navigated to the **Ranking List**. The ranking list will be empty or contains only you during the workshop. But will have others, once change the same backend in the final competition session. You will click the top left **Menu** button to navigate between different pages, or click the top right **AR** button to enter AR camera mode.

![](/images/introduction/screenshot-1.jpg)

Once you entered the **AR** mode, find a real product and **scan** with the camera. An AR Lucky Money will be displayed. The AR Lucky Money contains some money, so you need to **click** and open it. In real world application, you will see some product ads here, but we omit it in this workshop. If you choose to **share** with your friends, you will get some bonus. After you click the share button or the close button, you will be **routed back** to the Ranking List.

![](/images/introduction/screenshot-2.jpg)

Click the **Menu** button and switch to the **Sharing Page**. There will be some lucky money with remaining value inside. **Click** and open the lucky mone to gain more. 

![](/images/introduction/screenshot-3.jpg)
   
  






