## n8n Discord App 
This is a very barebones n8n workflow one can use to get a Discord App running within n8n. 

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

Congratulations, you should now have a running stub of a Discord Bot App.