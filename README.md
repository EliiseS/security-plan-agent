# Security plan agents

This repository contains a spike to explore the use of AI agents for generating and maintaining security plans. The goal is to speed up the creation of security plans based on architecture and sequence diagrams, as well as to assist in the documentation process.

The primary approach is through the [`security-plan-creation.chatmode.md`](./.github/chatmodes/security-plan-creation.chatmode.md), which assists with the creation of security plans.

## Chat Modes

The main functionality is provided through specialized chat modes, each designed for specific tasks in the security plan creation and prompt engineering workflow.

### Chat Mode Usage

1. Open GitHub Copilot Chat in VS Code
2. Select the desired chat mode from the modes dropdown
3. Follow the guided workflow specific to the selected chat mode
4. The chat modes will automatically validate prerequisites and guide you through the process

### Available Chat Modes

- [`security-plan-creation.chatmode.md`](./.github/chatmodes/security-plan-creation.chatmode.md): Expert security architect chat mode for creating comprehensive cloud security plans
- [`prompt-builder.chatmode.md`](./.github/chatmodes/prompt-builder.chatmode.md): Expert prompt engineering and validation system

## Prompts

Additional prompts are provided under `/.github/prompts` directory for specialized tasks that support the security plan creation process.

These prompts are heavily inspired by the Edge AI Accelerator project and their use of Hyper velocity engineering.

### Prompt Usage

1. Open the prompt you want to use in the `/.github/prompts` directory.
2. Choose a model that has reasoning and planning capabilities, such as `gpt-4o` or `Claude Sonnet 4`.
3. Provide the prompt with a simple command like `Use the instructions and generate me the document. Ask clarifying questions where you see fit`.

### Available Prompts

The following prompts are available to support the security plan creation workflow:

- [`mermaid-diagram-creator.prompt.md`](./.github/prompts/mermaid-diagram-creator.prompt.md): Analyzes images and creates corresponding Mermaid diagrams. To use this prompt, add the image as additional context to the prompt.
- [`prompt-new.prompt.md`](./.github/prompts/prompt-new.prompt.md): Creates prompt/instruction files based on source code or user-provided files, analyzing coding standards and conventions.
- [`prompt-refactor.prompt.md`](./.github/prompts/prompt-refactor.prompt.md): Refactors existing prompt/instruction files to improve clarity, organization, and effectiveness while maintaining their original purpose.

## Threat generation

Threats are generated using the [anmalkov/mcp-crisp](https://hub.docker.com/r/anmalkov/mcp-crisp) MCP server.

## Contributing

If you have suggestions for improving the chat mode or supporting prompts, please feel free to open an issue or submit a pull request. Your contributions are welcome!
