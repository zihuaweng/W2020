Issue Description

Telegram has a secrect chat function. In secrect chat, messages are be set to be self-distructable. However, the self-destruct only happen on the receiver side, the message will remain on the sender's secret chat view. We think this is not secure enough, as the sender's phone can be seen by other people. Thus we decide to add the self-destruct function to the sender's side. From the screenshot, we can see that after more than 5s, the 5s self-destruct message was still on the sender's chat view.

<img src="https://s2.ax1x.com/2020/03/08/3x3cEF.png" height = "400" alt="self-destruct" align=center />

To finish this functionality, we are going to locate the code that creating a secret message, update the method setting view to destroy message based on the related timer, remove the message stored in the storage/cache, and add animation if possible.
