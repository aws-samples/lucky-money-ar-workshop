---
title: "Add API"
chapter: false
weight: 52
---

## Adding APIs and data

Using Amplify, we are going to deploy an [AWS AppSync API](https://aws.amazon.com/appsync/).


1. Run the following command to add an API:
```
amplify add api
```

1. Choose **GraphQL**, and enter the API name as `LuckyMoneyAPI`
![](/images/addGameLogic/amplify-add-api.png)

1. Choose **Amazon Cognito User Pool** for authorization type

1. Choose **No, I am done** when prompted for additional changes

1. Choose **N** for annotated GraphQL Schema and **N** for guided schema creation. Leave the custom type name as **MyType** and press enter. This will create a base schema, but we will replace that with our own.
![](/images/addGameLogic/amplify-add-api-2.png)

1. Notice the location of the schema file that Amplify created. This is the schema definition that amplify will use to create the AppSync schema. To edit this, you can click the file location and it will open a context menu where you can select **Open**
![](/images/addGameLogic/amplify-add-api-3.png)

1. Replace the schema in **schema.graphql** with the following code and **save** schema.graphql
{{< highlight graphql >}}
type Advertisement @model(subscriptions: null) @key(fields: ["ProductType"]){
  ProductType: String!
  ADContent: String
  ImageUrl: String
  ProductDescription: String
  RPMaxSharedNum: Int
  RPMoneyInside: Float
  RPMoneyToShare: Float
  RPShareBonus: Float
}

type SharedRedPacket @model(mutations: null) @key(fields: ["UserEmail", "ProductType"]) @key(name: "ByProductType", fields: ["ProductType"], queryField: "redPacketsByProductType") {
  UserEmail: String!
  ProductType: String!
  RPShareDetails: String
  SharedDoneFlag: Boolean
  UpdateTime: String
}

type User @model(mutations: null) @key(fields: ["UserEmail"]) @key(name: "ByGroupBalance", fields: ["Group", "Balance"], queryField: "usersByBalance"){
  UserEmail: String!
  Balance: Float
  HasSharedRP: Boolean
  Group: String!
}

type Mutation {
  openPrivateRedPacket(ProductType: String!, UserEmail: String!): User! @function(name: "LuckyMoneyFunction-${env}")
  openSharedRedPacket(FriendUserEmail: String, ProductType: String!, UserEmail: String!): SharedRedPacket! @function(name: "LuckyMoneyFunction-${env}")
  shareRedPacket(ProductType: String!, UserEmail: String!): SharedRedPacket! @function(name: "LuckyMoneyFunction-${env}")
}

input CreateUserInput {
  UserEmail: String!
  Group: String! = "AKO2020"
  Balance: Int = 0
}
{{< /highlight >}}

1. Make sure you save the schema.graphql file and then run `amplify push` to push our changes into AWS. Leave the default responses for the next set of questions.

Once you get through all the questions, deployment will start. This will take a few minutes. When complete, you will receive the GraphQL endpoint information in the terminal window. This endpoint is now accessible by an authenticated user.
![](/images/addGameLogic/appsync_endpoint.png)

{{% notice note %}}
Notice the @ signs in this schema? The Amplify framework supports these GraphQL annotations - which are directives that tell amplify to configure our AppSync API in a certain way:</br>
@model - creates the data model using DynamoDB</br>
@connection - creates relationship between types</br>
@auth - creates fields identifying access control and the appropriate query controls</br>
To learn more about these, check out https://aws-amplify.github.io/docs/cli-toolchain/graphql#directives
{{% /notice %}}

## Add DynamoDB data

1. Open the file **src/aws-exports.js** and find the **aws_user_pools_web_client_id** value. Copy the value as you will need it for the next step:
![](/images/addGameLogic/copy_web_client_id.png)

2. visit the [AWS AppSync console](https://console.aws.amazon.com/appsync/home), Click on the name of your API.

3. Select **Queries** from the left column, and then click **Login with User Pools**, and then enter the following values, and click **Login**
  * ClientId: enter the value you copied for **aws_user_pools_web_client_id**
  * Username: Your username, for this workshop, it should be same as your email address
  * Password

1. We need to add some product data so we don’t have an empty **Advertisement** table. To do this, paste in the following set of queries and then press the **Orange Play** button.
{{< highlight graphql >}}
mutation {
  # Create the Advertisement
  createAdvertisement(input:{
    ProductType: "1",
    ADContent: "None"
    ImageUrl: "None"
    ProductDescription: "None",
    RPMoneyInside: 200
    RPMoneyToShare: 200
    RPShareBonus: 100
    RPMaxSharedNum: 5
  }) {
    ProductType
    ADContent
    ImageUrl
    ProductDescription
  }
}
{{< /highlight >}}

![](/images/addGameLogic/insert-ads.png)

This will load our initial advertisement data - you should see successful logs in the right hand column of the AppSync query console.

## Update Lambda Environment 

The lambda function will need to access our datasource, we set through Lambda Environment. Update using the following code, and replace **${env}**, **${region}**, **${user_pool_id}** and **${appsync_id}** with your own value.

{{< highlight bash >}}
aws lambda update-function-configuration \
--function-name LuckyMoneyFunction-${env} \
--environment Variables="{env=${env}, REGION=${region}, COGNITO_USER_POOL_ID=${user_pool_id}, APPSYNC_ID=${appsync_id}}"
{{< /highlight >}}

* **${env}**: Amplify environment, use `amplify status` to check. If you followed this workshop, it should be `dev`
* **${region}**: AWS region. If you followed this workshop, it should be `us-west-2`
* **${user_pool_id}**: Cognito User Pool Id. Find it in **src/aws-exports.js**
![](/images/addGameLogic/user_pool_id.png?width=30pc)
* **${appsync_id}**: AppSync ID. Find it in **amplify/amplify-meta.json**
![](/images/addGameLogic/amplify-meta.png?width=30pc)