# Security plan agents

This repository contains a spike to explore the use of AI agents for generating and maintaining security plans. The goal is to speed up the creation of security plans based on architecture and data flow diagrams, as well as to assist in the documentation process.

## Prompts

Couple of prompts are provided under `/.github/prompts` directory. These prompts are designed to be used in a agent experience usually within VSCode Copilot.

These prompts are heavily inspired by the Edge AI Accelerator project their use of Hyper velocity engineering.

### Prompts Usage

1. Open the prompt you want to use in the `/.github/prompts` directory.
2. Choose a model that has reasoning and planning capabilities, such as `gpt-4o` or `Claude Sonnet 4`.
3. When the prompt is in the context with a simple command like `Use the instructions and generate me the document. Ask clarifying questions where you see fit`.

### Available Prompts

There are following prompts available:

- `security-plan-create.prompt.md`: This prompt helps you create a security plan based on the architecture and data flow diagrams of your project.
  - This prompt looks into the `references` and `security_plan_context` directories for additional context and information.
  - Add markdown mermaid diagrams to the `security_plan_context` directory to provide visual context for the security plan.
- `security-plan-create-2.prompt.md`: This is an alternative version of the security plan creation prompt created by Collin Scott.
- `mermaid-diagram-creator.prompt.md`: This prompt analyzes images and creates corresponding Mermaid diagrams. To use this prompt, you have to add the image as additional context to the prompt.
- `prompt-new.prompt.md`: This prompt creates prompt/instruction files based on source code or user-provided files, analyzing coding standards and conventions.
- `prompt-refactor.prompt.md`: This prompt refactors existing prompt/instruction files to improve clarity, organization, and effectiveness while maintaining their original purpose.

## Improvements

- Create a prompt with an adversary role that can analyze the security plan and suggest improvements or identify potential vulnerabilities.
- Get threats and recommendations from a MCP Server to integrate it in a non static manner with the security plan agent.

## Contributing

If you have suggestions for improving the prompts, please feel free to open an issue or submit a pull request. Your contributions are welcome!
