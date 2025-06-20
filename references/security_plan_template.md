# Security Plan – Project Name

## Preamble

*Important to note:* <name_of_the_organization> cannot certify/attest to the security of an architecture nor code. This document is intended to help produce backlog items specific to the customer engagement and to document the relevant security design decisions made by the team during build. Please direct your customer to work with their account team or preferred security vendor to seek an audit or pen-test from a security vendor if required/desired.

## Overview

Please find the Security Plan for the Project Name below. This document shows the architecture and data flow diagram of the application. These artifacts were constructed based on documentation and source code from the project itself and are subject to change as the architecture and codebase evolves. Each of the labeled entities in the figures below are accompanied by meta-information which describes the threats, describes the data in scope, and recommendations for security controls.

## Diagrams

### Architecture Diagram

`<insert image here>`

### Data Flow Diagram

`<insert image here>`

### Data Flow Attributes
  
| #   | Transport Protocol                     | Data Classification                                                                                                                                                                                                                                | Authentication                        | Authorization                      | Notes              |
| --- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | ---------------------------------- | ------------------ |
| 1   | [Name of the protocol for the service] | [Data classification guidance](https://eng.ms/docs/microsoft-customer-partner-solutions-mcaps/industry-and-partner-sales/industry-solutions-engineering-ise/industry-solutions-engineering-ise/centraloperations/security/securityplanguidelines)] | [Method of authenticating the caller] | [Method of authorizing the caller] | [Additional Notes] |
| ... | ...                                    | ...                                                                                                                                                                                                                                                | ...                                   | ...                                | ...                |

## Threats and Mitigations

`<insert notable threats and mitigations here however you like>`


## Secrets Inventory

An ideal architecture would contain *zero secrets*. Credential-less options like managed identities should be used wherever possible. Where secrets are required, it’s important to track them for operational purposes. Please see our [Example Secrets Inventory]([https://eng.ms/docs/microsoft-customer-partner-solutions-mcaps/industry-and-partner-sales/industry-solutions-engineering-ise/industry-solutions-engineering-ise/centraloperations/security/securityplanguidelines#example-secrets-inventory) to help you get started.

| Name | What is its purpose? | Where does it live? | How was it generated? | What's the rotation strategy? Does it cause downtime? | How does the secret get distributed to consumers? | What’s the secret’s lifespan? |
| ---- | -------------------- | ------------------- | --------------------- | ----------------------------------------------------- | ------------------------------------------------- | ----------------------------- |
| ...  | ...                  | ...                 | ...                   | ...                                                   | ...                                               | ...                           |
