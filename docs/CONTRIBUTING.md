# Contributing Guidelines

Thank you for your interest in contributing to the LeetCode Documentation Generator! This document provides guidelines and information for contributors.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Guidelines](#issue-guidelines)
- [Documentation Standards](#documentation-standards)

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors, regardless of background, experience level, or identity.

### Expected Behavior

- **Be Respectful**: Treat all community members with respect and kindness
- **Be Collaborative**: Work together constructively and help others learn
- **Be Patient**: Remember that everyone has different experience levels
- **Be Constructive**: Provide helpful feedback and suggestions

### Unacceptable Behavior

- Harassment, discrimination, or offensive language
- Personal attacks or trolling
- Publishing private information without permission
- Any behavior that would be inappropriate in a professional setting

## Getting Started

### Prerequisites

Before contributing, ensure you have:

- **Chrome Browser** (version 88+)
- **Git** for version control
- **Text Editor** (VS Code, Sublime Text, etc.)
- **Basic Knowledge** of JavaScript, HTML, CSS
- **Understanding** of Chrome Extension development (helpful but not required)

### Development Setup

1. **Fork the Repository**
   ```bash
   # Fork on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/leetcode-doc-generator.git
   cd leetcode-doc-generator
   ```

2. **Set Up Upstream Remote**
   ```bash
   git remote add upstream https://github.com/prithvi1236/leetcode-doc-generator.git
   ```

3. **Load Extension in Chrome**
   - Open `chrome://extensions/`
   - Enable "Developer mode"
   - Click "Load unpacked"
   - Select the project directory

4. **Verify Setup**
   - Extension should appear in Chrome toolbar
   - Test basic functionality on a LeetCode submission page

## Development Workflow

### Branch Strategy

We use a **feature branch workflow**:

1. **Main Branch**: `main` - Production-ready code
2. **Feature Branches**: `feature/description` - New features
3. **Bug Fix Branches**: `bugfix/description` - Bug fixes
4. **Hotfix Branches**: `hotfix/description` - Critical fixes

### Creating a Feature Branch

```bash
# Update main branch
git checkout main
git pull upstream main

# Create feature branch
git checkout -b feature/your-feature-name

# Make your changes
# ... development work ...

# Commit changes
git add .
git commit -m "Add: descriptive commit message"

# Push to your fork
git push origin feature/your-feature-name
```

### Commit Message Guidelines

Follow the **Conventional Commits** specification:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```bash
feat(popup): add drag and drop reordering
fix(content): handle missing code blocks gracefully
docs(api): update storage API documentation
style(popup): improve button spacing and colors
refactor(storage): simplify validation logic
test(content): add extraction edge case tests
chore(deps): update docx library to latest version
```

## Coding Standards

### JavaScript Style Guide

#### General Principles
- **Readability**: Code should be self-documenting
- **Consistency**: Follow existing code patterns
- **Simplicity**: Prefer simple, clear solutions
- **Performance**: Consider performance implications

#### Code Style

```javascript
// ✅ Good: Use const/let, descriptive names
const problemData = await extractProblemData();
const isValidSubmission = validateSubmissionUrl(url);

// ❌ Bad: Use var, unclear names
var data = await extract();
var valid = validate(url);

// ✅ Good: Proper function documentation
/**
 * Extracts problem data from LeetCode submission page
 * @param {string} url - Submission page URL
 * @returns {Promise<Object>} Extracted problem data
 * @throws {Error} If extraction fails
 */
async function extractProblemData(url) {
  // Implementation
}

// ✅ Good: Error handling
try {
  const result = await riskyOperation();
  return result;
} catch (error) {
  console.error('Operation failed:', error);
  throw new Error(`Failed to complete operation: ${error.message}`);
}

// ✅ Good: Async/await over Promises
async function saveData(data) {
  try {
    await chrome.storage.local.set(data);
    console.log('Data saved successfully');
  } catch (error) {
    throw new Error(`Save failed: ${error.message}`);
  }
}
```

#### Naming Conventions

- **Variables**: `camelCase` - `problemData`, `isValid`
- **Functions**: `camelCase` - `extractProblemData()`, `validateInput()`
- **Constants**: `UPPER_SNAKE_CASE` - `STORAGE_KEY`, `MAX_CODE_LENGTH`
- **Classes**: `PascalCase` - `DocumentGenerator`, `StorageManager`
- **Files**: `camelCase` - `popup.js`, `docxGenerator.js`

### HTML/CSS Guidelines

#### HTML Best Practices
```html
<!-- ✅ Good: Semantic HTML -->
<section class="problem-management">
  <header class="section-header">
    <h2>Problems</h2>
  </header>
  <main class="problems-list" role="main">
    <!-- Content -->
  </main>
</section>

<!-- ✅ Good: Accessibility -->
<button 
  id="captureButton" 
  class="primary-button"
  aria-label="Capture problem from current page"
  type="button">
  <i class="fas fa-camera" aria-hidden="true"></i>
  Capture from Current Page
</button>
```

#### CSS Best Practices
```css
/* ✅ Good: BEM methodology */
.problem-card {
  /* Block */
}

.problem-card__header {
  /* Element */
}

.problem-card--dragging {
  /* Modifier */
}

/* ✅ Good: Logical property grouping */
.button {
  /* Positioning */
  position: relative;
  
  /* Box model */
  display: inline-block;
  padding: 8px 16px;
  margin: 4px;
  
  /* Typography */
  font-size: 14px;
  font-weight: 500;
  
  /* Visual */
  background: #007bff;
  border: none;
  border-radius: 4px;
  
  /* Misc */
  cursor: pointer;
  transition: background-color 0.2s ease;
}
```

### Documentation Standards

#### JSDoc Comments
All functions should have JSDoc comments:

```javascript
/**
 * Validates problem data before saving
 * @param {Object} problem - Problem data to validate
 * @param {string} problem.name - Problem name (1-300 chars)
 * @param {string} problem.code - Source code (1-100k chars)
 * @param {string} problem.language - Programming language
 * @returns {Object} Validation result {valid: boolean, error: string|null}
 * @throws {TypeError} If problem is not an object
 * @example
 * const result = validateProblemData({
 *   name: "Two Sum",
 *   code: "function twoSum() {...}",
 *   language: "JavaScript"
 * });
 * if (!result.valid) {
 *   console.error(result.error);
 * }
 */
function validateProblemData(problem) {
  // Implementation
}
```

#### Code Comments
```javascript
// ✅ Good: Explain why, not what
// Retry extraction after page refresh because content script may not be ready
const maxRetries = 3;
for (let attempt = 1; attempt <= maxRetries; attempt++) {
  // Implementation
}

// ✅ Good: Complex logic explanation
// Extract original indentation by analyzing spaces after line number
// Line format: "1     code" -> spaces after "1" contain original indentation
const originalIndentation = spacesAfterNumber.length > 1 
  ? spacesAfterNumber.substring(1) 
  : '';

// ❌ Bad: Obvious comments
// Increment counter
counter++;

// Set button text
button.textContent = 'Click me';
```

## Testing Guidelines

### Manual Testing Checklist

Before submitting a PR, test the following:

#### Core Functionality
- [ ] Extension loads without console errors
- [ ] Popup opens and displays correctly
- [ ] Problem set info saves and persists across sessions
- [ ] Code extraction works on different LeetCode submission pages
- [ ] Document generation produces valid .docx files
- [ ] Keyboard shortcut (Ctrl+Shift+K) works

#### Edge Cases
- [ ] Handle pages without code blocks
- [ ] Handle malformed or invalid submission URLs
- [ ] Handle very large code submissions (near 100k limit)
- [ ] Handle special characters in problem names and code
- [ ] Handle network connectivity issues
- [ ] Handle storage quota exceeded scenarios

#### Browser Compatibility
- [ ] Chrome latest version
- [ ] Chrome version 88 (minimum supported)
- [ ] Different screen resolutions (1920x1080, 1366x768, etc.)
- [ ] Different zoom levels (100%, 125%, 150%)

#### User Experience
- [ ] All buttons provide appropriate feedback
- [ ] Loading states are clear and informative
- [ ] Error messages are helpful and actionable
- [ ] Drag and drop works smoothly
- [ ] Auto-refresh functionality works correctly

### Automated Testing (Future)

When we implement automated testing, follow these guidelines:

```javascript
// Unit test example
describe('validateProblemData', () => {
  it('should return valid for correct problem data', () => {
    const problem = {
      name: 'Two Sum',
      code: 'function twoSum() {}',
      language: 'JavaScript',
      submissionLink: 'https://leetcode.com/problems/two-sum/submissions/123/'
    };
    
    const result = validateProblemData(problem);
    
    expect(result.valid).toBe(true);
    expect(result.error).toBeNull();
  });
  
  it('should return invalid for missing name', () => {
    const problem = {
      code: 'function twoSum() {}',
      language: 'JavaScript'
    };
    
    const result = validateProblemData(problem);
    
    expect(result.valid).toBe(false);
    expect(result.error).toContain('name is required');
  });
});
```

## Pull Request Process

### Before Submitting

1. **Test Thoroughly**: Complete the manual testing checklist
2. **Update Documentation**: Update relevant documentation
3. **Check Code Style**: Ensure code follows style guidelines
4. **Commit Messages**: Use conventional commit format
5. **Rebase if Needed**: Rebase on latest main if behind

### PR Template

When creating a PR, use this template:

```markdown
## Description
Brief description of changes made.

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Manual testing completed
- [ ] All edge cases tested
- [ ] Cross-browser testing completed
- [ ] Performance impact assessed

## Screenshots (if applicable)
Add screenshots to help explain your changes.

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No console errors or warnings
- [ ] Tested on multiple LeetCode submission pages
```

### Review Process

1. **Automated Checks**: Basic checks (if implemented)
2. **Code Review**: Maintainer reviews code quality and functionality
3. **Testing**: Reviewer tests changes manually
4. **Feedback**: Address any feedback or requested changes
5. **Approval**: PR approved and merged

### After Merge

1. **Delete Branch**: Delete feature branch after merge
2. **Update Local**: Pull latest changes to local main
3. **Release Notes**: Changes included in next release notes

## Issue Guidelines

### Bug Reports

Use this template for bug reports:

```markdown
## Bug Description
Clear description of the bug.

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. See error

## Expected Behavior
What you expected to happen.

## Actual Behavior
What actually happened.

## Environment
- Chrome Version: [e.g., 91.0.4472.124]
- Extension Version: [e.g., 1.0.0]
- Operating System: [e.g., Windows 10, macOS 11.4]
- LeetCode Page URL: [if applicable]

## Screenshots
Add screenshots if helpful.

## Console Logs
Include any relevant console error messages.
```

### Feature Requests

Use this template for feature requests:

```markdown
## Feature Description
Clear description of the proposed feature.

## Problem Statement
What problem does this feature solve?

## Proposed Solution
Describe your proposed solution.

## Alternative Solutions
Describe alternatives you've considered.

## Additional Context
Any other context, mockups, or examples.
```

### Issue Labels

We use these labels to categorize issues:

- `bug` - Something isn't working
- `enhancement` - New feature or request
- `documentation` - Improvements or additions to documentation
- `good first issue` - Good for newcomers
- `help wanted` - Extra attention is needed
- `question` - Further information is requested
- `wontfix` - This will not be worked on

## Documentation Standards

### README Updates

When adding features, update the README:

1. **Features Section**: Add new features to the list
2. **Usage Section**: Update usage instructions if needed
3. **API Documentation**: Update if APIs change
4. **Screenshots**: Update screenshots if UI changes

### Code Documentation

1. **JSDoc Comments**: All public functions must have JSDoc
2. **Inline Comments**: Explain complex logic
3. **Architecture Docs**: Update if architecture changes
4. **API Docs**: Update if APIs change

### Commit Documentation

Include documentation updates in the same commit as code changes when possible.

## Getting Help

### Resources

- **GitHub Issues**: Ask questions or report problems
- **Code Review**: Learn from feedback on your PRs
- **Documentation**: Read existing docs thoroughly
- **Chrome Extension Docs**: [Official Chrome Extension documentation](https://developer.chrome.com/docs/extensions/)

### Communication

- **Be Specific**: Provide detailed information about issues
- **Be Patient**: Maintainers are volunteers with limited time
- **Be Helpful**: Help others when you can
- **Be Respectful**: Follow the code of conduct

## Recognition

Contributors will be recognized in:

- **README**: Contributors section
- **Release Notes**: Major contributions mentioned
- **GitHub**: Contributor statistics and graphs

Thank you for contributing to the LeetCode Documentation Generator! Your contributions help make this tool better for students worldwide.