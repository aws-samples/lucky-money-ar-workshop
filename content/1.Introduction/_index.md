---
title: "Introduction"
chapter: false
weight: 10
---


{{% notice note %}}
If you have already understood the use scenario well in the speaking session, you may optionally skip this descriptive introduction and go the next page to start directly.
Otherwise, it's highly recommended for you to do the overall understanding and thinking about this before you start.
{{% /notice  %}}

## Use Scenario 

This workshop will deliver an application for end users to do AR scanning to earn red packets and share red packets with each other, whose profit model is mobile online advertising. 

There are two types of roles in this scenario, advertisers and users.

* The advertiser (such as coca-cola drink, Mcdonald's, etc) would invest a certain amount of money in Red Packet inventory with the purpose of promoting their products. 

* Users in this application could scan some physical symbols (or logos) to get a red packet, 
which contains certain digital lucky money (2 USD for example) inside with the investor's logo and name to suggest the sponsor.

To get the money, the user may either click the red packet directly to get a randomly allocated amount of money 
or click and share the red packet with friends to earn additional bonus (as sharing makes more people be able to see the 
advertisement, which benefits the advertiser).  

The total open times and total money inside one shred red packet is fixed. 
In our application design, it's 2 USD with maximum 5 open chances per shared ped packet, which means that only
5 friends who is the fasted to click the shared red packet would split the 2 USD with each one getting a random number based purely on luck. 
The friend are not able to re-share the packet again.

In real world, users could withdraw the money to his or her own bank accounts to get the real money after gaining the digital money from the red packets.
In our workshop, so sorry but this step is skipped : )  However, we will still build a dashboard ranking who is the luckiest guy holding most money in hand in real time for the competition.


## Business Requirements

* User registration function for this application
* Uses login in to begin AR scanning  
* Uses open red packet after matching scanning to get a random number of money
* Users be able to choose to share the red packet with friends 
* Friends split the shared red packet with each one getting a random number yet total amount being 2 USD.
* Calculations for random number for each packet and the total number per user gets.
* Real time dashboard for players total amount ranking


## Technical Requirements
* Develop a Web application using modern toolsets
* Simple, re-usable APIs
* AR integration with the application
* High speed and Low latency database









