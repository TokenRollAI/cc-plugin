# llmdoc Structure Reference

## Directory Layout

```
llmdoc/
├── index.md              # START HERE - Navigation and overview
├── overview/             # "What is this project?"
│   └── project-overview.md
├── architecture/         # "How does it work?" (LLM Retrieval Map)
│   └── *.md
├── guides/               # "How do I do X?"
│   └── *.md
├── reference/            # "What are the specifics?"
│   └── *.md
└── agent/                # Temporary agent reports (auto-cleaned)
    └── *.md
```

## Category Purposes

| Category | Question Answered | Content Type |
|----------|------------------|--------------|
| `overview/` | "What is this project?" | High-level context, purpose, tech stack |
| `architecture/` | "How does it work?" | LLM retrieval maps, component relationships |
| `guides/` | "How do I do X?" | Step-by-step workflows (5-7 steps max) |
| `reference/` | "What are the specifics?" | Conventions, data models, API specs |

## Reading Priority

1. **Always** read `index.md` first
2. **Always** read ALL `overview/*.md` documents
3. Read relevant `architecture/` docs before modifying related code
4. Consult `guides/` for step-by-step workflows
5. Check `reference/` for conventions and specs

## Document Conventions

- **Brevity**: Under 150 lines per document
- **No Code Blocks**: Use `path/file.ext:line` references
- **Kebab-case**: File names like `project-overview.md`
- **LLM-First**: Write for machine consumption
