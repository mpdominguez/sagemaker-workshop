---
title: "Create Additional Personalize Campaigns"
chapter: false
weight: 7
---

If you have built the additional two Personalize models, for Item-to-Item Similarities and Personal Rankings, then you'll need to create the associated campaigns for these solutions, as it is the campaigns that we will add to the application. If those solutions have been built then continue with these steps, but if not you can always come back to these steps later before adding them to the application.

In the AWS Console, go to the Amazon Personalize service console, click on Dataset groups link on the left-hand menu, and select the personalize-recs-dataset-group link, then click into the Campaigns menu item on the left. Select Create campaign

![campaignSingle](/images/campaignSingle.png)

First, we want to build the campaign for the Similar Items model - enter a name for the campaign, such as similar-items-campaign, select via the drop-down that solution that you previously built in the console, similar-items-solution, and ensure that minimum TPS is set to 1. Hit Create campaign

![createCampaign](/images/createCampaign.png)

Now build the campaign for the Personal Rankings model - follow the same steps as before, but this time use rankings-campaign for the campaign name and select the rankings-solution model in the drop-down control.
