
---
title: "Add Action Behaviors"
chapter: false
weight: 22
---


## Add Assets Action Behaviors 

####  Red Packet 
1. Click the "box" entity and click **Add Component**, choose **State Machine**.    
   ![](/image/WechatIMG7.png)   
   
   ![](/image/WechatIMG8.png)   

1. Click the **+** button to add behavior.     
   ![](/image/WechatIMG11.png)

1. Name the behavior name and the state name **wait for click**.

1. Click **Add Action**    
   ![](/image/WechatIMG13.png)

1. Find **Click/Tap on entity** in the list or search it in the search bar. Don't forget to **Add** it.   
   ![](/image/WechatIMG14.png)

1. Click **Add State** and name it **hide**   
   ![](/image/WechatIMG15.png)
   
1. Click **Add Action**. Find and add **Hide**   
   ![](/image/WechatIMG19.png)

1. Add another action. Click **Add Action** and add **Emit Message**   
   ![](/image/WechatIMG21.png)

1. In the channel, enter **showMoney**      
   ![](/image/WechatIMG23.png)

1. Add another action **Execute Script**. Click the **+** button to add script, choose **Custom (Legacy Format)**. 
    ![](/image/script-create-script.png)

    ![](/image/script-choose-type.png)

1. Click the **pencil** button to edit script, copy the following codes into the function `enter`, then save the change.
    
    {{< highlight javascript >}}
        window.postMessage('sumerian-open-packet','*');
        ctx.transitions.success();
    {{< /highlight >}}

    ![](/image/script-edit-script.png)

    ![](/image/script-save-change.png)

1. Click the **wait for click** state, drag a line to **hide** state, and drag a line back to **wait for click** state.   
   ![](/image/script-drag-line.png)

#### Red Packet with Money 

1. Click the **box with money** entity.   

1. Click **Add Component** button, choose **State Machine**    

1. Click the **+** button    

1. Name both the behavior name and the state name **listen**    

1. Click **Add Action**    

1. Find **Listen** and click **Add**    
   ![](/image/WechatIMG24.png)

1. In the channel, enter **showMoney**
    
1. Click **Add State** and name it **show**    

1. Click **Add action** and add **Show**    

1. Click the **listen** state and drag a line to **show** state   

#### Money 
1. Repeat the **Red Packet with Money Behavior** 3 times for the 3 money entities.

#### Share Button

1. Click the **Share Btn** entity, create **State Machine**.

1. Click **+** button to add a new behavior, the repeat the steps **Red Packet with Money Behavior**.
    ![](/image/share-button-first-behavior.png)

1. Create another behavior, name both the behavior name and the state name **ListenForClickShare**.
    ![](/image/share-button-behaviors.png)

1. Add Action **Click/Tap on entity**.

1. Create another state, name it **ExeShareScript**.

1. Add another action **Execute Script**. Click the **+** button to add script, choose **Custom (Legacy Format)**. 

1. Click the **pencil** button to edit script, copy the following codes into the function `enter`, then save the change.

    {{< highlight javascript >}}
	    window.postMessage('sumerian-share-packet','*');
	    ctx.transitions.success();
    {{< /highlight >}}

    ![](/image/share-button-save-script.png)

1. Click the **ListenForClickShare** state, drag a line to **ExeShareScript** state, and drag a line back to **ListenForClickShare** state.
    ![](/image/share-button-state-machine.png)

#### Close Button

1. The steps are similar to **Share Button**. So repeat the steps **Share Button**, change all the names from **shate** to **close**, and use the following codes instead of the previous one in **edit script**.

{{< highlight javascript >}}
	window.postMessage('sumerian-close-packet','*');
	ctx.transitions.success();
{{< /highlight >}}

## Config Default Hidden
1. Click the eye button on the left console, the corresponding entity will be disappear.
   ![](/image/config-default-hidden.png)


## Conclusion
Congratulations! You have completed the sumerian settings. So far, we cannot test the final effects as it has not been integrated with our application yet.
Please proceed to the next chapter 'Integrate Sumerian with Amplify' and after that, you will be able to test both modules.

