# Nextcloud Talk Dashboard Widget

A custom Vue.js widget for the Nextcloud dashboard that displays the latest messages from a selected Nextcloud Talk room and allows users to interact with chat without switching away from the dashboard.

## Features

- 📱 **Compact Design**: Fits perfectly in the Nextcloud dashboard
- 💬 **Message Display**: Shows last 10 messages with timestamps and user names
- 😊 **Emoji Reactions**: React to messages with emoji
- 💭 **Quick Replies**: Send messages directly from the dashboard
- ⚡ **Real-time Updates**: Ready for WebSocket integration
- 🎨 **Modern UI**: Clean, responsive design with TailwindCSS
- 🔒 **Secure**: Uses Nextcloud's authentication system

## Files Structure

```
├── TalkWidget.vue          # Main Vue component
├── talkApi.js             # API utility functions
├── auth.js                # Authentication stub
├── example-usage.html     # Demo page
└── README.md              # This file
```

## Quick Start

### 1. Basic Usage

```vue
<template>
  <talk-widget :selected-room-id="currentRoomId" />
</template>

<script setup>
import TalkWidget from './TalkWidget.vue'
import { ref } from 'vue'

const currentRoomId = ref('your-room-id')
</script>
```

### 2. API Integration

The widget uses the Nextcloud Talk REST API:

```javascript
import { fetchMessages, sendMessage, sendReaction } from './talkApi.js'
import { getAuthToken } from './auth.js'

// Fetch messages
const token = getAuthToken()
const response = await fetchMessages('room-id', token)

// Send a message
await sendMessage('room-id', 'Hello!', token)

// Add a reaction
await sendReaction('message-id', '👍', token)
```

## API Reference

### TalkWidget.vue Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `selectedRoomId` | String | `null` | The ID of the Talk room to display |

### talkApi.js Functions

#### `fetchMessages(roomId, token, limit = 10)`
Fetches the last N messages from a Talk room.

**Parameters:**
- `roomId` (string): The room identifier
- `token` (string): Bearer token for authentication
- `limit` (number): Number of messages to fetch (default: 10)

**Returns:** Promise with messages array

#### `sendMessage(roomId, messageText, token)`
Sends a message to a Talk room.

**Parameters:**
- `roomId` (string): The room identifier
- `messageText` (string): The message content
- `token` (string): Bearer token for authentication

**Returns:** Promise with sent message data

#### `sendReaction(messageId, emoji, token)`
Adds an emoji reaction to a message.

**Parameters:**
- `messageId` (string): The message identifier
- `emoji` (string): The emoji to react with
- `token` (string): Bearer token for authentication

**Returns:** Promise indicating success/failure

### auth.js Functions

#### `getAuthToken()`
Retrieves the current authentication token. In production, this should be replaced with actual Nextcloud session/OAuth token retrieval.

**Returns:** Bearer token string

## Integration with Nextcloud

### Authentication Setup

Replace the mock authentication in `auth.js` with real Nextcloud tokens:

```javascript
// Example: Using Nextcloud's global OC object
export function getAuthToken() {
  if (typeof window !== 'undefined' && window.OC) {
    return window.OC.requestToken
  }
  return null
}
```

### Dashboard Integration

1. **Include the widget in your Nextcloud app:**
   ```javascript
   // In your Nextcloud app's dashboard
   import TalkWidget from './TalkWidget.vue'
   ```

2. **Add to your dashboard template:**
   ```vue
   <template>
     <div class="dashboard-widgets">
       <talk-widget :selected-room-id="selectedRoom" />
     </div>
   </template>
   ```

3. **Handle room selection:**
   ```javascript
   // You can implement a room selector component
   const selectedRoom = ref(null)
   ```

## Styling

The widget uses TailwindCSS classes and is designed to integrate seamlessly with Nextcloud's design system. Key styling features:

- **Responsive Design**: Adapts to different screen sizes
- **Dark Mode Ready**: Uses CSS variables for easy theming
- **Accessibility**: Proper contrast ratios and keyboard navigation
- **Custom Scrollbars**: Styled scrollbars for the messages container

## Future Enhancements

### Live Updates
Implement real-time message updates using WebSockets:

```javascript
// Example WebSocket integration
const socket = new WebSocket('wss://your-nextcloud.com/ws/talk/room/room-id')
socket.onmessage = (event) => {
  const data = JSON.parse(event.data)
  if (data.type === 'message') {
    // Add new message to the list
    messages.value.push(data.message)
  }
}
```

### Room Selector
Add a dropdown to select different Talk rooms:

```vue
<template>
  <div class="room-selector">
    <select v-model="selectedRoomId">
      <option v-for="room in rooms" :key="room.id" :value="room.id">
        {{ room.name }}
      </option>
    </select>
  </div>
  <talk-widget :selected-room-id="selectedRoomId" />
</template>
```

### Emoji Picker
Integrate an emoji picker library for better reaction UX:

```javascript
import { Picker } from 'emoji-mart-vue'

// Add emoji picker component
```

## Development

### Running the Demo

1. Open `example-usage.html` in a web browser
2. Select a room from the dropdown
3. Click "Load Mock Data" to see the widget in action

### Testing

The widget includes mock data for testing. In production, replace the mock authentication and API calls with real Nextcloud endpoints.

## Browser Support

- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

## License

This widget is designed for use with Nextcloud and follows Nextcloud's licensing terms.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## Support

For issues and questions:
- Check the Nextcloud Talk API documentation
- Review the Nextcloud developer documentation
- Open an issue in the project repository
