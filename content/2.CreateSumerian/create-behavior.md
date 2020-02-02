
---
title: "Add Action Behaviors"
chapter: false
weight: 22
---


## Add Assets Action Behaviors 

####  Red Packet 

1.  Select the **LuckyMoney** entity
    ![](/images/addSumerian/add-behaviors-select-packet.png)

1. Click **Add Component**, choose **State Machine**.    
   ![](/images/addSumerian/add-component.png)   
   ![](/image/WechatIMG8.png)
   
1. Click the **+ button**  to add behavior.     
   ![](/image/WechatIMG11.png)

1. Name the **behavior name** and the **state name** `wait for click`

1. Click **Add Action**    
   ![](/image/WechatIMG13.png)

1. Find **Click/Tap on entity** in the list or search it in the search bar. Don't forget to **Add** it.   
   ![](/image/WechatIMG14.png)

1. Click **Add State** and change the **state name** to `hide`   
   ![](/image/WechatIMG15.png)
   
1. Next We will add **three actions** to state ``hide``. Firstly, Click **Add Action**,  find **Hide** to add.
   ![](/images/addSumerian/add-action-for-hide.png)
   ![](/image/WechatIMG19.png)

1. Add another action for ``hide``. Click **Add Action** and add **Emit Message**   
   ![](/images/addSumerian/add-action-for-hide.png)
   ![](/image/WechatIMG21.png)

1. In the **channel** for ``hide``, enter `showMoney`      
   ![](/image/WechatIMG23.png)

1. Add the 3rd action **Execute Script** for ``hide`` state. 
    ![](/images/addSumerian/add-action-for-hide.png)
    ![](/images/addSumerian/execute-script.png)
    
1. Click the **+ button** to add script, choose **Custom (Legacy Format)**. 
    ![](/image/script-create-script.png)
    ![](/image/script-choose-type.png)

1. Click the **pencil** button to edit script, copy the following codes into the function **enter**, then save the change.
    ![](/image/script-edit-script.png)
    
    {{< highlight javascript >}}
        window.postMessage('sumerian-open-packet','*');
        ctx.transitions.success();
    {{< /highlight >}}

    ![](/image/script-save-change.png)

1. The 'wait for click' and 'hide' states are now **overlapped** in the same position. Use the mouse to **move the hide state around**. Then Click the **wait for click** state, drag a line from **On Click/Tap Entity** to **hide** state.
   ![](/images/addSumerian/script-drag-line.gif)

#### Red Packet with Money 

1. Click the **LuckyMoneyWithCash** entity in the left side **Entities** Panel  

1. Click **Add Component** button, choose **State Machine**    

1. Click the **+ button**     

1. Name both the **behavior name** and the **state name** to `listen`    

1. Click **Add Action**    

1. Find **Listen** and click **Add**    
   ![](/image/WechatIMG24.png)

1. In the **Message Channel** under **Selected State**, enter `showMoney`
    
1. Click **+ Add State** and name it `show`

1. Click **Add action** and add **Show**    

1. Drag a line from **On 'showMoney' event** under **listen** state to **show** state   
    ![](/images/addSumerian/script-drag-line-2.png)

#### Share Button

1. In the left side **Entities** panel, Click **Share Btn** entity, click **Add Component**, and select **State Machine**.

1. Create a behavior, name both the **behavior name** and the **state name** `ListenForClickShare`.
    ![](/image/share-button-behaviors.png)

1. Click **Add Action**, select **Click/Tap on entity**.

1. Create another state by click **+ Add State**, name it `ExeShareScript`.

1. Add action **Execute Script** for the state. Click the **+ button** to add script, choose **Custom (Legacy Format)**. 

1. Click the **pencil** button to edit script, copy the following codes into the function **enter**, then **save** the change.

    {{< highlight javascript >}}
	    window.postMessage('sumerian-share-packet','*');
	    ctx.transitions.success();
    {{< /highlight >}}

    ![](/image/share-button-save-script.png)

1. The two states are now overlapped together. Move the mouse to move one state around and then **drag a line** from **ListenForClickShare** state to **ExeShareScipt** state.
    ![](/images/addSumerian/script-drag-line-3.png)


#### Close Button
For close button part, the steps are pretty much the same with **Share Button** except **button chosen and the naming**. So repeat the steps from **Share Button** part except choosing **close btn** instead and **replacing** all the names containing **share** with **close**.
![](/images/addSumerian/close-btn.png)

Naming examples include renaming 'ListenForClickShare' to `ListenForClickClose`, renaming 'ExeShareScript' to `ExeCloseScript` and so on.
Also, use the following codes instead of the previous one in **edit script**.

{{< highlight javascript >}}
    window.postMessage('sumerian-close-packet','*');
    ctx.transitions.success();
{{< /highlight >}}

The ListenForClickClose and ExeCloseScript are now **overlapped** in the same position. Use the mouse to **move one state around**.
Then drag a line from **On Click/Tap Entity** to **hide** state.
![](/images/addSumerian/script-drag-line-4.png)

## Config Default Hidden
1. Click the **eye button** on the left console, the corresponding entity (lucky money with cash) will disappear.
   ![](/images/addSumerian/hide-cash.png)


## Configure AR Camera

1. On the left side **Entities** panel, DELETE **Default Camera**.
    ![](/images/addSumerian/add-behaviors-delete-default-camera.png)

1. There are two AR Cameras displaying in left navigation bar. 
   ![](/images/addSumerian/two-cameras.png)
   
1. Click one of them and choose **Camera** tab to find the **main AR Camera**.
The right info bar displays if that's the main camera or not. 
![](/images/addSumerian/main-camera.png)

1. Change the main camera's **Z** value of position to `2`
![](/images/addSumerian/change-camera.png)

1. **Save** the scene through **Scene** tab on the left top of Sumerian.

1. (Optional) Delete the AR Camera which is NOT the main camera.

## Conclusion
Congratulations! You have completed the sumerian settings. So far, we cannot test the final effects as it has not been integrated with our application yet. In the next session, we will create a React web application.

You could leave the Sumerian Console open, open a new tab to access AWS Console. We will come back to the Sumerian console later.

