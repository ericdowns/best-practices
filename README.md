# WordPress Theme Best Practices

This repository contains a collection of individual best practice files for WordPress theme development. Each file documents specific patterns and conventions that can be referenced and implemented during project builds.

## Purpose

These best practices are designed to be:
- **Modular**: Each practice is documented in its own file
- **Reference-ready**: Easy to link to specific practices during development
- **Implementation-focused**: Clear code examples and patterns
- **Project-agnostic**: No specific project names or references

## How to Use

When working on a project, you can:

1. **Link to specific practices**: Reference individual files (e.g., `tailwind-images.md`)
2. **Copy patterns**: Use the code examples as starting points
3. **Follow conventions**: Implement the documented standards
4. **Share with team**: Send GitHub links to specific best practices

## Current Best Practices

- [`tailwind-images.md`](tailwind-images.md) - Image handling using pb-bottom method with absolute positioning

## Adding New Best Practices

Each new best practice should:
- Be documented in its own `.md` file
- Include clear code examples
- Provide do's and don'ts
- Be project-agnostic
- Focus on one specific pattern or convention

## File Naming Convention

Use descriptive, kebab-case filenames:
- `tailwind-[topic].md` for Tailwind-specific practices
- `wordpress-[topic].md` for WordPress-specific practices  
- `[technology]-[topic].md` for other technologies

## Quick Reference

Simply share this repo URL with Claude and specify which practice to implement:

> "Please implement the image handling pattern from the tailwind-images.md file in this repository"