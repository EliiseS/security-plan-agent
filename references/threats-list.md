# Threats and Recommendations for Security Plan

|   #   | Threat                                                           | Status | Risk |
| :---: | :--------------------------------------------------------------- | :----- | :--- |
|   1   | [Code vulnerability and secret scanning](#threat-1)              |        |      |
|   2   | [CI CD pipelines might contain sensitive information](#threat-2) |        |      |
|   3   | [Web applications](#threat-3)                                    |        |      |
|   4   | [Multitenant applications](#threat-4)                            |        |      |
|   5   | [Container images may contain vulnerabilities](#threat-5)        |        |      |
|   6   | [Compromising publicly exposed services](#threat-6)              |        |      |
|   7   | [Subversion of authorization controls](#threat-7)                |        |      |
|   8   | [Broken or non-existent authentication](#threat-8)               |        |      |
|   9   | [Man in the middle](#threat-9)                                   |        |      |
|  10   | [Insufficient logging and monitoring](#threat-10)                |        |      |
|  11   | [Sending malicious or unnecessary traffic](#threat-11)           |        |      |
|  12   | [Distributed denial of service](#threat-12)                      |        |      |
|  13   | [Software supply chain integrity compromises](#threat-13)        |        |      |
|  14   | [Unencrypted internal network traffic](#threat-14)               |        |      |
|  15   | [Secrets leaking](#threat-15)                                    |        |      |
|  16   | [Insecure output handling](#threat-16)                           |        |      |
|  17   | [Model theft](#threat-17)                                        |        |      |
|  18   | [Training data poisoning](#threat-18)                            |        |      |
|  19   | [Overreliance](#threat-19)                                       |        |      |
|  20   | [Model denial of service](#threat-20)                            |        |      |
|  21   | [Information disclosure](#threat-21)                             |        |      |
|  22   | [Excessive agency](#threat-22)                                   |        |      |
|  23   | [Prompt injection](#threat-23)                                   |        |      |
|  24   | [Supply chain vulnerabilities](#threat-24)                       |        |      |
|  25   | [Malicious files](#threat-25)                                    |        |      |
|  26   | [Direct attack on data store](#threat-26)                        |        |      |
|  27   | [Deletion of user profile and personal data](#threat-27)         |        |      |

## Threat #1

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Code  
**Threat:** Code repositories may contain vulnerabilities and expose sensitive information.

**Mitigation:**

Implement CI/CD pipeline security scanning.

1. Integrate static code analysis tools in CI/CD pipelines to gain security insights about code under development, prevent insecure code from entering production, and generate more secure and complete solutions.
2. Implement dependency and supply chain scanning in CI/CD pipelines to ensure the most current and secure libraries are utilized.
3. Deploy credential scanning in CI/CD pipelines to prevent secrets, key information, and passwords from being exposed.

---

## Threat #2

**Principle:** Confidentiality and Integrity  
**Affected Asset:** CI/CD Pipelines  
**Threat:** CI/CD Pipelines should always be cleansed of sensitive information being leaked.

**Mitigation:**

All secrets, key materials, and credentials stored in CI/CD pipelines must be protected from exposure in plaintext format.

1. Implement pre-commit checks to ensure secure pipelines free of sensitive artifacts.

---

## Threat #3

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Web Application  
**Threat:** Attackers might exploit vulnerabilities in the web application by intercepting and manipulating communication between client and server, or exploiting resources improperly loaded from third party domains.

**Mitigation:**

Implement HTTP Strict Transport Security (HSTS), Content Security Policy (CSP) and X-Content-Type-Option headers to enhance the security of web applications and mitigate risks associated with MIME type confusion attacks.

1. Utilize HSTS:
   - Enforce HTTPS: Add HSTS header to ensure that all communications between the client and server are securely transmitted via HTTPS, reducing the risk of Man-in-the-Middle (MitM) attacks.
   - Set an adequate max-age: Define a sufficient max-age duration to ensure that the HSTS policy is cached for an optimal period.
   - Include subdomains: Ensure that the HSTS policy is applied to all subdomains by using the `includeSubDomains` directive to protect traffic on all subdomains.

2. Implement CSP:
   - Define allowed sources: Strictly specify which sources (domains) are legitimate for loading content, mitigating the risk of content injection attacks like Cross Site Scripting (XSS).
   - Limit functionality: Specify the types of resources that are allowed to be loaded and restrict the usage of inline scripting and stylesheet execution.
   - Define Embedding Limits: Utilize the frame-ancestors directive in CSP to precisely determine which domains can embed your application, enhancing protection against potential clickjacking attacks.
   - Report violations: Use the `report-uri` or `report-to` directive to specify a location to send reports when policy violations occur, aiding in the early detection of possible breaches or misconfigurations.

3. Configure X-Content-Type-Options:
   - Prevent MIME Sniffing: Set the X-Content-Type-Options header to `nosniff` to instruct browsers to adhere strictly to the declared content type, preventing MIME type confusion.

4. Monitoring and Alerting:
   - Implement monitoring and alerting to detect and notify in case of violations or attempts to bypass the security headers.
   - Regularly audit and adjust policies to stay abreast with any changes in application structure or added third-party services.

---

## Threat #4

**Principle:** Confidentiality, Integrity and Avaliability  
**Affected Asset:** Data Storage and Processing  
**Threat:** Malicious or compromised tenants might exploit vulnerabilities to access, modify, or disrupt data and services of other tenants in a multi-tenancy environment, leading to potential data leaks, unauthorized data manipulation, and service interruptions.

**Mitigation:**

Ensure strict data and operation isolation among tenants.

1. Data Isolation and Security:
   - Implement "Tenant-Level Data Isolation" by ensuring that each tenant's data is stored separately, either through distinct storage containers or using schema separations, and enabling robust access controls and data encryption.
   - Apply "Row-Level Security (RLS)" and "Data Masking" within databases to restrict data visibility and modification strictly to authorized tenant users and processes.

2. API Security Specific to Tenant Isolation:
   - Implement "API Tenant Context Checks", ensuring that all API calls are validated for the requesting tenant's context and preventing any cross-tenant data access or operations.
   - Utilize "OAuth Scopes and Claims" to restrict API access and operations strictly within the tenant boundary by validating tenant-specific tokens and claims.

3. Service and Resource Isolation:
   - Adopt "Tenant-Aware Application Design", ensuring that services, especially those dealing with data processing and handling, are aware of tenant boundaries and are incapable of cross-tenant operations.
   - Implement "Resource Throttling" per tenant to ensure that any spikes in resource usage (intentional or otherwise) by a tenant does not impact the availability or performance of services for other tenants.

4. Tenant Activity Monitoring and Anomaly Detection:
   - Develop "Tenant-Specific Monitoring" by tagging logs and metrics with tenant identifiers to observe individual tenant activities, ensuring abnormal activities are easily traceable back to specific tenants.
   - Implement "Anomaly Detection" for identifying unusual activities or resource usage patterns at the tenant level, alerting administrators, and optionally triggering automated containment or mitigation actions.

5. Tenant-Specific Backup and Restore:
   - Establish "Isolated Backup Strategies", ensuring that backup data from different tenants is strictly isolated and cannot be cross-restored, maintaining data integrity and confidentiality even in disaster recovery scenarios.
   - Formulate and Test "Tenant-Specific Restore Procedures" to ensure that, during restoration processes, there are no scenarios under which data could be restored to an incorrect tenant context.

---

## Threat #5

**Principle:** Integrity  
**Affected Asset:** All services  
**Threat:** Container images may contain unknown vulnerabilities, security issues and malicious applications.

**Mitigation:**

Only run containers from trusted registries. When using ACR, enable Azure Defender to scan your registry for security vulnerabilities on each pushed container image and expose detailed findings per image.

1. Use container scanning tools within your CI/CD pipelines to detect vulnerabilities earlier.
2. Use Microsoft Defender for Containers to secure clusters, containers, and applications. Also scan your images for vulnerabilities with Microsoft Defender or any other image scanning solution.

---

## Threat #6

**Principle:** Confidentiality and Integrity  
**Affected Asset:** All services  
**Threat:** A large attack surface, particularly those that are exposed on the internet, will increase the probability of a compromise.

**Mitigation:**

Minimize the application attack surface by limiting publicly exposed services.

1. Use strong network controls by using Azure Virtual Networks, Network Security Groups (NSG), or Private Endpoints to protect against unsolicited traffic.
2. Use Azure Private Endpoints to block all internet connections to services that do not need to be publicly exposed.
3. Turning on Azure Defender will provide a list of vulnerabilities associated with your subscription, it is advised to turn on this feature (free 30-day trial) to obtain a list of security measures and mitigate through that list.

---

## Threat #7

**Principle:** Confidentiality  
**Affected Asset:** All services  
**Threat:** Authorization controls could be subverted to allow an authenticated user/service to access unauthorized information. Worse, this weakness could be used to elevate privileges.

**Mitigation:**

Audit identities and their roles to determine least privileges.

1. Use Azure RBAC on services to segregate duties and grant only the least amount of access to perform an action at a particular scope.
2. Use Azure services role-based authorization feature.
3. Grant the appropriate permissions and roles to service principals.

---

## Threat #8

**Principle:** Confidentiality  
**Affected Asset:** All services  
**Threat:** Broken or non-existent authentication mechanisms may allow attackers to gain access to confidential information.

**Mitigation:**

All services within the Azure Trust Boundary must authenticate all incoming requests, including requests coming from the same network. Proper authorizations should also be applied to prevent unnecessary privileges.

1. Use Entra ID authentication for centralized identity management.
2. Whenever available, use Azure Managed Identities to authenticate services. Service Principals may be used if Managed Identities are not supported.
3. External users or services may use Username + Passwords, Tokens, or Certificates to authenticate, provided these are stored on Key Vault or any other vaulting solution.
4. For authorization, use Azure RBAC to segregate duties and grant only the least amount of access to perform an action at a particular scope.
5. Leverage AAD PIM for any administrative access.
6. Avoid storing secrets in databases or configuration files.

---

## Threat #9

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Inbound Internet connections  
**Threat:** As a result of the vulnerability of not encrypting data, plaintext data could be intercepted during transit via a man-in-the-middle (MitM), downgrade or cross-protocol attacks attack. Sensitive data could be exposed or tampered to allow further exploits.

**Mitigation:**

All products and services must encrypt data in transit using approved cryptographic protocols and algorithms.

1. Use TLS to encrypt all HTTP-based network traffic. Use other mechanisms, such as IPSec, to encrypt non-HTTP network traffic that contains customer or confidential data.
2. Use only TLS 1.2 or TLS 1.3. Use ECDHE-based ciphers suites and NIST curves. Use strong keys. Enable HTTP Strict Transport Security (HSTS). Turn off TLS compression and do not use ticket-based session resumption.

---

## Threat #10

**Principle:** Integrity  
**Affected Asset:** All services  
**Threat:** Exploitation of insufficient logging and monitoring is the bedrock of nearly every major incident.
Attackers rely on the lack of monitoring and timely response to achieve their goals without being detected.

**Mitigation:**

Logging of critical application events must be performed to ensure that, should a security incident occur, incident response and root-cause analysis may be done. Steps must also be taken to ensure that logs are available and cannot be overwritten or destroyed through malicious or accidental occurrences.

At a minimum, the following events should be logged.

1. Login/logout events
2. Password change events (if not integrated into SSO)
3. Privilege delegation events
4. Security validation failures (e.g., input validation or authorization check failures)
5. Application errors and system events
6. Application and system startups and shutdowns, as well as logging initialization

- Implement a monitoring solution such as Azure Monitor or Log Analytics to monitor the web applications, security events, analytics, and workloads, respectively.
- If incident management is a concern of the customer, implement Microsoft Sentinel to act as the SIEM and make incident response more efficient.
- Use an alerting system to provide notifications when things need direct action.
- Install an Azure Monitor agent on critical VMs to allow ingestion of events into Azure Monitor.

---

## Threat #11

**Principle:** Integrity  
**Affected Asset:** All services  
**Threat:** Sending malicious or unnecessary traffic to Azure resources can lead to a compromise of that resource or create a vector for the attacker to bypass other security controls.

**Mitigation:**

Use and configure firewalls to protect against malicious incoming and outgoing network traffic.

1. Use Application Gateway before publicly accessible Azure Services and enable the Web App Firewall (WAF). By default, the WAF will use Azure-managed rules such as the OWASP ruleset. Custom rules should be defined if the default rules do not meet the customer’s needs.
2. Turn on DDoS protection feature.

---

## Threat #12

**Principle:** Availability  
**Affected Asset:** All services  
**Threat:** Distributed denial of service (DDoS) attacks is some of the largest availability and security concerns facing customers that are moving their applications to the cloud. A DDoS attempts to exhaust an application's resources, making the application unavailable to legitimate users.

**Mitigation:**

DDoS protection is important to ensure the availability of your cloud services. Azure provides DDoS solutions to you and your customers to provide extra safeguards to this popular attack.

1. Turn on DDoS protection feature.
2. All Azure services are protected with the DDoS Basic plan that handles large DDoS attacks.
3. Upgrading to the Standard plan allows better support and mitigation in the event of an active attack.

---

## Threat #13

**Principle:** Integrity  
**Threat:** Compromises to your software supply chain, including the packages and libraries your software depends on, can result in a multitude of severe breaches and disruptive incidents. Attackers exploiting vulnerabilities within these components can gain unauthorized access, execute malicious code, or disrupt critical systems, posing significant risks to the security and functionality of your software infrastructure.

**Mitigation:**

1. Create a Software Bill of Materials (SBOM) with every release. SBOMs document any dependencies of a piece of software and can be used to track down dependencies with bugs, malicious code, or known breaches.
2. Cryptographically sign your releases and any related artifacts, such as SBOMs or vulnerability scans. Verify those signatures before running anything to ensure integrity.

---

## Threat #14

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Internal network traffic  
**Threat:** Unencrypted network traffic within the tenant boundary may cause compliance issues if it contains sensitive data.

**Mitigation:**

All products and services must encrypt data in transit using approved cryptographic protocols and algorithms.

1. Use TLS to encrypt all HTTP-based network traffic. Use other mechanisms, such as IPSec, to encrypt non-HTTP network traffic that contains customer or confidential data.
2. Use only TLS 1.2 or TLS 1.3. Use ECDHE-based ciphers suites and NIST curves. Use strong keys. Enable HTTP Strict Transport Security (HSTS). Turn off TLS compression and do not use ticket-based session resumption.

---

## Threat #15

**Principle:** Confidentiality and Integrity  
**Affected Asset:** All services  
**Threat:** Secrets leaking into unsecured locations are an easy way for adversaries to gain access to a system. These secrets can be used to either spoof the owners of these secrets or, in the case of encryption keys, use them to decrypt data.

**Mitigation:**

Proper storage and management of secrets is critical in protecting systems from compromises, in most cases, with severe impact.

1. Never store secrets in code, configuration files or databases. Instead, use a vault or any secure container (such as encrypted variables) to store secrets.
2. Separate application secrets by environment.
3. Rotate all secrets before turning over the application to the customer.

- Store all secrets, encryption keys and certificates in Key Vault.
- You can use multiple Key Vaults to separate secrets for different and critical services to minimize secrets leaking
- Define and implement secrets rotation strategy. All items in the vault should have expiration dates.

---

## Threat #16

**Principle:** Confidentiality  
**Affected Asset:** Web application, Non Search Microservice, Search API RAG microservice, Conversational Microservice, Browser  
**Threat:** Insufficient scrutiny of LLM output, unfiltered acceptance of the LLM output could lead to unintended code execution.

Insecure Output Handling occurs when insufficient validation, sanitization, and handling of the outputs generated by large language models before they are passed downstream to other components and systems. Since LLM-generated content can be controlled by prompt input, this behavior is like providing users with indirect access to additional
functionality.

**Mitigation:**

1. Treat the model as any other user, adopting a zero-trust approach, and apply proper input validation on responses coming from the model to backend functions.
2. Follow the best practices to ensure effective input validation and sanitization.
3. Encode model output back to users to mitigate undesired code execution by JavaScript or Markdown.
4. Ensure pydantic-style validation of LLM outputs (including semantic validation such as checking for bias in generated text, checking for bugs in generated code, etc.)
    - take corrective actions (e.g. re-asking LLM) when validation fails.
    - enforce structure and type guarantee (e.g. JSON).
5. Use [Azure AI Content Safety Filters](https://azure.microsoft.com/en-us/products/ai-services/ai-content-safety/#features) for prompt inputs and its responses.

---

## Threat #17

**Principle:** Confidentiality  
**Affected Asset:** Chat Bot Service, Routing Microservice, Conversational Microservice, Azure OpenAI GPT3.5 Model, Search API RAG microservice  
**Threat:** Users can modify the system-level prompt restrictions to "jailbreak" the LLM and overwrite previous controls in place.

This threat arises when the proprietary LLM models are compromised, physically stolen, copied or weights and parameters are extracted to create a functional equivalent.

**Mitigation:**

1. Implement strong access controls and strong authentication mechanisms to limit unauthorized access to LLM model repositories and training environments.
2. Restrict the LLMs access to network resources, internal services and API's.
3. Regularly monitor and audit access logs and activities related to LLM model repositories to detect and respond to any suspicious activities.
4. Automate MLOps deployment with governance and tracking and approval workflows.
5. Rate Limiting API calls where applicable.
6. All customer or confidential data must be encrypted before being written to non-volatile storage media (encrypted at-rest) per the following requirements.
7. Use approved algorithms. This includes AES-256, AES-192, or AES-128.

---

## Threat #18

**Principle:** Integrity  
**Affected Asset:** LLM  
**Threat:** Training data poisoning refers to manipulation of pre-training data or data involved within the fine-tuning or embedding processes to introduce vulnerabilities (which all have unique and sometimes shared attack vectors), backdoors or biases that could compromise the model’s security, effectiveness or ethical behavior. Poisoned information may be surfaced to users or create other risks like performance degradation, downstream software exploitation and reputational damage.

**Mitigation:**

1. Verify the supply chain of the training data, especially when sourced externally as well as maintaining attestations via the \"ML-BOM\" (Machine Learning Bill of Materials) methodology as well as verifying model cards.
2. Verify the correct legitimacy of targeted data sources and data obtained obtained during both the pre-training, fine-tuning and embedding stages.
3. Verify your use-case for the LLM and the application it will integrate to. Craft different models via separate training data or fine-tuning for different use-cases to create a more granular and accurate generative AI output as per its defined use-case.
4. Ensure sufficient sandboxing through network controls is present to prevent the model from scraping unintended data sources which could hinder the machine learning output.
5. Use strict vetting or input filters for specific training data or categories of data sources to control volume of falsified data. Data sanitization, with techniques such as statistical outlier detection and anomaly detection methods to detect and remove adversarial data from potentially being fed into the fine-tuning process.
6. Adversarial robustness techniques such as federated learning and constraints to minimize the effect of outliers or adversarial training to be vigorous against worst-case perturbations of the training data.
7. An \"MLSecOps\" approach could be to include adversarial robustness to the training lifecycle with the auto poisoning technique.
8. An example repository of this would be Auto poison testing, including both attacks such as Content Injection Attacks ("attempting to promote a brand name in model responses") and Refusal Attacks ("always making the model refuse to respond") that can be accomplished with this approach.
9. Testing and Detection, by measuring the loss during the training stage and analyzing trained models to detect signs of a poisoning attack by analyzing model behavior on specific test inputs.
10. Monitoring and alerting the number of skewed responses exceeding a threshold.
11. Use of a human loop to review responses and auditing.
12. Implement dedicated LLMs to benchmark against undesired consequences and trained other LLMs using reinforcement learning techniques.
13. Perform LLM-based red team exercises or LLM vulnerability scanning in the testing phases of the LLM lifecycle.

---

## Threat #19

**Principle:** Integrity  
**Affected Asset:** Web application, Non Search Microservice, Search API RAG microservice, Conversational Microservice, Browser  
**Threat:** Overreliance can occur when an LLM produces erroneous information and provides it in an authoritative manner.

**Mitigation:**

1. Regularly monitor and review the LLM outputs. Use self-consistency or voting techniques to filter out inconsistent text. Comparing multiple model responses for a single prompt can better judge the quality and consistency of output.
2. Cross-check the LLM output with trusted external sources. This additional layer of validation can help ensure the information provided by the model is accurate and reliable.
3. Enhance the model with fine-tuning or embeddings to improve output quality. Generic pre-trained models are more likely to produce inaccurate information compared to tuned models in a particular domain. Techniques such as prompt engineering, parameter efficient tuning (PET), full model tuning, and chain of thought prompting can be employed for this purpose.
4. Implement automatic validation mechanisms that can cross-verify the generated output against known facts or data. This can provide an additional layer of security and mitigate the risks associated with hallucinations.
5. Break down complex tasks into manageable subtasks and assign them to different agents. This not only helps in managing complexity, but it also reduces the chances of hallucinations as each agent can be held accountable for a smaller task.
6. Communicate the risks and limitations associated with using LLMs. This includes potential for information inaccuracies, and other risks. Effective risk communication can prepare users for potential issues and help them make informed decisions.
7. Build APIs and user interfaces that encourage responsible and safe use of LLMs. This can involve measures such as content filters, user warnings about potential inaccuracies, and clear labeling of AI-generated content.
8. When using LLMs in development environments, establish secure coding practices and guidelines to prevent the integration of possible vulnerabilities.
9. Educate users so that they understand the implications of using the LLM outputs directly without any validation.

---

## Threat #20

**Principle:** Avaliability  
**Affected Asset:** Chat Bot Service, Routing Microservice, Conversational Microservice, Azure OpenAI GPT3.5 Model, Search API RAG microservice  
**Threat:** An attacker interacts with an LLM in a method that consumes an exceptionally high amount of resources, which results in a decline in the quality of service for them and other users, as well as potentially incurring high resource costs. This can also be due to vulnerabilities in supply chain.

**Mitigation:**

1. Implement input validation and sanitization to ensure user input adheres to defined limits and filters out any malicious content.
2. Cap resource use per request or step, so that requests involving complex parts execute more slowly.
3. Enforce API rate limits to restrict the number of requests an individual user or IP address can make within a specific time frame.
4. Limit the number of queued actions and the number of total actions in a system reacting to LLM responses.
5. Continuously monitor the resource utilization of the LLM to identify abnormal spikes or patterns that may indicate a DoS attack.
6. Set strict input limits based on the LLMs context window to prevent overload and resource exhaustion.
7. Promote awareness among developers about potential DoS vulnerabilities in LLMs and provide guidelines for secure LLM implementation.
8. All services within the Azure Trust Boundary must authenticate all incoming requests, including requests coming from the same network. Proper authorization should also be applied to prevent unnecessary privileges.
9. Whenever available, use Azure Managed Identities to authenticate services. Service Principals may be used if Managed Identities are not supported.
10. External users or services may use Username + Passwords, Tokens, or Certificates to authenticate, provided these are stored on Key Vault or any other vaulting solution.
11. For authorization, use Azure RBAC and conditional access policies to segregate duties and grant only the least amount of access to perform an action at a particular scope.

---

## Threat #21

**Principle:** Confidentiality and Privacy  
**Affected Asset:** Web application, Non Search Microservice, Search API RAG microservice, Conversational Microservice, Browser  
**Threat:** LLM applications have the potential to reveal sensitive information,proprietary algorithms, or other confidential details through their output. This can result in unauthorized access to sensitive data, intellectual property, privacy violations, and other security breaches.

**Mitigation:**

1. Integrate adequate data sanitization and scrubbing techniques to prevent user data from entering the training model data.
2. Implement robust input validation and sanitization methods to identify and filter out potential malicious inputs to prevent the model from being poisoned.
3. When enriching the model with data and if fine-tuning a model.
4. Anything that is deemed sensitive in the fine-tuning data has the potential to be revealed to a user. Therefore, apply the rule of least privilege and do not train the model on information that the highest-privileged user can access which may be displayed to a lower-privileged user.
5. Access to external data sources (orchestration of data at runtime) should be limited.
6. Apply strict access control methods to external data sources and a rigorous approach to maintaining a secure supply chain.

---

## Threat #22

**Principle:** Confidentiality and Integrity  
**Affected Asset:** LLM  
**Threat:** Excessive Agency is the vulnerability that enables damaging actions to be performed in response to unexpected/ambiguous outputs from an LLM.

**Mitigation:**

1. Limit the plugins/tools that LLM agents are allowed to call to only the minimum functions necessary.
2. Limit the functions that are implemented in LLM plugins/tools to the minimum necessary.
3. Avoid open-ended functions where possible.
4. Limit the permissions that LLM plugins/tools are granted to other systems to the minimum necessary to limit the scope of undesirable actions.
5. Track user authorization and security scope to ensure actions taken on behalf of a user are executed on downstream systems in the context of that specific user, and with the minimum privileges
necessary.
6. Utilize human-in-the-loop control to require a human to approve all actions before they are taken.
7. Implement authorization in downstream systems rather than relying on an LLM to decide if an action is allowed or not. When implementing tools/plugins enforce the complete mediation principle so that all requests made to downstream systems via the plugins/tools are validated against security policies.

---

## Threat #23

**Principle:** Confidentiality, Integrity, Avaliability and Privacy  
**Affected Asset:** Chat Bot Service, Routing Microservice, Conversational Microservice, Azure OpenAI GPT3.5 Model, Search API RAG microservice
**Threat:** Users can modify the system-level prompt restrictions to "jailbreak" the LLM and overwrite previous controls in place.

As a result of the vulnerability, an attacker can create malicious input to manipulate LLMs into unknowingly execute unintended actions. There are two types of prompt injection direct and indirect. Direct prompt injections, also known as jail breaking, occur when an attacker overwrites or reveal underlying system prompt. This allows attackers to exploit backend systems.

Indirect prompt injections occur when LLM accepts input from external sources like websites or files. Attackers may embed a prompt injection in the external content.

**Mitigation:**

1. Enforce privilege control on LLM access to backend systems.
2. Segregate external content from user prompts and limit the influence when untrusted content is used.
3. Manually monitor input and output periodically to check that as expected.
4. Maintain fine user control on decision making capabilities by LLM.
5. All products and services must encrypt data in transit using approved cryptographic protocols and algorithms.
6. Use [Azure AI Content Safety Filters](https://azure.microsoft.com/en-us/products/ai-services/ai-content-safety/#features) for prompt inputs and its responses.
7. Use TLS to encrypt all HTTP-based network traffic. Use other mechanisms, such as IPSec, to encrypt non-HTTP network traffic that contains customer or confidential data.
8. Use only TLS 1.2 or TLS 1.3. Use ECDHE-based ciphers suites and NIST curves. Use strong keys. Enable HTTP Strict Transport Security. Turn off TLS compression and do not use ticket-based session resumption.

---

## Threat #24

**Principle:** Confidentiality, Integrity and Avaliability  
**Affected Asset:** Web application, Non Search Microservice, Search API RAG microservice, Conversational Microservice, Azure OpenAI GPT 3.5 model, Azure OpenAI Text moderation tool, Azure Cognitive Service API APP  
**Threat:** Vulnerabilities in the open source/third party packages used for development could lead to an attacker exploiting those vulnerabilities

This threat occurs due to vulnerabilities in software components, training data, ML models or deployment platforms.

**Mitigation:**

1. Use Azure Artifacts to publish and control feeds, that will lower risk of supply chain vulnerability.
2. Carefully vet data sources and suppliers, including T&Cs and their privacy policies, only using trusted suppliers. Ensure adequate and independently audited security is in place and that model operator policies align with your data protection policies, i.e., your data is not used for training their models; similarly, seek assurances and legal mitigation against using copyrighted material from model maintainers.
3. Only use reputable plug-ins and ensure they have been tested for your application requirements. LLM-Insecure Plugin Design provides information on the LLM-aspects of Insecure Plugin design you should test against to mitigate risks from using third-party plugins.
4. Maintain an up-to-date inventory of components using a Software Bill of Materials (SBOM) to ensure you have an up-to-date, accurate, and signed inventory preventing tampering with deployed packages. SBOMs can be used to detect and alert new, zero-day vulnerabilities quickly.
5. At the time of writing, SBOMs do not cover models, their artifacts, and datasets. If your LLM application uses its own model, you should use MLOps best practices and platforms offering secure model repositories with data, model, and experiment tracking.
6. You should also use model and code signing when using external models and suppliers.
7. Anomaly detection and adversarial robustness tests on supplied models and data can help detect tampering and poisoning.
8. Implement sufficient monitoring to cover component and environment vulnerabilities scanning, use of unauthorized plugins, and out-of-date components, including the model and its artifacts.
9. Implement a patching policy to mitigate vulnerable or outdated components. Ensure the application relies on a maintained version of APIs and the underlying model.
10. Regularly review and audit supplier Security and Access, ensuring no changes in their security posture or T&Cs.

---

## Threat #25

**Principle:** Integrity  
**Affected Asset:** All data stores  
**Threat:** Malicious files can compromise cloud applications and will continue to be an issue if they persist on file storage and databases.

**Mitigation:**

Scan all files before uploading them to non-compute Azure resources.

Use a scanning solution that meets the following criteria:

1. Scans should cover ALL files.
2. Access to unscanned files should be locked down until proof of a file’s safety is generated.
3. Generate persistent evidence for compliance and security operations purposes.

---

## Threat #26

**Principle:** Confidentiality  
**Affected Asset:** All data stores  
**Threat:** Data is a valuable target for most threat actors and attacking the data store directly, as opposed to stealing it during transit, allows data exfiltration at a much larger scale.

**Mitigation:**

All customer or confidential data must be encrypted before being written to non-volatile storage media (encrypted at-rest) per the following requirements.

1. Use approved algorithms. This includes AES-256, AES-192, or AES-128.
2. Leverage SQL TDE whenever available.
3. Encryption must be enabled before writing data to storage.
4. Azure Storage, Cosmos DB, Azure SQL Database and Azure Database for MySQL encryption for data at rest is always on and uses AES-256.
5. Transparent Database Encryption (TDE) (to encrypt data at rest) with service managed keys are enabled by default for any Azure SQL databases.
6. Azure SQL Database backup data is automatically encrypted using Azure platform-managed keys.
7. If utilizing IaC products, ensure the configuration files are stored in a storage account or other safe location and not in your repository.

---

## Threat #27

**Principle:** Privacy  
**Affected Asset:** All data stores  
**Threat:** The application does not allow the deletion of a user’s profile from storage.

**Mitigation:**

If required by regulation, implement the ability to delete a user’s profile, and all associated data, from all the datastores.

---
