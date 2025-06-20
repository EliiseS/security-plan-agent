---
mode: 'agent'
description: 'Creates comprehensive security plans'
---
 Copilot Instructions: Use these guidelines to help complete the security plan template for Visual Studio projects.
  These instructions are intended for Copilot or other LLMs to accurately create or update a comprehensive security plan using the provided template.

1. Review the Latest Template:
   - Retrieve the latest version of the [Security Plan Template](../../references/security_plan_template.md).
   - Review the current project context to determine if an existing security plan needs updating or if a new one should be created.
   - Prompt the user if there is ambiguity about whether to create a new plan or update an existing one.

2. Project and Title Update:
   - Prompt the user for a title if necessary.
   - Replace every instance of "Project Name" with the actual project title.

3. Diagrams Section:
   - **Architecture Diagrams:**
     - Prompt the user to provide architecture images or a compatible diagram file.
     - If images are available, request uploads or sharing of links; if not, leave placeholders for future insertion.
   - **Data Flow Diagrams:**
     - Always generate the data flow diagram in Mermaid format.
     - Ensure the diagram is oriented left-to-right.
     - Number each edge sequentially, starting from the leftmost (closest to the user) and incrementing as flows move right.
     - Each edge in the diagram must correspond to a row in the Data Flow Attributes table.
     - Include all relevant services (e.g., Private Endpoints, monitoring, storage, databases) as nodes.
     - Use clear, descriptive node names matching the architecture.
     - For local images, store them in an assets folder (e.g., /assets) and update markdown image links accordingly.
     - Confirm that image paths are valid in both GitHub and local environments.

4. Data Flow Attributes Table:
   - For every numbered edge in the data flow diagram, create a corresponding row in the table.
   - Specify the transport protocol used (e.g., HTTPS, TCP).
   - Define data classification according to organizational guidelines.
   - Document the authentication method (e.g., Active Directory, OAuth), and authorization approach (e.g., role-based access).
   - Use the "Notes" column for any additional specifics.
   - Cross-check all entries with the provided Data Classification Guidance.

5. Threats and Mitigations:
   - Identify key security threats (e.g., injection attacks, XSS, data breaches).
   - List detailed mitigation strategies for each threat, referencing tools such as SonarQube or static analysis tools available in Visual Studio or Azure DevOps.
   - Follow the principles outlined in Microsoft Threat Modeling Security Fundamentals.

6. Secrets Inventory:
   - Use TODO markers if there is insufficient information to determine the actual values.
   - For any secrets (e.g., API keys, connection strings), document:
     • Name, purpose, and location (e.g., Azure Key Vault, environment variables).
     • Generation method, rotation strategy, potential downtime impacts, distribution method, and lifespan.
   - Refer to the Example Secrets Inventory for further guidance.

7. Integration with Visual Studio Code & Development Workflow:
   - Utilize VS Code TODO/FIXME comments to flag incomplete sections.
   - Consider integrating security validation tasks via tasks.json to check for common vulnerabilities.
   - Ensure this document remains synchronized with code changes, updating it during pull requests or sprint reviews.

8. Final Review and Continuous Improvement:
   - Confirm that the document meets internal standards and customer requirements.
   - Update the relevant sections immediately when changes occur in the architecture or codebase.
   - Do not include copilot instructions in the final document.
-->
