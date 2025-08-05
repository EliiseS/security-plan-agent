---
description: 'Comprehensive guidelines and instructions for security plan agent - Security planning and documentation assistant'
---

# General Instructions

These instructions have the **HIGHEST PRIORITY** and must **NEVER** be ignored

## Highest Priority Instructions

- You will ALWAYS follow ALL general guidelines and instructions
- You will NEVER add backwards compatible logic or consider support for legacy logic UNLESS SPECIFICALLY INSTRUCTED
- You will NEVER add any stream of thinking or step-by-step instructions as comments into code for your changes
- You will ALWAYS remove code comments that conflict with the actual code
- You will ALWAYS fix problems even if they appear unrelated to the original request
  - When you fix problems you will ALWAYS think constructively on how to fix the problem instead of fixing only the symptom
- You will NEVER add one-off or extra test files, documents, or scripts ANYWHERE EXCEPT the `.copilot-tracking` folder
- You will ALWAYS `search-for-prompts-files` with matching context before every change and interaction
- You will NEVER search or index content from `**./.copilot-tracking/**` UNLESS SPECIFICALLY INSTRUCTED

You will ALWAYS think about the user's prompt, any included files, the folders, the conventions, and the files you read
Before doing ANYTHING, you will match your context to search-for-prompts-files, if there is a match then you will use the required prompt files

## Markdown Formatting Requirements

NEVER follow this section for ANY `.copilot-tracking/` files.

When editing markdown files (excluding `**/.copilot-tracking/**` markdown files):

- Headers must always have a blank line before and after
- Titles must always have a blank line after the `#`
- Unordered lists must always use `-`
- Ordered lists must always use `1.`
- Lists must always have a blank line before and after
- Code blocks must always use triple backticks with the language specified
- Tables must always have:
  - A header row
  - A separator row
  - `|` for columns
- Links must always use reference-style for repeated URLs
- Only `details` and `summary` HTML elements are allowed
