---
title: "Add Game Logic"
chapter: false
weight: 50
---

So far, you have created an React Web application and added an AR feature into it. In the following session, you will add some API and business logics.

The following is a proposed database design. We create a **User** table in DynamoDB, this is a reference to the users in Cognito User Pool. We record the 'lucky money' balance in DynamoDB table.
![](/images/addGameLogic/tables.jpg)

In this session, we will:

* Create an AppSync API resource
* Create an AppSycn custom resolver Lambda function
* Add client side API implementation to web application

