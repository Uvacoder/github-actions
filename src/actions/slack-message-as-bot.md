---
path: '/slack-message-as-bot'
title: 'Slack Message as bot'
github_url: 'https://github.com/pullreminders/slack-github-action'
author: 'pullreminders'
tags: ['slack']
subtitle: '
This action wraps the Slack chat.postMessage API method for posting to channels, private groups, and DMs. This action is designed to be used with Slack bot tokens. Slack bots have two main advantages versus user tokens and incoming webhooks: (1) Bots cant be disabled inadvertently when a Slack user is disabled or removed. Slack has written about this in a recent announcement, and (2) Bots offer a powerful range of capabilities that can be leveraged to perform more functions.'
---

## Usage:

```workflow
action "Post message to Slack" {
  uses = "pullreminders/slack-github-action@master"
  secrets = [
    "SLACK_BOT_TOKEN",
  ]
  args = "{\"channel\":\"C1234567890\",\"text\":"Hello world"}"
}
```

Here's what the Slack message would look like:

<img src="docs/images/slack-message-example.png" width="540">

## Setup

To use this GitHub Action you'll first need to create a Slack App and install it to your Slack workspace.

### Creating a Slack App

1. **Create a Slack App**. Go to [Slack's developer site](https://api.slack.com/apps) then click "Create an app". Name the app "GitHub Action" (you can change this later) and make sure your team's Slack workspace is selected under "Development Slack Workspace" ([see screenshot](docs/images/slack-app.png)).
2. **Add a Bot user**. Browse to the "Bot users" page listed in the sidebar. Name your bot "GitHub Action" (you can change this later) and leave the other default settings as-is ([see screenshot](docs/images/bot-user.png)).
3. **Set an icon for your bot.** Browse to the "Basic information" page listed in the sidebar. Scroll down to the section titled "Display information" to set an icon. Feel free to use one of the [icons in this repository](docs/app-icons).
4. **Install your app to your workspace.** At the top of the "Basic information" page you can find a section titled "Install your app to your workspace". Click on it, then use the button to complete the installation ([see screenshot](docs/images/install-slack-all.png)).

## Using the action

To use this GitHub Action, you'll need to set a `SLACK_BOT_TOKEN` secret on GitHub. To get your Slack bot token, browse to the "OAuth & Permissions" page listed in Slack and copy the "Bot User OAuth Access Token" beginning in `xoxb-`.

### Posting messages

Slack's [chat.postMessage](https://api.slack.com/methods/chat.postMessage) method accepts a JSON payload containing options ??? this JSON payload should be supplied as the argument in your GitHub Action. At a bare minimum, your payload must include a channel ID and the message. Here's what a basic message might look like:

```workflow
action "Post message to Slack" {
  uses = "pullreminders/slack-github-action@master"
  secrets = [
    "SLACK_BOT_TOKEN",
  ]
  args = "{\"channel\":\"C1234567890\",\"text\":"Hello world"}"
}
```

Please note that if you are using the visual editor you should not escape quotes because GitHub will automatically escape them for you.

#### Channel IDs

A "channel ID" can be the ID of a channel, private group, or user you would like to post a message to. Your bot can message any user in your Slack workspace but needs to be invited into channels and private groups before it can post to them.

If you open Slack in your web browser, you can find channel IDs at the end of the URL when viewing channels and private groups. Note that this doesn't work for direct messages.

```
https://myworkspace.slack.com/messages/CHANNEL_ID/
```

You can also find channel IDs using the Slack API. Get a list of channels that your bot is a member of via Slack's [users.conversations](https://api.slack.com/methods/users.conversations) endpoint. Get user IDs for direct messages using Slack's [users.lookupByEmail](https://api.slack.com/methods/users.lookupByEmail) endpoint

#### Formatting messages

Please refer to [Slack's documentation](https://api.slack.com/docs/messages) on message formatting. They also have a [message builder](https://api.slack.com/docs/messages/builder) that's great for playing around and previewing messages. Your messages can contain attachments, markdown, buttons, and more.

## Secrets

- `SLACK_BOT_TOKEN` - **Required**. The Slack bot token to use for posting messages ([more info](https://api.slack.com/docs/token-types#bot))
