# API Documentation

## Overview

This document provides comprehensive API documentation for the LeetCode Documentation Generator Chrome extension. The extension follows a modular architecture with well-defined interfaces between components.

## Table of Contents

- [Storage API](#storage-api)
- [Content Script API](#content-script-api)
- [Document Generation API](#document-generation-api)
- [Background Script API](#background-script-api)
- [Error Handling](#error-handling)
- [Data Models](#data-models)

## Storage API

The Storage API provides persistent data management using Chrome's local storage.

### `saveProblemSetInfo(info)`

Saves problem set metadata with validation.

**Parameters:**
- `info` (Object): Problem set information
  - `title` (string): Problem set title (2-200 characters, required)
  - `submittedBy` (string): Student name (2-100 characters, required)

**Returns:** `Promise<void>`

**Throws:** 
- `Error` - If validation fails or storage operation fails

**Example:**
```javascript
try {
  await saveProblemSetInfo({
    title: "Problem Set 6",
    submittedBy: "John Doe"
  });
  console.log('Problem set info saved successfully');
} catch (error) {
  console.error('Failed to save problem set info:', error.message);
}
```

### `getProblemSetInfo()`

Retrieves problem set metadata.

**Returns:** `Promise<Object>` - Problem set information
- `title` (string): Problem set title
- `submittedBy` (string): Student name

**Example:**
```javascript
const info = await getProblemSetInfo();
console.log(`Title: ${info.title}, Student: ${info.submittedBy}`);
```

### `addProblem(problem)`

Adds a new problem to the current problem set.

**Parameters:**
- `problem` (Object): Problem data
  - `name` (string): Problem name (1-300 characters, required)
  - `submissionLink` (string): Valid LeetCode submission URL (required)
  - `code` (string): Source code (1-100,000 characters, required)
  - `language` (string): Programming language (required)

**Returns:** `Promise<void>`

**Throws:**
- `Error` - If validation fails or storage operation fails

**Example:**
```javascript
await addProblem({
  name: "Two Sum",
  submissionLink: "https://leetcode.com/problems/two-sum/submissions/123456/",
  code: "function twoSum(nums, target) {\n    // implementation\n}",
  language: "JavaScript"
});
```

### `updateProblem(id, updates)`

Updates an existing problem.

**Parameters:**
- `id` (string): Problem ID
- `updates` (Object): Fields to update (partial problem object)

**Returns:** `Promise<void>`

**Throws:**
- `Error` - If problem not found or update fails

### `deleteProblem(id)`

Deletes a problem from the problem set.

**Parameters:**
- `id` (string): Problem ID

**Returns:** `Promise<void>`

**Note:** Does not throw if problem not found (idempotent operation)

### `getAllProblems()`

Retrieves all problems in the current problem set.

**Returns:** `Promise<Array<Object>>` - Array of problems sorted by order

**Example:**
```javascript
const problems = await getAllProblems();
problems.forEach((problem, index) => {
  console.log(`${index + 1}. ${problem.name} (${problem.language})`);
});
```

### `reorderProblems(problemIds)`

Reorders problems based on array of problem IDs.

**Parameters:**
- `problemIds` (Array<string>): Array of problem IDs in desired order

**Returns:** `Promise<void>`

**Throws:**
- `Error` - If problemIds is invalid or reorder fails

### `clearAll()`

Clears all data (problem set info and all problems).

**Returns:** `Promise<void>`

## Content Script API

The Content Script API handles extraction of data from LeetCode pages.

### `extractProblemData()`

Extracts problem data from the current LeetCode submission page.

**Returns:** `Promise<Object>` - Extracted problem data
- `name` (string): Problem name (without number prefix)
- `submissionLink` (string): Full submission URL
- `code` (string): Cleaned source code
- `language` (string): Programming language

**Throws:**
- `Error` - If extraction fails with descriptive error message

**Example:**
```javascript
try {
  const data = await extractProblemData();
  console.log('Extracted:', data.name, data.language);
} catch (error) {
  console.error('Extraction failed:', error.message);
}
```

### `removeLineNumbers(code)`

Removes line numbers from code while preserving indentation.

**Parameters:**
- `code` (string): Code with potential line numbers

**Returns:** `string` - Cleaned code with preserved indentation

**Supported Formats:**
- `1 code` → `code`
- `1. code` → `code`
- `1|code` → `code`
- `1:code` → `code`
- `1code` → `code`
- Number-only lines are completely removed

### `detectLeetCodeSubmissionPage()`

Detects if the current page is a valid LeetCode submission page.

**Returns:** `boolean` - True if on a valid submission page

**Supported URLs:**
- `/problems/{slug}/submissions/{id}/`
- `/submissions/detail/{id}/`

## Document Generation API

The Document Generation API creates formatted .docx documents.

### `generateDocxDocument(documentData)`

Generates a formatted .docx document from problem set data.

**Parameters:**
- `documentData` (Object): Complete document data
  - `problemSetInfo` (Object): Problem set metadata
    - `title` (string): Problem set title
    - `submittedBy` (string): Student name
  - `problems` (Array<Object>): Array of problem objects

**Returns:** `Document` - docx Document instance ready for export

**Example:**
```javascript
const doc = generateDocxDocument({
  problemSetInfo: {
    title: "Problem Set 6",
    submittedBy: "John Doe"
  },
  problems: [
    {
      name: "Two Sum",
      submissionLink: "https://leetcode.com/...",
      code: "function twoSum() { ... }",
      language: "JavaScript"
    }
  ]
});

// Convert to blob and download
const blob = await docx.Packer.toBlob(doc);
```

### `generateFilename(problemSetInfo)`

Generates a sanitized filename for the document.

**Parameters:**
- `problemSetInfo` (Object): Problem set metadata

**Returns:** `string` - Sanitized filename with .docx extension

**Format:** `{Student Name} - {Problem Set Title}.docx`

## Background Script API

The Background Script API handles message routing and lifecycle management.

### Message Types

#### `PING`
Tests if content script is ready.

**Request:** `{ type: 'PING' }`
**Response:** `{ success: true, message: 'Content script is ready' }`

#### `EXTRACT_PROBLEM_DATA`
Requests problem data extraction from content script.

**Request:** `{ type: 'EXTRACT_PROBLEM_DATA' }`
**Response:** 
```javascript
{
  success: true,
  data: {
    name: "Problem Name",
    submissionLink: "https://...",
    code: "source code",
    language: "JavaScript"
  }
}
```

## Error Handling

### Error Types

#### Validation Errors
Thrown when input data doesn't meet requirements.

```javascript
// Example validation error
{
  name: "ValidationError",
  message: "Problem name must be between 1 and 300 characters"
}
```

#### Storage Errors
Thrown when Chrome storage operations fail.

```javascript
// Example storage error
{
  name: "StorageError", 
  message: "Failed to save data: Storage quota exceeded"
}
```

#### Extraction Errors
Thrown when content script cannot extract data.

```javascript
// Example extraction error
{
  name: "ExtractionError",
  message: "Could not find problem name on this page"
}
```

### Error Handling Best Practices

1. **Always use try-catch blocks** for async operations
2. **Provide specific error messages** to help users understand issues
3. **Log errors to console** for debugging
4. **Graceful degradation** when possible

```javascript
// Good error handling example
try {
  await addProblem(problemData);
  showStatus('Problem added successfully', 'success');
} catch (error) {
  console.error('Failed to add problem:', error);
  
  if (error.message.includes('validation')) {
    showStatus(`Validation error: ${error.message}`, 'error');
  } else if (error.message.includes('storage')) {
    showStatus('Storage error. Please try again.', 'error');
  } else {
    showStatus('Unexpected error occurred.', 'error');
  }
}
```

## Data Models

### Problem Object

```typescript
interface Problem {
  id: string;              // Unique identifier (timestamp)
  name: string;            // Problem name (1-300 chars)
  submissionLink: string;  // Valid LeetCode URL
  code: string;            // Source code (1-100,000 chars)
  language: string;        // Programming language
  capturedAt: number;      // Timestamp when captured
  order: number;           // Display order (0-based)
}
```

### Problem Set Info Object

```typescript
interface ProblemSetInfo {
  title: string;       // Problem set title (2-200 chars)
  submittedBy: string; // Student name (2-100 chars)
}
```

### Document Data Object

```typescript
interface DocumentData {
  problemSetInfo: ProblemSetInfo;
  problems: Problem[];
}
```

### Storage Structure

```typescript
interface StorageData {
  [STORAGE_KEY]: {
    info: ProblemSetInfo;
    problems: Problem[];
  }
}
```

## Rate Limits and Constraints

### Storage Limits
- **Chrome Local Storage**: ~5MB total
- **Individual Problem Code**: 100,000 characters max
- **Problem Set Title**: 200 characters max
- **Student Name**: 100 characters max
- **Problem Name**: 300 characters max

### Performance Considerations
- **DOM Queries**: Minimized and cached where possible
- **Storage Operations**: Batched when feasible
- **Memory Usage**: Temporary data cleaned up after operations
- **Network Requests**: Only for CDN resources (docx library)

## Version Compatibility

### Chrome Extension Manifest V3
- **Minimum Chrome Version**: 88
- **Service Worker**: Background script runs as service worker
- **Content Script Injection**: Automatic on matching URLs
- **Permissions**: `storage`, `activeTab`, LeetCode host permissions

### Browser Support
- ✅ Chrome 88+
- ✅ Chromium-based browsers (Edge, Brave, etc.)
- ❌ Firefox (different extension API)
- ❌ Safari (different extension API)