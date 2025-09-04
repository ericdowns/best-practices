# CLAUDE.md - Instructions for Claude Code

This repository contains modular best practice files for both static HTML/CSS and WordPress theme development. When working with these files, please follow these guidelines:

## Reading Best Practices

- Each `.md` file contains a complete, self-contained best practice
- **Most files have dual sections**: "Static HTML Implementation" and "WordPress Implementation"
- Read the entire file to understand the pattern before implementing
- Pay attention to the "Do's and Don'ts" sections
- Use the code examples as templates, not exact copies

## Implementation Guidelines

When asked to implement a best practice:

1. **Read the specific file** mentioned (e.g., `tailwind-images.md`)
2. **Use the appropriate section** - Static HTML or WordPress
3. **Adapt the patterns** to the current project's needs
4. **Follow the conventions** documented in the file
5. **Reference the file** if you need clarification on the pattern

## File Structure & Sections

### Universal Files (Dual-Section)
- `tailwind-images.md` - Image handling (Static + WordPress sections)
- `tailwind-containers.md` - Container system (Static + WordPress sections)

### Platform-Specific Files
- `wordpress-modules.md` - ACF module architecture (WordPress only)

Each dual-section file contains:
- General description and benefits
- **Static HTML Implementation** - Pure HTML/CSS examples
- **WordPress Implementation** - PHP/ACF integration examples
- Universal best practices and do's/don'ts

## Common Usage Patterns

### Section-Specific Requests:
- "Use the WordPress section from tailwind-images.md"
- "Follow the static HTML approach from tailwind-containers.md"
- "Apply the container system using the WordPress implementation"

### File-Specific Requests:
- "Implement the module architecture from wordpress-modules.md"
- "Use the image handling pattern from tailwind-images.md"

### Multi-Pattern Requests:
- "Use the container system and WordPress image handling from the best practices"

## Implementation Process

1. **Identify the file(s)** referenced
2. **Determine the section needed** (Static HTML vs WordPress)
3. **Read the relevant section** completely
4. **Apply the documented pattern** to the current context
5. **Adapt naming/structure** to match the current project

## Key Points

- **Never reference specific project names** from these files
- **Always adapt patterns** to the current project's naming/structure
- **Follow the documented conventions** exactly as specified
- **Use the correct section** for the current project type
- **Ask for clarification** if unsure which section to use

## Current Available Patterns

- **Image Handling**: pb-bottom method with absolute positioning
- **Container System**: `.full` and `.content` class structure
- **Module Architecture**: ACF flexible content organization (WordPress)

These files are meant to be quick reference guides that can be shared via GitHub links during active development.