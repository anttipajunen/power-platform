---
title: "Use Bot Framework Composer to display an options list in chatbots"
description: "Use Bot Framework Composer to add multi-select options to your Power Virtual Agents chatbot."
keywords: "composer, adaptive card"
ms.date: 10/05/2022

ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.reviewer: makolomi
ms.custom: "cex"
ms.collection: virtualagent
---

# Example 2 - Display a multi-select options list in Power Virtual Agents

You can enhance your bot by developing custom dialogs with [Bot Framework Composer](/composer/) and then adding them to your Power Virtual Agents bot.

In this example, you'll learn how to display a multi-select list in Power Virtual Agents by using Composer.

Before you begin, ensure you read [Extend your bot with Bot Framework Composer](advanced-bot-framework-composer.md) to understand how Composer integrates with Power Virtual Agents.

> [!IMPORTANT]
> Bot Framework Composer integration is not available to users who only have the [Teams Power Virtual Agents license](requirements-licensing-subscriptions.md). You must have a [trial](sign-up-individual.md) or full Power Virtual Agents license.

## Prerequisites

- [Learn more about what you can do with Power Virtual Agents](fundamentals-what-is-power-virtual-agents.md).
- [Introduction to Bot Framework Composer](/composer/introduction).
- See how to [Extend your bot with Bot Framework Composer](advanced-bot-framework-composer.md).
- [Example 1 - Show an Adaptive Card in Power Virtual Agents](advanced-bot-framework-composer-example1.md).

## Display a multi-select options list

Using Composer, you can create a multi-select options list to be used in your chatbot.

Open the Power Virtual Agents bot used in [Example 1](advanced-bot-framework-composer-example1.md) and on the left-hand menu, select **Topics**. Select the down-arrow symbol next to **+ New topic**, and then select **Open in Bot Framework Composer** to open the bot in Composer.

While on the **Create** tab in Composer add another Bot Framework dialog. Name your new dialog **DailySpecials** in Composer.

In your new **DailySpecials** dialog in Composer select the **BeginDialog** trigger to open the **Authoring canvas**.

On the canvas, select _Add_ (**+**) then **Manage properties** then **Set a property** to create a new **Set a property** action within the **BeginDialog** trigger.

In the **Set a property** pane, set the **Property** to `conversation.days_array`.
Change the **Value** type to **\[] array**, to indicate this property will contain an array. Set the **Value** to the following array that lists the days of the week:

```JSON
["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
```

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_setArray.png" alt-text="Composer Create property.":::

Next, go to the **Bot Responses** tab in Composer and select **DailySpecials**. Select **Show code** and add the following template to the **Bot Responses** tab for **DailySpecials** to create daily offers for all the days of the week:

```lu
# DailySpecials(day)
- SWITCH: ${day}
- CASE: ${0}
    - All tofu meals are 10% off on Sundays!
    - Every Sunday, all tofu entrees are 10% off.
- CASE: ${1}
    - All steak options are 10% off on Mondays!
    - Enjoy your Monday with a special offer of 10% off on all steak dishes!
- CASE: ${2}
    - All the chicken meal options are 10% off on Tuesdays!
    - Tuesday special is 10% off on all the chicken dishes!
  - CASE: ${3}
    - All the chicken and tofu meal options are 10% off on Wednesdays!
    - Wednesday special is 10% off on all the chicken and tofu dishes!
  - CASE: ${4}
    - On Thursdays, get a free delivery in Seattle, Bellevue, and Redmond on all orders over $80!
    - Thursday special is a free delivery on orders over $80 in Seattle, Bellevue, and Redmond.
- CASE: ${5} 
    - Friday special - get a 10% discount on all dishes and delivery is free on all orders over $80!
    - Every Friday, we offer 10% off on all meals and a free delivery on orders over $80!
- CASE: ${6}
    - On Saturdays, we have a free delivery on all orders over $50.
    - Free delivery on all orders over $50 on Saturdays!
- DEFAULT:
    - Holiday special - free delivery anywhere in Seattle, Bellevue and Redmond on orders over $70 today!
    - Holiday Delivery is on us if you are in Seattle, Bellevue and Redmond and your order is over $70 total!
```

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_BotResponse.png" alt-text="Composer Bot Responses tab - bot responses code.":::

Go back to the **Create** tab in Composer and select **BeginDialog** under **DailySpecials**.

Add a new prompt for user input to this dialog by selecting **Multi-choice** under the **Ask a question** menu option.

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_askaquestion.png" alt-text="Composer Create tab - ask a multi choice questions.":::

Enter the following for the **Text** prompt:
`Please select a day:`

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_prompt.png" alt-text="Composer Create tab - add bot response.":::

Select the **User Input (Choice)** action. On the **Prompt with multi-choice** pane, under **User Input**, set **Property** to `conversation.day_choice`.

Set **Output format** to **index** to return the index of the selected option instead of a value.

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_input_variable.png" alt-text="Composer Create tab - set up choice output property.":::

Next, scroll down the **Prompt with multi-choice** pane and set **List style** to **heroCard** to display our options list vertically.

Select **Write an expression** for the **Array of choices** field and set it to use the `conversation.days_array` property we created.

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_array_multi_option.png" alt-text="Composer Create tab - set up array of choices.":::

You have created a multi-choice option list that is based on `conversation.days_array` and stores the user selection into the `conversation.day_choice` property.

You can use this `conversation.day_choice` property to display the daily special for the selected day.

Under the **User Input** action, add a **Send a response** action to your **DailySpecials** dialog. On the **Bot response** side pane, select **Show code**. Add the following expression:

```lg
- ${DailySpecials(conversation.day_choice)}
```

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_addResponse.png" alt-text="Composer Create tab - display Daily Special for the selected day.":::

In the **Bot explorer**, navigate to the Power Virtual Agents **main (root) dialog**. This dialog is the top-level read-only dialog in Composer that you created when you opened your bot in Composer. On the on the actions menu, select the **Add new trigger** option.

:::image type="content" source="media/Composer_Example2/E2_main_addNewTrigger.png" alt-text="Composer Create_tab - add new trigger.":::

Set the type of trigger to **Intent recognized** and name it **Specials**. Add the following trigger phrases for your intent and select **Submit**.

```lu
-what specials do you have
-any special deals
-do you have discounts
```

:::image type="content" source="media/Composer_Example2/E2_main_nameNewTrigger.png" alt-text="Composer Create_tab - add new Intent Recognized trigger.":::

A new trigger **Specials** will be created. Select **Begin a new dialog** under **Dialog management** to create a node that can call another dialog. Select to call **DailySpecials** on the **Begin a new dialog** side pane:

:::image type="content" source="media/Composer_Example2/E2_main_callDialog.png" alt-text="Composer Create tab - call a new dialog.":::

You are now ready to add your Composer content to your Power Virtual Agents bot. Go to the **Publish** tab in Composer and publish it to your Power Virtual Agents bot.

Once your new Composer content is successfully published, go to the Power Virtual Agents **Topics** page to verify that your new Composer content has been uploaded correctly. In your **Topics** list, you can see the new **Specials** and **DailySpecials** content that you have created in Bot Framework Composer.

> [!NOTE]
> You might need to refresh your **Topics** page in Power Virtual Agents to see the new content that has been uploaded from Composer.

:::image type="content" source="media/Composer_Example2/E2_DailySpecials_inPVA.png" alt-text="Power Virtual Agents Topics page refresh.":::

Make sure **Track between topics** is turned on, and test your new bot content by entering the following text in the **Test bot** pane in Power Virtual Agents to start a conversation:

- **Do you have any specials?**

:::image type="content" source="media/Composer_Example2/Example2._cropped.png" alt-text="Power Virtual Agents Test pane.":::

[!INCLUDE [Publish Composer](includes/composer-publish-note.md)]

[!INCLUDE[footer-include](includes/footer-banner.md)]
