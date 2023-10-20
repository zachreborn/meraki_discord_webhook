# meraki_discord_webhook
Cisco Meraki webhook template for Discord. This JSON template will allow Cisco Meraki to send alerts to a Discord channel.

# How to Use
1. Create a new webhook in your Discord server
2. Tie the webhook to a specific channel
3. Login to your Meraki dashboard
4. Navigate to Network-wide > Alerts
5. Navigate to the Webhooks section
6. Click "Add or edit webhook templates"
7. Copy + paste the contents of the `webhook_template.json` file into the text box
8. Enter a name for the template
9. Save the template
10. Input the Discord webhook URL into the "Webhook URL" field and select your new "Discord" template.
11. Click "Save Changes"
