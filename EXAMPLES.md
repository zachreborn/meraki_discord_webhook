# Examples and Use Cases

## Sample Alert Types and Their Outputs

### 1. Critical Gateway Offline Alert

**Meraki Alert Data:**
```json
{
  "alertType": "Gateway went down",
  "alertLevel": "critical",
  "organizationName": "Acme Corporation",
  "networkName": "Headquarters",
  "deviceName": "MX64-HQ-01",
  "occurredAt": "2023-10-16T09:15:23Z"
}
```

**Discord Output:**
- **Color**: Red (#FF4545)
- **Title**: Gateway went down (clickable to network)
- **Organization**: Acme Corporation (clickable)
- **Network**: Headquarters (clickable) 
- **Device**: MX64-HQ-01 (clickable)
- **Alert Level**: critical
- **Alert Data**: [Formatted JSON with device details]
- **Occurred At**: 2023-10-16T09:15:23Z

### 2. Warning Port Flapping Alert

**Meraki Alert Data:**
```json
{
  "alertType": "Port flapping detected",
  "alertLevel": "warning", 
  "organizationName": "Acme Corporation",
  "networkName": "Branch Office",
  "deviceName": "MS220-BR-02",
  "occurredAt": "2023-10-16T14:22:11Z"
}
```

**Discord Output:**
- **Color**: Orange (#FF9500)
- **Title**: Port flapping detected
- **Alert Level**: warning
- [Additional fields as above]

### 3. Info Client Connection Alert

**Meraki Alert Data:**
```json
{
  "alertType": "Client connected",
  "alertLevel": "info",
  "organizationName": "Acme Corporation", 
  "networkName": "Guest Network",
  "deviceName": "MR36-LOBBY",
  "occurredAt": "2023-10-16T16:45:33Z"
}
```

**Discord Output:**
- **Color**: Blue (#1ABC9C)
- **Title**: Client connected
- **Alert Level**: info
- [Additional fields as above]

## Customization Examples

### Adding a Custom Footer

Add this to the embed object:

```json
"footer": {
  "text": "Meraki Network Monitor • Powered by IT Team",
  "icon_url": "https://your-domain.com/meraki-icon.png"
}
```

### Adding Timestamp Display

Add this to the embed object:

```json
"timestamp": "{{occurredAt}}"
```

This will show "Today at 2:15 PM" style timestamps in Discord.

### Custom Field for Uptime

Add this to the fields array:

```json
{
  "name": "Device Uptime",
  "value": "{{deviceUptime | default: 'Unknown'}}",
  "inline": true
}
```

### Conditional Fields

Show different fields based on alert type:

```json
{
  "name": "{% if alertType contains 'Gateway' %}WAN Status{% else %}Port Status{% endif %}",
  "value": "{{wanStatus | default: portStatus}}",
  "inline": false
}
```

## Advanced Template Features

### Using Filters

Meraki webhook templates support these filters:

- `json_markdown`: Formats JSON data with syntax highlighting
- `default: 'fallback'`: Provides fallback values
- `date: '%B %d, %Y'`: Formats dates
- `upcase`: Converts to uppercase
- `downcase`: Converts to lowercase

Example:
```json
"value": "{{alertLevel | upcase | default: 'UNKNOWN'}}"
```

### Complex Conditionals

```json
"color": "{% if alertLevel == 'critical' %} 16711680 {% elsif alertLevel == 'warning' %} 16776960 {% elsif alertLevel == 'info' %} 65280 {% else %} 8421504 {% endif %}"
```

### Multiple Embeds

Send multiple embeds in one webhook:

```json
{
  "embeds": [
    {
      "title": "Primary Alert",
      "description": "{{alertType}}",
      "color": 16711680
    },
    {
      "title": "Quick Actions",
      "description": "• [View Dashboard]({{networkUrl}})\\n• [Check Device]({{deviceUrl}})",
      "color": 3447003
    }
  ]
}
```

## Testing Your Template

### Manual Testing with cURL

```bash
curl -X POST "YOUR_DISCORD_WEBHOOK_URL" \\
  -H "Content-Type: application/json" \\
  -d '{
    "embeds": [{
      "title": "Test Alert",
      "color": 16711680,
      "fields": [{
        "name": "Status",
        "value": "Testing webhook template",
        "inline": false
      }]
    }]
  }'
```

### Using Postman

1. Create new POST request
2. URL: Your Discord webhook URL
3. Headers: `Content-Type: application/json`
4. Body: Raw JSON with your template structure
5. Replace template variables with test data

### Common Test Scenarios

1. **Critical Alert**: Test red color and urgent messaging
2. **Warning Alert**: Test orange color and moderate urgency  
3. **Info Alert**: Test blue color and informational tone
4. **Unknown Level**: Test fallback color behavior
5. **Long Alert Data**: Test JSON formatting with complex data
6. **Special Characters**: Test with device names containing symbols

## Color Reference

### Decimal Color Values

| Color Name | Hex Code | Decimal Value | Use Case |
|------------|----------|---------------|-----------|
| Red        | #FF0000  | 16711680      | Critical alerts |
| Orange     | #FFA500  | 16753920      | Warning alerts |
| Yellow     | #FFFF00  | 16777215      | Caution alerts |
| Green      | #00FF00  | 65280         | Success/recovery |
| Blue       | #0000FF  | 255           | Info alerts |
| Purple     | #800080  | 8388736       | Maintenance |
| Gray       | #808080  | 8421504       | Unknown/default |

### Converting Colors

```javascript
// Hex to decimal
parseInt('0xFF4545')  // Returns: 16729413

// RGB to decimal  
(r << 16) + (g << 8) + b  // Where r,g,b are 0-255
```