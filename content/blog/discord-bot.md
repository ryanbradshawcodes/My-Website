---
title: "Creating a discord bot for my CSE server"
date: 2022-07-26
slug: "discordbot"
description: "A guide on how I created and deployed my bot for my CSE discord server"
keywords: ["guide", "website", "discord", "host", "warwick", "university", "student", "v14", "discord.js"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

I wanted to create a Discord server just for Computer Systems Engineers to increase collaboration with the small number of people that take the course. To extend this into a proper learning opportunity I then decided I wanted to create my own discord bot using [discord.js](https://discord.js.org/#/). 

The main thing I wanted to implement was a role selector such that users could select their current year of study. This role would then be used to determine which channels the user can see and interact with.

## Prerequisites

Creating the discord channel itself requires little technical expertise. For the bot, it requires basic knowledge of the language JavaScript. At the start of each subsection I will provide some brief, bulleted steps. I would encourage you to still watch the video as this will ensure you do not miss anything.

Anything that needs downloading will be linked in the bulleted steps where appropriate.

## Development

### Initial setup

* [Install node.js](https://nodejs.org/en/)
* [Install Visual Studio Code](https://code.visualstudio.com/)
* Access the [Discord Developer Portal](https://discord.com/developers/applications)
* Add your new bot to your server (should be offline at this stage)
* Start coding! 
* Check the bot is now online
* Create your first command
* Test it out

{{< youtube 6IgOXmQMT68 >}}

### Creating a generic menu selector

* Create a components folder
* Create a handle components file
* Ignore the button specific code

{{< youtube dbfF570IyCg >}}

* Now you should have the general framework to implement the menu
* Follow the tutorial below and create a generic menu selector

{{< youtube Ance5go0e0M >}}

### Creating a role selector

* Now you should have the foundation in place for a select menu
* Navigate to ```src/commands/tools/<name of the menu file>```
* Adjust the roles to your liking
* Navigate to ```src/events/client/interactionCreate.js```
* Navigate within the ```isSelectMenu()``` else if statement
* Define the roles you want using their IDs
* Create an ```if``` statement to specify the role selector menu
* Change the role of the member depending on the option they pick

To define the roles you can use:
```const [ROLE_NAME] = interaction.guild.roles.cache.find(role => role.id === '[ID]');```


To specify your role selector menu you can use:
``` if (interaction.customId === '[CUSTOM_ID_OF_MENU]') {...}```

Finally you can take the users choice and determine their roles as such:
Within the previous if statement we just created ```if (interaction.values[0] === '[VALUE_OF_MENU_OPTION]')```
Now you can add and remove roles with ```await member.roles.add/remove([ROLE_NAME])```

### 24/7 Bot uptime

* Create a [Heroku account](https://signup.heroku.com/login)
* Install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-**cli**)
* Install [https://git-scm.com/](https://git-scm.com/)
* Create the ```.gitignore``` file (copy in token even if using .env)
* Create the ```Procfile```
* Create a repository and deploy it
* Activate the worker command on Heroku
* Check the logs and your discord to see if your bot is online and working!

{{< youtube EY4T1vOVJDY >}}

## The final result

* Feel free to add me on Discord by the way!

{{< youtube E2AqIqLh-Lc >}}

## Reflection

This was a fun mini-project to complete first of all. I found it quite engaging for a few reasons:

* I made myself accountable by telling people I was making a Discord server
* I use Discord a lot
* Due to the recent changes to discord.js I could not just completely rely on video tutorials

Using JavaScript for the first time alonside discord.js was overwhelming and if I wanted a smoother devleopment experience I should have first learnt some of the JavaScript syntax. This would have made it easier for me to understand the code, though I did manage to work my way through by reading the [documentation](https://discord.js.org/#/docs/discord.js/main/general/welcome) and various help logs on the discord.js [Discord channel](https://discord.gg/djs).

I might add more features to this bot, although currently I have no specific plans as I only wanted a role selector when I first envisioned this project.

