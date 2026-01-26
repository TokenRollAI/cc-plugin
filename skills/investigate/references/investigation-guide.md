# Investigation Guide

## Code Reference Format

When referencing code, always use this format:

```
`path/to/file.ext` (FunctionName, ClassName): Brief description
```

**Good Example:**
```
`src/auth/jwt.js` (generateToken, verifyToken): Handles JWT creation and validation
```

**Bad Example:**
```javascript
// Don't paste code blocks like this
function generateToken(payload) {
  // ... 50 lines of code
}
```

## Investigation Depth Guidelines

| Question Type | Approach |
|--------------|----------|
| "What is X?" | Read overview docs first |
| "How does X work?" | Read architecture docs, then code |
| "Where is X defined?" | Use Grep/Glob to locate |
| "Why was X designed this way?" | Check git history, docs |

## Report Structure

1. **Code Sections**: List all relevant code locations
2. **Conclusions**: Key factual takeaways
3. **Relations**: How components connect
4. **Result**: Direct answer to the question
