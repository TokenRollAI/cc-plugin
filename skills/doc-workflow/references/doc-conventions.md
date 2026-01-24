# Documentation Conventions

## Code Reference Format

Always reference code instead of pasting:

```
`path/to/file.ext` (SymbolName): Brief description
```

**Example:**
```
`src/auth/jwt.js` (generateToken, verifyToken): Handles JWT creation and validation
```

## Document Quality Checklist

- [ ] **Brevity**: Under 150 lines?
- [ ] **Clarity**: Purpose clear from title and first lines?
- [ ] **Accuracy**: Based on verified code?
- [ ] **Categorization**: In correct category?
- [ ] **No Code Blocks**: Using references only?

## Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Files | kebab-case | `project-overview.md` |
| Categories | lowercase | `overview/`, `guides/` |
| Titles | Title Case | `# Project Overview` |

## Update Principles

1. **Minimality**: Use fewest words necessary
2. **Accuracy**: Based on actual code, not assumptions
3. **Consistency**: Match existing document styles
4. **Atomicity**: One concept per document
