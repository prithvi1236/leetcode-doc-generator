# LeetCode Documentation Generator

A Chrome extension that helps students document their LeetCode problem solutions in a standardized .docx format with intelligent code extraction and automatic page refresh capabilities.

## 🚀 Quick Install

<div align="center">

![Installation Demo](https://github.com/user-attachments/assets/1ff17fb2-37c3-4de9-9b36-c9b07efb6778)

*[Watch full tutorial on YouTube](https://www.youtube.com/watch?v=oswjtLwCUqg)*

</div>

1. Download [leetcode-doc-generator](https://github.com/prithvi1236/leetcode-doc-generator/archive/refs/heads/main.zip)
2. `chrome://extensions` → Developer mode → Load unpacked → Select folder leetcode-doc-generator-main
3. ✅ Done!

## Features

- ✅ **Smart Code Extraction**: Automatically capture LeetCode submission details from submission pages
- ✅ **Keyboard Shortcut**: Quick capture with Ctrl+Shift+K - opens popup and auto-captures
- ✅ **Auto-Redirect & Extract**: Handles `/submissions/detail/{id}/` URLs automatically
- ✅ **Problem Management**: Manage multiple problems in a problem set with full CRUD operations
- ✅ **Drag & Drop Reordering**: Reorder problems with intuitive drag-and-drop interface
- ✅ **Professional Document Generation**: Generate beautifully formatted .docx documents
- ✅ **Multi-Language Support**: Smart language detection (handles multiple code blocks on page)

## Project Structure

```
leetcode-doc-generator/
├── manifest.json          # Extension configuration (Manifest V3)
├── package.json           # Project metadata and dependencies
├── popup.html            # Popup UI with enhanced status indicators
├── popup.js              # Popup logic with auto-refresh capabilities
├── popup.css             # Popup styling with loading animations
├── content.js            # Advanced DOM extraction & code cleaning
├── background.js         # Background service worker
├── docxGenerator.js      # Professional .docx file generation
├── storage.js            # Chrome storage operations (CRUD)
├── docx.min.js           # docx library (CDN loaded)
└── icons/                # Extension icons
    ├── icon16.png
    ├── icon48.png
    ├── icon128.png
    ├── bmc-brand-icon.png  # Buy Me a Coffee icon
    └── README.md
```

## Usage

### 1. Set up problem set info:
- Click the extension icon in the toolbar
- Enter your problem set title and student name
- Click "Save Problem Set Info"

### 2. Capture submissions:

**Method 1: Using Keyboard Shortcut (Fastest)**
- Navigate to any LeetCode submission page (standard or detail-only URL)
- Press **Ctrl+Shift+K** (or customize at chrome://extensions/shortcuts)
- Popup opens and automatically captures the submission
- Works on both URL formats:
  - `/problems/{slug}/submissions/{id}/`
  - `/submissions/detail/{id}/` (auto-redirects and extracts)

**Method 2: Manual Capture**
- Navigate to a LeetCode submission page
- Click the extension icon
- Click "Capture from Current Page"
- **Auto-refresh**: If content script isn't loaded, the page will refresh automatically
- **Visual feedback**: Watch the button states and status messages
- The problem will be added to your list with clean, properly formatted code

### 3. Manage problems:
- **Reorder**: Use drag-and-drop
- **Edit**: Modify problem details with the edit button
- **Delete**: Remove individual problems
- **Clear All**: Remove all problems at once

### 4. Generate document:
- Click "Generate Document" to download a formatted .docx file
- Filename format: `{Student Name} - {Problem Set Title}.docx`

### 5. Start new problem set:
- Click "Start New Problem Set" to clear all data and begin fresh

## Document Format

Generated .docx documents include:

- **Header:** Problem set title (bold, 24pt) and student name (12pt)
- **For each problem:**
  - Problem name (bold, 18pt, Arial)
  - Submission link (12pt, Arial, black text)
  - Clean code (10pt, Courier New, red text, monospace)
- Professional sans-serif font (Arial) for all non-code text
- Proper spacing between sections
- **Clean formatting**: No line numbers, preserved indentation

## Technical Details

### Code Extraction & Cleaning
- **DOM Parsing**: Intelligently extracts code from LeetCode submission pages
- **HTML Filtering**: Removes React syntax highlighter line number elements automatically
- **Line Number Removal**: Strips various formats (`1 code`, `1.code`, `1|code`) while preserving indentation
- **Multi-Language Detection**: Prioritizes actual code over decorative elements, handles multiple languages per page

### Auto-Refresh System
- **Content Script Detection**: Automatically checks if extraction scripts are loaded
- **Smart Recovery**: Refreshes page and retries if content script unavailable
- **Visual Feedback**: Real-time status updates with loading animations and countdown timers

### Data Management
- **Validation**: Comprehensive input validation for all captured data
- **Storage**: Uses Chrome's local storage API for persistent data across sessions
- **CRUD Operations**: Full create, read, update, delete support for problems and problem sets

### Document Generation
- **Professional Formatting**: Generates .docx files with proper fonts, spacing, and structure
- **Clean Code Output**: Monospace font with preserved indentation, no line number artifacts

## Requirements & Limitations

**Requirements:**
- Chrome browser (Manifest V3 compatible)
- Internet connection (for docx library)
- LeetCode submission pages

**Limitations:**
- LeetCode submission pages only
- Maximum 100,000 characters per code submission
- Requires JavaScript enabled

**Permissions:**
- `storage` - Save problems and settings
- `activeTab` - Read pages and auto-refresh
- LeetCode domain access

## Troubleshooting

**Common Issues:**
- **Content script not loaded** → Extension auto-refreshes page
- **Code extraction fails** → Ensure page is fully loaded
- **Line numbers in output** → Updated extraction handles this automatically
- **Missing indentation** → Enhanced algorithm preserves formatting

**Debug:** Check browser console for detailed logs during capture process.

## License

MIT - Feel free to use, modify, and distribute.
