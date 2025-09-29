# Free Feedback Tool with Google Sheets Integration

Try it out here: https://ycb.github.io/free-feedback-tool-google-sheets/

A lightweight, customizable feedback tool for web applications that allows users to submit feedback directly from your website with optional Google Sheets integration.

## ✨ Features

- **Element Inspection**: Click on any element to provide feedback
- **Screenshot Capture**: Automatically captures screenshots of the feedback location
- **Multiple Feedback Types**: Bug reports, suggestions, and praise
- **Sentiment Tracking**: Positive, neutral, or negative feedback
- **Google Sheets Integration**: Optional automatic sync to Google Sheets
- **Local Storage Fallback**: Works offline with localStorage backup
- **Keyboard Shortcuts**: `Ctrl+Shift+F` to start feedback mode
- **Responsive Design**: Works on desktop and mobile devices

## 🚀 Quick Start

### Installation

```bash
npm install @ycb/free-feedback-tool-google-sheets
```

### Basic Usage

```jsx
import React from 'react';
import { FeedbackSystem } from '@ycb/free-feedback-tool-google-sheets';

function App() {
  return (
    <div>
      <h1>My Website</h1>
      <p>Your website content here...</p>
      
      {/* Add the feedback system */}
      <FeedbackSystem />
    </div>
  );
}
```

### With Google Sheets Integration

1. Create a Google Apps Script that receives POST data and writes to a Google Sheet
2. Set the environment variable:

```bash
VITE_GOOGLE_APPS_SCRIPT_URL=https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec
```

## 📋 Google Apps Script Setup

Create a new Google Apps Script with the following code:

```javascript
function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);
    
    // Open your Google Sheet
    const sheet = SpreadsheetApp.openById('YOUR_SHEET_ID').getActiveSheet();
    
    // Add headers if this is the first row
    if (sheet.getLastRow() === 0) {
      sheet.getRange(1, 1, 1, 8).setValues([
        ['Timestamp', 'Category', 'Sentiment', 'Message', 'Email', 'Page URL', 'User Agent', 'Screenshot']
      ]);
    }
    
    // Add the feedback data
    sheet.appendRow([
      data.timestamp,
      data.category,
      data.sentiment,
      data.message,
      data.email || '',
      data.pageUrl,
      data.userAgent,
      data.screenshot
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({ success: true }))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ success: false, error: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

## 🎯 How It Works

1. **Click "Provide Feedback"** button or press `Ctrl+Shift+F`
2. **Click any element** on your website to pin it
3. **Fill out the feedback form** with category, sentiment, and message
4. **Submit feedback** - it's automatically saved to localStorage and optionally sent to Google Sheets

## 🛠️ Customization

### Styling

The tool uses Tailwind CSS classes and can be customized by overriding the default styles:

```css
.feedback-pin {
  /* Custom pin styles */
}

.feedback-modal {
  /* Custom modal styles */
}
```

### Configuration

```jsx
<FeedbackSystem
  googleSheetsUrl="https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec"
  categories={['bug', 'suggestion', 'praise', 'feature-request']}
  sentiments={['positive', 'neutral', 'negative']}
  theme="light" // or "dark"
/>
```

## 📱 Demo

Check out the live demo in the `test-full.html` file:

```bash
# Serve the demo locally
python3 -m http.server 3000
# Open http://localhost:3000/test-full.html
```

## 🔧 Development

### Build the Library

```bash
npm run build
```

### Run Tests

```bash
npm test
```

### Development Mode

```bash
npm run dev
```

## 📦 Package Contents

- `FeedbackSystem` - Main component
- `FeedbackModal` - Modal dialog for feedback form
- `FloatingFeedbackButton` - Floating action button
- `FeedbackPin` - Visual pin for clicked elements
- UI Components (Button, Dialog, Input, etc.)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details

## 🆘 Support

- Create an issue for bug reports
- Start a discussion for feature requests
- Check the demo page for usage examples

## 🔗 Links

- [GitHub Repository](https://github.com/ycb/free-feedback-tool-google-sheets)
- [NPM Package](https://www.npmjs.com/package/@ycb/free-feedback-tool-google-sheets)
- [Demo Page](./test-full.html)

---

Made with ❤️ for better user feedback collection
