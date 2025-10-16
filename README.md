# Meraki Discord Webhook

A comprehensive Discord webhook template for Cisco Meraki that transforms network alerts into rich, formatted Discord messages with dynamic styling and clickable links.

## Overview

This webhook template integrates Cisco Meraki network alerts with Discord channels, providing real-time notifications with:

- **Dynamic color coding** based on alert severity
- **Rich embed formatting** with organized field layout
- **Clickable links** to Meraki dashboard resources
- **JSON markdown formatting** for alert data
- **Conditional logic** for alert-specific styling

## Features

### Alert Severity Color Coding
- 🟢 **Info alerts**: Blue (#1ABC9C / 1752220)
- 🟠 **Warning alerts**: Orange (#FF9500 / 16752640) 
- 🔴 **Critical alerts**: Red (#FF4545 / 16711769)
- ⚪ **Default/Unknown**: Blue fallback

### Rich Information Display
- Organization name and direct link
- Network name and direct link
- Device name and direct link
- Alert level with visual emphasis
- Formatted alert data as JSON markdown
- Timestamp of when the alert occurred

### Template Variables
The webhook uses Meraki's built-in template variables:

| Variable | Description | Example |
|----------|-------------|----------|
| `{{alertType}}` | Type of alert triggered | "Gateway down" |
| `{{alertLevel}}` | Severity level | "critical", "warning", "info" |
| `{{organizationName}}` | Organization name | "Acme Corp" |
| `{{organizationUrl}}` | Direct link to organization | `https://dashboard.meraki.com/...` |
| `{{networkName}}` | Network name | "Main Office" |
| `{{networkUrl}}` | Direct link to network | `https://dashboard.meraki.com/...` |
| `{{deviceName}}` | Device name | "MX-001" |
| `{{deviceUrl}}` | Direct link to device | `https://dashboard.meraki.com/...` |
| `{{alertData}}` | Raw alert data | JSON object with alert details |
| `{{occurredAt}}` | Alert timestamp | "2023-10-16T14:30:00Z" |

## Setup Instructions

### Step 1: Create Discord Webhook
1. Navigate to your Discord server
2. Go to **Server Settings** → **Integrations** → **Webhooks**
3. Click **New Webhook**
4. Choose the target channel for alerts
5. Copy the webhook URL (you'll need this later)

### Step 2: Configure Meraki Dashboard
1. Log into your [Meraki Dashboard](https://dashboard.meraki.com)
2. Navigate to **Network-wide** → **Alerts**
3. Scroll to the **Webhooks** section
4. Click **Add or edit webhook templates**

### Step 3: Add the Template
1. Copy the entire contents of `webhook.json`
2. Paste into the Meraki webhook template editor
3. Give your template a descriptive name (e.g., "Discord Rich Embed")
4. Click **Save**

### Step 4: Configure Webhook Endpoint
1. In the **Webhook URL** field, paste your Discord webhook URL
2. Select your newly created template from the dropdown
3. Configure which alerts should trigger notifications
4. Click **Save Changes**

## Template Structure

The webhook template creates a Discord embed with the following structure:

```json
{
  "embeds": [{
    "title": "Alert Type",
    "url": "Network Dashboard Link", 
    "color": "Dynamic Color Based on Alert Level",
    "fields": [
      // Organization info with link
      // Network info with link  
      // Device info with link
      // Alert level
      // Formatted alert data
      // Timestamp
    ]
  }]
}
```

## Customization

### Modifying Colors
To change alert colors, edit the `color` field in `webhook.json`:

```json
"color": "{% if alertLevel == 'info' %} YOUR_INFO_COLOR {% elsif alertLevel == 'warning' %} YOUR_WARNING_COLOR {% elsif alertLevel == 'critical' %} YOUR_CRITICAL_COLOR {% else %} YOUR_DEFAULT_COLOR {% endif %}"
```

Use decimal color values. Convert hex colors using: `parseInt('0xFFFFFF')`

### Adding Custom Fields
Add new fields to the `fields` array:

```json
{
  "name": "Custom Field",
  "value": "{{customVariable}}",
  "inline": true
}
```

### Modifying Layout
- Set `"inline": true` for side-by-side fields
- Set `"inline": false` for full-width fields
- Fields with `inline: true` will group in sets of 3 per row

## Troubleshooting

### Common Issues

**Webhook not firing:**
- Verify the Discord webhook URL is correct
- Check that alerts are enabled for the specific event types
- Ensure the template is selected in the webhook configuration

**Messages not formatting correctly:**
- Validate JSON syntax in the template
- Check that all template variables are spelled correctly
- Ensure Discord webhook URL permissions allow embeds

**Colors not displaying:**
- Verify color values are valid integers
- Check conditional logic syntax in template
- Ensure alertLevel values match expected strings

### Testing the Integration

1. **Manual Test**: Use Discord's webhook tester or tools like Postman
2. **Trigger Test Alert**: Temporarily disconnect a device to generate an alert
3. **Check Logs**: Monitor Meraki event logs for webhook delivery status

## Example Output

When an alert fires, Discord will display:

```
🔴 Gateway Offline

📊 Organization: Acme Corp
🌐 Network: Main Office  
📱 Device: MX-001

⚠️ Alert Level: critical

📋 Alert Data:
{
  "deviceSerial": "Q2XX-XXXX-XXXX",
  "lastReportedAt": "2023-10-16T14:28:15Z",
  "alertTypeId": "gateway_down"
}

🕐 Occurred At: 2023-10-16T14:30:00Z
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Test your changes thoroughly
4. Submit a pull request with detailed description

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support

For issues related to:
- **Discord webhooks**: Check [Discord Developer Documentation](https://discord.com/developers/docs/resources/webhook)
- **Meraki webhooks**: Consult [Meraki API Documentation](https://developer.cisco.com/meraki/webhooks/)
- **This template**: Open an issue in this repository
