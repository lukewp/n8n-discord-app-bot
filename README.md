# n8n Discord Bot App

## 1. n8n Discord Keepalive Workflow
This is a very barebones n8n workflow one can use to get a Discord App running within n8n. I use this as a "Keepalive" workflow that runs in parallel with other functions you want your bot to run. It handles the basic functionality for the interactions URL.

Follow these steps:
1. Enable Community Nodes in the n8n UI via Settings: click the three dots next to your name at the bottom-left, select `Settings` then `Community nodes` 
2. Install `n8n-nodes-tweetnacl` node on your n8n server in the n8n UI via `Settings` ... `Community nodes` ... `Install`
3. Return to the main n8n menu and create a new workflow
4. Copy the code from `Discord App Bot Signature Verification API.json` in the `/workflows` folder from this repo
5. Click into the empty workflow canvas you created, and paste (Ctrl-V) the json you copied.
6. Create a new Discord App in the [Discord Developer interface](https://discord.com/developers/applications)
7. On the Discord Site, in your new app's settings, Under General Information, find the `PUBLIC KEY`, and click the `Copy` button
8. In your n8n workflow, double-click the `Discord App Config` node
9. Paste the PUBLIC KEY value you copied in as the value for `publicKey`
10. Save your workflow
11. Open the first node (called `Discord POSTs here`) and click the slider at the top from `Test URL` to `Production URL`, copy the `Production URL` value.
12. Click `Active` at the top of your workflow to activate it and then click `Save` 
13. Click back to your Discord App settings: under the General Information tab where you found `PUBLIC KEY`, scroll down to find the empty box under `Interactions Endpoint URL`, paste your n8n webhook URL here.
14. Click `Save Changes` at the bottom.

Congratulations, you should now have a running stub of a Discord Bot App. Just keep this active and running on your server to keep your app active.

## 2. n8n Discord Functional Workflow - Github-Repo-RAG Technical Support Chatbot
Once you've built the Keepalive bot, it's time to get a real AI-backed bot implemented. The first thing we're going to do is install the community node `n8n-nodes-discord-trigger` which you should know how to do because we installed the `n8n-nodes-tweetnacl` community node in our first step. Then we're going to use the trigger node from this community node to listen to all messages on all channels our bot is paying attention to. Then we're going to wire up the other Discord Messages node from n8n-core to retrieve the last several messages for context, reformat them and send through a prompt to ChatGPT, who we're also going to expose a Github node tool to, then run an IF switch to decide whether or not our app will contribute to the channels or threads it's in, based on recent discourse.

Making this work is a little trickier; Follow these steps:
1. On the `App Settings ... Installation` page, give your bot the following defaults:
    * User Install scopes:
        * applications.commands
    * Guild Install scopes:
        * applications.commands
        * bot
    * Guild Install permissions:
        * Add Reactions
        * Attach Files
        * Create Private Threads
        * Create Public Threads
        * Embed Links
        * Manage Channels
        * Manage Messages
        * Manage Threads
        * Read Message History
        * Send Messages
        * Send Messages in Threads
        * Send TTS Messages
        * View Channels
2. From the main n8n page, create three different Discord credentials in n8n and open them in parallel tabs in your browser: 
    * A **Discord Bot API**
    * A **Discord OAuth2 API**
    * A **Discord Bot Trigger API**
    * Each of these credential types requires something a little different. I'd start with the easiest one, the Discord Bot API: it only needs the `Bot Token`. But you have to paste the `Bot Token` into the other two credentials as well -- and you have to do all of them at the same time, because once you click off the page that shows you the token, you can't get back to it. Next, add your `Client ID` to the Discord Bot Trigger API, and the Discord OAuth2 API. From the Discord App Settings site's OAuth2 page where you can see the `Client ID`, you can copy the `Client Secret` into the OAuth2 API -- this is another one you can only copy once. Finally, open another browser tab and go to your n8n Settings page, create a new API key for your n8n instance and copy it into the Bot Trigger API credential page. (You can only copy this one once, also)
3. You should now be able to test and save all your credentials. Once you see the green "Tested Successfully" message, we have to copy-paste back into the Discord App Settings page:
    * From your n8n Discord OAuth2 API page, copy the Callback URL and paste it in the OAuth2 settings page, into a new URI box under the Redirects header (click the blue button to enable the form to accept the URL).
4. In Github, go to your profile, Developer Settings, and create a new token. Give it permission to read the repo you want it to read.
5. In n8n, create a Github credential and paste in the token you just created.
6. In n8n, create an OpenAI API credential and give it the info from your OpenAI API account.
7. Create a blank canvas in n8n and copy-paste the contents of `Github-powered tech support Discord Bot.json` from the workflows folder. Adjust the credentials in the nodes to point at your newly-created credentials for Discord, OpenAI, and Github, and tweak the prompt as appropriate for your use case.
8. In theory, if you clicked "Connect" on the OAuth2 Discord n8n Credential, you already added your bot to a server you wanted it added to. You should be able to start talking to your bot now. If not, you're going to have to do some diagnosis but you're likely very very close to a working implementation of an AI-powered chatbot that replies to messages in your Discord channel, drawing on the Github repo you gave it access to. Check each of the nodes for error messages.