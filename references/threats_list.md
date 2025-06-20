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

**Recommendation #:** 1

**Category:** DevOps Security  
**Title:** DS-2: Ensure software supply chain security

Ensure the enterprise’s SDLC (Software Development Lifecycle) or process include a set of security controls to govern the in-house and third-party software components (including both proprietary and open-source software) where applications have dependencies. Define gating criteria to prevent vulnerable or malicious components being integrated and deployed into the environment.

The software supply chain security controls should at least include the following aspects:

Properly manage a Software Bill of Materials (SBOM) by identifying the upstream dependencies required for the service/resource development, build, integration and deployment phase.
Inventory and track the in-house and third-party software components for known vulnerability when there is a fix available in the upstream.
Assess the vulnerabilities and malware in the software components using static and dynamic application testing for unknown vulnerabilities.
Ensure the vulnerabilities and malware are mitigated using the appropriate approach. This may include source code local or upstream fix, feature exclusion and/or applying compensating controls if the direct mitigation is not available.
If closed source third-party components are used in production environments, there may be limited visibility to its security posture. Consider additional controls such as access control, network isolation and endpoint security to minimize the impact if there is malicious activity or vulnerabilities associated with the component.

**Azure Guidance:**

For the GitHub platform, ensure the software supply chain security through the following capability or tools from GitHub Advanced Security or GitHub’s native feature:- Use Dependency Graph to scan, inventory and identify all your project’s dependencies and related vulnerabilities through Advisory Database.

- Use Dependabot to ensure that the vulnerable dependency is tracked and remediated, and ensure your repository automatically keeps up with the latest releases of the packages and applications it depends on.
- Use GitHub's native code scanning capability to scan the source code when sourcing the code externally.
- Use Microsoft Defender for Cloud to integrate vulnerability assessment for your container image in the CI/CD workflow.
For Azure DevOps, you can use third-party extensions to implement similar controls to inventory, analyze and remediate the third-party software components and their vulnerabilities


GitHub Dependency Graph:
 [https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)

GitHub Dependabot:
 [https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates)

Identify vulnerable container images in your CI/CD workflows:
 [https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd](https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd)

Azure DevOps Marketplace – supply chain security:
 [https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS](https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS)

**Recommendation #:** 2

**Category:** DevOps Security  
**Title:** DS-3: Secure DevOps infrastructure

Ensure the DevOps infrastructure and pipeline follow security best practices across environments including your build, test, and production stages. This typically includes the security controls for following scope:

- Artifact repositories that store source code, built packages and images, project artifacts and business data.
- Servers, services, and tooling that host CI/CD pipelines.
- CI/CD pipeline configuration.

**Azure Guidance:**

As part of applying the Microsoft Cloud Security Benchmark to your DevOps infrastructure security controls, prioritize the following controls:
- Protect artifacts and the underlying environment to ensure the CI/CD pipelines don’t become avenues to insert malicious code. For example, review your CI/CD pipeline to identify any misconfiguration in core areas of Azure DevOps such as Organization, Projects, Users, Pipelines (Build & Release), Connections, and Build Agent to identify any misconfigurations such as open access, weak authentication, insecure connection setup and so on. For GitHub, use similar controls to secure the Organization permission levels.
- Ensure your DevOps infrastructure is deployed consistently across development projects. Track compliance of your DevOps infrastructure at scale by using Microsoft Defender for Cloud (such as Compliance Dashboard, Azure Policy, Cloud Posture Management) or your own compliance monitoring tools.
- Configure identity/role permissions and entitlement policies in Entra ID, native services, and CI/CD tools in your pipeline to ensure changes to the pipelines are authorized.
- Avoid providing permanent “standing” privileged access to the human accounts such as developers or testers by using features such as Azure managed identifies and just-in-time access.
- Remove keys, credentials, and secrets from code and scripts used in CI/CD workflow jobs and keep them in a key store or Azure Key Vault.
- If you run self-hosted build/deployment agents, follow Microsoft Cloud Security Benchmark controls including network security, posture and vulnerability management, and endpoint security to secure your environment.

Note: Refer to the Logging and Threat Detection, DS-7, and the Posture and Vulnerability Management sections to use services such as Azure Monitor and Microsoft Sentinel to enable governance, compliance, operational auditing, and risk auditing for your DevOps infrastructure.

DevSecOps controls overview – secure pipelines:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/secure/devsecops-controls](https://docs.microsoft.com/azure/cloud-adoption-framework/secure/devsecops-controls)

Secure your GitHub organization:
 [https://docs.github.com/en/code-security/getting-started/securing-your-organization](https://docs.github.com/en/code-security/getting-started/securing-your-organization)

Azure DevOps pipeline – Microsoft hosted agent security considerations:
 [https://docs.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#security](https://docs.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#security)




**Recommendation #:** 3

**Category:** DevOps Security  
**Title:** DS-4: Integrate static application security testing into DevOps pipeline

Ensure static application security testing (SAST) fuzzy testing, interactive testing, mobile application testing, are part of the gating controls in the CI/CD workflow. The gating can be set based on the testing results to prevent vulnerable packages from committing into the repository, building into the packages, or deploying into the production.

**Azure Guidance:**

Integrate SAST into CI/CD pipelines (e.g., in infrastructure as code templates) so source code can be scanned automatically in CI/CD workflows. Azure DevOps Pipeline or GitHub can integrate the below tools and third-party SAST tools into workflows.
- GitHub CodeQL for source code analysis.
- Microsoft BinSkim Binary Analyzer for Windows and *nix binary analysis.
- Azure DevOps Credential Scanner (Microsoft Security DevOps extension) and GitHub native secret scanning for credential scan in the source code.

GitHub CodeQL:
 [https://codeql.github.com/docs/](https://codeql.github.com/docs/)

BinSkim Binary Analyzer:
 [https://github.com/microsoft/binskim](https://github.com/microsoft/binskim)

Azure DevOps Credential Scan:
 [https://secdevtools.azurewebsites.net/helpcredscan.html](https://secdevtools.azurewebsites.net/helpcredscan.html)

GitHub secret scanning:
 [https://docs.github.com/en/code-security/secret-security/about-secret-scanning](https://docs.github.com/en/code-security/secret-security/about-secret-scanning)

**Recommendation #:** 4

**Category:** DevOps Security  
**Title:** DS-5: Integrate dynamic application security testing into DevOps pipeline

Ensure dynamic application security testing (DAST) are part of the gating controls in the CI/CD workflow. The gating can be set based on the testing results to prevent vulnerability from building into the packages or deploying into the production.

**Azure Guidance:**

Integrate DAST into CI/CD pipelines so runtime applications can be tested automatically in Azure DevOps or GitHub workflows. Automated penetration testing (with manual assisted validation) should also be part of the DAST.

Azure DevOps Pipeline or GitHub supports the integration of third-party DAST tools into the CI/CD workflow.

DAST tools in Azure DevOps marketplace:
 [https://marketplace.visualstudio.com/search?term=DAST&target=AzureDevOps&category=All%20categories](https://marketplace.visualstudio.com/search?term=DAST&target=AzureDevOps&category=All%20categories)

**Recommendation #:** 5

**Category:** DevOps Security  
**Title:** DS-6: Enforce security of workload throughout DevOps lifecycle

Ensure the workload is secured throughout the entire lifecycle in development, testing, and deployment stage. Use Microsoft Cloud Security Benchmark to evaluate the controls (such as network security, identity management, privileged access and so on) that can be set as guardrails by default or shift left prior to the deployment stage. In particular, ensure the following controls are in place in your DevOps process:
- Automate the deployment by using Azure or third-party tooling in the CI/CD workflow, infrastructure management (infrastructure as code), and testing to reduce human error and attack surface.
- Ensure VMs, container images and other artifacts are secure from malicious manipulation.
- Scan the workload artifacts (in other words, container images, dependencies, SAST and DAST scans) prior to the deployment in the CI/CD workflow
- Deploy vulnerability assessment and threat detection capability into the production environment and continuously use these capabilities in the run-time.

**Azure Guidance:**

Guidance for Azure VMs:
- Use Azure Shared Image Gallery to share and control access to your images by different users, service principals, or AD groups within your organization. Use Azure role-based access control (Azure RBAC) to ensure that only authorized users can access your custom images.
- Define the secure configuration baselines for the VMs to eliminate unnecessary credentials, permissions, and packages. Deploy and enforce configuration baselines through custom images, Azure Resource Manager templates, and/or Azure Policy guest configuration.

Guidance for Azure container services:
- Use Azure Container Registry (ACR) to create your private container registry where granular access can be restricted through Azure RBAC, so only authorized services and accounts can access the containers in the private registry.
- Use Defender for Containers for vulnerability assessment of the images in your private Azure Container Registry. In addition, you can use Microsoft Defender for Cloud to integrate the container image scans as part of your CI/CD workflows.

For Azure serverless services, adopt similar controls to ensure security controls "shift-left" to the stage prior to deployment.  

Shared Image Gallery overview:
 [https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)

How to implement Microsoft Defender for Cloud vulnerability assessment recommendations:  [https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

Security considerations for Azure Container:
 [https://docs.microsoft.com/azure/container-instances/container-instances-image-security](https://docs.microsoft.com/azure/container-instances/container-instances-image-security)

Azure Defender for container registries:
 [https://docs.microsoft.com/azure/security-center/defender-for-container-registries-introduction](https://docs.microsoft.com/azure/security-center/defender-for-container-registries-introduction)

**Recommendation #:** 6

**Category:** DevOps Security  
**Title:** DS-7: Enable logging and monitoring in DevOps

Ensure your logging and monitoring scope includes non-production environments and CI/CD workflow elements used in DevOps (and any other development processes). The vulnerabilities and threats targeting these environments can introduce significant risks to your production environment if they are not monitored properly. The events from the CI/CD build, test and deployment workflow should also be monitored to identify any deviations in the CI/CD workflow jobs.

Follow Microsoft Cloud Security Benchmark – Logging and Threat Detection as the guideline to implement your logging and monitoring controls for workload.

**Azure Guidance:**

Enable and configure the audit logging capabilities in non-production and CI/CD tooling environments (such as Azure DevOps and GitHub) used throughout the DevOps process.

The events generated from Azure DevOps and the GitHub CI/CD workflow, including the build, test and deployment jobs, should also be monitored to identify any anomalous results.

Ingest the above logs and events into Microsoft Sentinel or other SIEM tools through a logging stream or API to ensure the security incidents are properly monitored and triaged for handling.

Azure DevOps - audit streaming:
 [https://docs.microsoft.com/azure/devops/organizations/audit/auditing-streaming?view=azure-devops](https://docs.microsoft.com/azure/devops/organizations/audit/auditing-streaming?view=azure-devops)

GitHub logging:
 [https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization)




---

## Threat #2

**Principle:** Confidentiality and Integrity  
**Affected Asset:** CI/CD Pipelines  
**Threat:** CI/CD Pipelines should always be cleansed of sensitive information being leaked.

**Mitigation:**

All secrets, key materials, and credentials stored in CI/CD pipelines must be protected from exposure in plaintext format.

1. Implement pre-commit checks to ensure secure pipelines free of sensitive artifacts.

**Recommendation #:** 1

**Category:** DevOps Security  
**Title:** DS-4: Integrate static application security testing into DevOps pipeline

Ensure static application security testing (SAST) fuzzy testing, interactive testing, mobile application testing, are part of the gating controls in the CI/CD workflow. The gating can be set based on the testing results to prevent vulnerable packages from committing into the repository, building into the packages, or deploying into the production.

**Azure Guidance:**

Integrate SAST into CI/CD pipelines (e.g., in infrastructure as code templates) so source code can be scanned automatically in CI/CD workflows. Azure DevOps Pipeline or GitHub can integrate the below tools and third-party SAST tools into workflows.
- GitHub CodeQL for source code analysis.
- Microsoft BinSkim Binary Analyzer for Windows and *nix binary analysis.
- Azure DevOps Credential Scanner (Microsoft Security DevOps extension) and GitHub native secret scanning for credential scan in the source code.

GitHub CodeQL:
 [https://codeql.github.com/docs/](https://codeql.github.com/docs/)

BinSkim Binary Analyzer:
 [https://github.com/microsoft/binskim](https://github.com/microsoft/binskim)

Azure DevOps Credential Scan:
 [https://secdevtools.azurewebsites.net/helpcredscan.html](https://secdevtools.azurewebsites.net/helpcredscan.html)

GitHub secret scanning:
 [https://docs.github.com/en/code-security/secret-security/about-secret-scanning](https://docs.github.com/en/code-security/secret-security/about-secret-scanning)




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

**Recommendation #:** 1

**Category:** Data Protection  
**Title:** DP-3: Encrypt sensitive data in transit

Protect the data in transit against 'out of band' attacks (such as traffic capture) using encryption to ensure that attackers cannot easily read or modify the data.

Set the network boundary and service scope where data in transit encryption is mandatory inside and outside of the network. While this is optional for traffic on private networks, this is critical for traffic on external and public networks.

**Azure Guidance:**

Enforce secure transfer in services such as Azure Storage, where a native data in transit encryption feature is built in.

Enforce HTTPS for web application workloads and services by ensuring that any clients connecting to your Azure resources use transport layer security (TLS) v1.2 or later. For remote management of VMs, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol.

For remote management of Azure virtual machines, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol. For secure file transfer, use the SFTP/FTPS service in Azure Storage Blob, App Service apps, and Function apps, instead of using the regular FTP service.

Note: Data in transit encryption is enabled for all Azure traffic traveling between Azure datacenters. TLS v1.2 or later is enabled on most Azure services by default. And some services such as Azure Storage and Application Gateway can enforce TLS v1.2 or later on the server side.


Double encryption for Azure data in transit:
 [https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit)

Understand encryption in transit with Azure:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

Information on TLS Security:
 [https://docs.microsoft.com/security/engineering/solving-tls1-problem](https://docs.microsoft.com/security/engineering/solving-tls1-problem)

Enforce secure transfer in Azure storage:
 [https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account)

**Recommendation #:** 2

**Category:** Logging and threat detection  
**Title:** LT-3: Enable logging for security investigation

Enable logging for cloud resources to meet the requirements for security incident investigations and security response and compliance purposes.

**Azure Guidance:**

Enable logging capability for resources at the different tiers, such as logs for Azure resources, operating systems and applications inside VMs and other log types.

Be mindful about different types of logs for security, audit, and other operational logs at the management/control plane and data plane tiers. There are three types of the logs available at the Azure platform:
- Azure resource log: Logging of operations that are performed within an Azure resource (the data plane). For example, getting a secret from a key vault or making a request to a database. The content of resource logs varies by the Azure service and resource type.
- Azure activity log: Logging of operations on each Azure resource at the subscription layer, from the outside (the management plane). You can use the Activity Log to determine what, who, and when for any write operations (PUT, POST, DELETE) taken on the resources in your subscription. There is a single Activity log for each Azure subscription.
- Azure Active Directory logs: Logs of the history of sign-in activity and audit trail of changes made in the Azure Active Directory for a particular tenant.

You can also use Microsoft Defender for Cloud and Azure Policy to enable resource logs and log data collecting on Azure resources.

Understand logging and different log types in Azure:
 [https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

Understand Microsoft Defender for Cloud data collection:
 [https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

Enable and configure antimalware monitoring:
 [https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets](https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets)

Operating systems and application logs inside in your compute resources:
 [https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest](https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest)

**Recommendation #:** 3

**Category:** Network Security  
**Title:** NS-6: Deploy web application firewall

Deploy a web application firewall (WAF) and configure the appropriate rules to protect your web applications and APIs from application-specific attacks.

**Azure Guidance:**

Use web application firewall (WAF) capabilities in Azure Application Gateway, Azure Front Door, and Azure Content Delivery Network (CDN) to protect applications, services and APIs against application layer attacks at the edge of your network.

Set your WAF in "detection" or "prevention mode," depending on your needs and threat landscape.

Choose a built-in ruleset, such as OWASP Top 10 vulnerabilities, and tune it to your application needs.

How to deploy Azure WAF:
 [https://docs.microsoft.com/azure/web-application-firewall/overview](https://docs.microsoft.com/azure/web-application-firewall/overview)



**Recommendation #:** 4

**Category:** Posture and Vulnerability Management  
**Title:** PV-7: Conduct regular red team operations

Simulate real-world attacks to provide a more complete view of your organization's vulnerability. Red team operations and penetration testing complement the traditional vulnerability scanning approach to discover risks.

Follow industry best practices to design, prepare and conduct this kind of testing to ensure it will not cause damage or disruption to your environment. This should always include discussing testing scope and constraints with relevant stakeholders and resource owners.

**Azure Guidance:**

As required, conduct penetration testing or red team activities on your Azure resources and ensure remediation of all critical security findings.

Follow the Microsoft Cloud Penetration Testing Rules of Engagement to ensure your penetration tests are not in violation of Microsoft policies. Use Microsoft's strategy and execution of Red Teaming and live site penetration testing against Microsoft-managed cloud infrastructure, services, and applications.

Penetration testing in Azure:
 [https://docs.microsoft.com/azure/security/fundamentals/pen-testing](https://docs.microsoft.com/azure/security/fundamentals/pen-testing)

Penetration Testing Rules of Engagement:
 [https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

Microsoft Cloud Red Teaming:
 [https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)

Technical Guide to Information Security Testing and Assessment:
 [https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-115.pdf](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-115.pdf)




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

**Recommendation #:** 1

**Category:** Data Protection  
**Title:** DP-1: Discover, classify, and label sensitive data



Establish and maintain an inventory of the sensitive data, based on the defined sensitive data scope. Use tools to discover, classify and label the in- scope sensitive data.

**Azure Guidance:**

Use tools such as Microsoft Purview, which combines the former Azure Purview and Microsoft 365 compliance solutions, and Azure SQL Data Discovery and Classification to centrally scan, classify, and label the sensitive data that reside in the Azure, on-premises, Microsoft 365, and other locations.

Data classification overview:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/govern/policy-compliance/data-classification](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/policy-compliance/data-classification)

Labeling in the Microsoft Purview Data Map:
 [https://docs.microsoft.com/azure/purview/create-sensitivity-label](https://docs.microsoft.com/azure/purview/create-sensitivity-label)

Tag sensitive information using Azure Information Protection:
 [https://docs.microsoft.com/azure/information-protection/what-is-information-protection](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)

How to implement Azure SQL Data Discovery:
 [https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

Microsoft Purview data sources:
 [https://docs.microsoft.com/azure/purview/purview-connector-overview#purview-data-sources](https://docs.microsoft.com/azure/purview/purview-connector-overview#purview-data-sources)

**Recommendation #:** 2

**Category:** Data Protection  
**Title:** DP-2: Monitor anomalies and threats targeting sensitive data

Monitor for anomalies around sensitive data, such as unauthorized transfer of data to locations outside of enterprise visibility and control. This typically involves monitoring for anomalous activities (large or unusual transfers) that could indicate unauthorized data exfiltration.

**Azure Guidance:**

Use Azure Information protection (AIP) to monitor the data that has been classified and labeled.

Use Microsoft Defender for Storage, Microsoft Defender for SQL, Microsoft Defender for open-source relational databases, and Microsoft Defender for Cosmos DB to alert on anomalous transfer of information that might indicate unauthorized transfers of sensitive data information.  

Note: If required for compliance of data loss prevention (DLP), you can use a host-based DLP solution from Azure Marketplace or a Microsoft 365 DLP solution to enforce detective and/or preventative controls to prevent data exfiltration.

Enable Azure Defender for SQL:
 [https://docs.microsoft.com/azure/azure-sql/database/azure-defender-for-sql](https://docs.microsoft.com/azure/azure-sql/database/azure-defender-for-sql)

Enable Azure Defender for Storage:
 [https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center)

Enable Microsoft Defender for Azure Cosmos DB:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-enable-cosmos-protections?tabs=azure-portal](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-enable-cosmos-protections?tabs=azure-portal)

Enable Microsoft Defender for open-source relational databases and respond to alerts:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-usage](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-usage)

**Recommendation #:** 3

**Category:** Data Protection  
**Title:** DP-4: Enable data at rest encryption by default  

To complement access controls, data at rest should be protected against 'out of band' attacks (such as accessing underlying storage) using encryption. This helps ensure that attackers cannot easily read or modify the data.

**Azure Guidance:**

Many Azure services have data at rest encryption enabled by default at the infrastructure layer using a service-managed key. These service-managed keys are generated on the customer’s behalf and automatically rotated every two years.

Where technically feasible and not enabled by default, you can enable data at rest encryption in the Azure services, or in your VMs at the storage level, file level, or database level.

Understand encryption at rest in Azure:  [https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#encryption-at-rest-in-microsoft-cloud-services](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#encryption-at-rest-in-microsoft-cloud-services)

Data at rest double encryption in Azure:  [ [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)]( [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models))

Encryption model and key management table:
 [ [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)]( [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models))

**Recommendation #:** 4

**Category:** Logging and threat detection  
**Title:** LT-3: Enable logging for security investigation

Enable logging for your cloud resources to meet the requirements for security incident investigations and security response and compliance purposes.

**Azure Guidance:**

Enable logging capability for resources at the different tiers, such as logs for Azure resources, operating systems and applications inside in your VMs and other log types.

Be mindful about different types of logs for security, audit, and other operational logs at the management/control plane and data plane tiers. There are three types of the logs available at the Azure platform:
- Azure resource log: Logging of operations that are performed within an Azure resource (the data plane). For example, getting a secret from a key vault or making a request to a database. The content of resource logs varies by the Azure service and resource type.
- Azure activity log: Logging of operations on each Azure resource at the subscription layer, from the outside (the management plane). You can use the Activity Log to determine what, who, and when for any write operations (PUT, POST, DELETE) taken on the resources in your subscription. There is a single Activity log for each Azure subscription.
- Azure Active Directory logs: Logs of the history of sign-in activity and audit trail of changes made in the Azure Active Directory for a particular tenant.

You can also use Microsoft Defender for Cloud and Azure Policy to enable resource logs and log data collecting on Azure resources.

Understand logging and different log types in Azure:
 [https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

Understand Microsoft Defender for Cloud data collection:
 [https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

Enable and configure antimalware monitoring:
 [https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets](https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets)

Operating systems and application logs inside in your compute resources:
 [https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest](https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest)

**Recommendation #:** 5

**Category:** Logging and threat detection  
**Title:** LT-5: Centralize security log management and analysis

Centralize logging storage and analysis to enable correlation across log data. For each log source, ensure that you have assigned a data owner, access guidance, storage location, what tools are used to process and access the data, and data retention requirements.

Use Cloud native SIEM if you don't have an existing SIEM solution for CSPs. or aggregate logs/alerts into your existing SIEM.

**Azure Guidance:**

Ensure that you are integrating Azure activity logs into a centralized Log Analytics workspace. Use Azure Monitor to query and perform analytics and create alert rules using the logs aggregated from Azure services, endpoint devices, network resources, and other security systems.

In addition, enable and onboard data to Microsoft Sentinel which provides security information event management (SIEM) and security orchestration automated response (SOAR) capabilities.

How to collect platform logs and metrics with Azure Monitor:
 [https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

How to onboard Azure Sentinel:
 [https://docs.microsoft.com/azure/sentinel/quickstart-onboard](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Recommendation #:** 6

**Category:** Backup and recovery  
**Title:** BR-1: Ensure regular automated backups

Ensure backup of business-critical resources, either during resource creation or enforced through policy for existing resources.

**Azure Guidance:**

For Azure Backup supported resources (such as Azure VMs, SQL Server, HANA databases, Azure PostgreSQL Database, File Shares, Blobs or Disks), enable Azure Backup and configure the desired frequency and retention period. For Azure VM, you can use Azure Policy to have backup automatically enabled using Azure Policy.

For resources or services not supported by Azure Backup, use the native backup capability provided by the resource or service. For example, Azure Key Vault provides a native backup capability.

For resources/services that are neither supported by Azure Backup nor have a native backup capability, evaluate backup and disaster needs, and create appropriate mechanisms as per business requirements. For example:
- If Azure Storage is used for data storage, enable blob versioning for storage blobs which will allow preservation, retrieval, and restoration of every version of every object stored in Azure Storage.
- Service configuration settings can usually be exported to Azure Resource Manager templates.

How to enable Azure Backup:
 [https://docs.microsoft.com/azure/backup/](https://docs.microsoft.com/azure/backup/)

Auto-Enable Backup on VM Creation using Azure Policy:
 [https://docs.microsoft.com/azure/backup/](https://docs.microsoft.com/azure/backup/)backup-azure-auto-enable-backup

**Recommendation #:** 7

**Category:** Backup and recovery  
**Title:** BR-2: Protect backup and recovery data

Ensure backup data and operations are protected from data exfiltration, data compromise, ransomware/malware and malicious insiders. The security controls that should be applied include user and network access control, data encryption at-rest and in-transit.

**Azure Guidance:**

Use multi-factor-authentication and Azure RBAC to secure the critical Azure Backup operations (such as delete, change retention, updates to backup config). For Azure Backup supported resources, use Azure RBAC to segregate duties and enable fine grained access, and create private endpoints within your Azure Virtual Network to securely backup and restore data from your Recovery Services vaults.

For Azure Backup supported resources, backup data is automatically encrypted using Azure platform-managed keys with 256-bit AES encryption. You can also choose to encrypt the backups using a customer managed key. In this case, ensure the customer-managed key in the Azure Key Vault is also in the backup scope. If you use a customer-managed key, use soft delete and purge protection in Azure Key Vault to protect keys from accidental or malicious deletion. For on-premises backups using Azure Backup, encryption-at-rest is provided using the passphrase you provide.

Safeguard backup data from accidental or malicious deletion, such as ransomware attacks/attempts to encrypt or tamper backup data. For Azure Backup supported resources, enable soft delete to ensure recovery of items with no data loss for up to 14 days after an unauthorized deletion, and enable multifactor authentication using a PIN generated in the Azure portal. Also enable geo-redundant storage or cross-region restoration to ensure backup data is restorable when there is a disaster in primary region. You can also enable Zone-redundant Storage (ZRS) to ensure backups are restorable during zonal failures.

Note: If you use a resource's native backup feature or backup services other than Azure Backup, refer to the Microsoft Cloud Security Benchmark (and service baselines) to implement the above controls.

Overview of security features in Azure Backup:
 [https://docs.microsoft.com/azure/backup/security-overview](https://docs.microsoft.com/azure/backup/security-overview)

Encryption of backup data using customer-managed keys:
 [https://docs.microsoft.com/azure/backup/encryption-at-rest-with-cmk](https://docs.microsoft.com/azure/backup/encryption-at-rest-with-cmk)

Security features to help protect hybrid backups from attacks:
 [https://docs.microsoft.com/azure/backup/backup-azure-security-feature#prevent-attacks](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#prevent-attacks)

Azure Backup - set cross region restore
 [https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-cross-region-restore](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-cross-region-restore)

**Recommendation #:** 8

**Category:** Backup and recovery  
**Title:** BR-4: Regularly test backup

Periodically perform data recovery tests of your backup to verify that the backup configurations and availability of the backup data meets the recovery needs as per defined in the RTO (Recovery Time Objective) and RPO (Recovery Point Objective).

**Azure Guidance:**

Periodically perform data recovery tests of your backup to verify that the backup configurations and availability of the backup data meets the recovery needs as defined in the RTO and RPO.

You may need to define your backup recovery test strategy, including the test scope, frequency and method as performing the full recovery test each time can be difficult.

How to recover files from Azure Virtual Machine backup:
 [https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm)

How to restore Key Vault keys in Azure:
 [https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)




---

## Threat #5

**Principle:** Integrity  
**Affected Asset:** All services  
**Threat:** Container images may contain unknown vulnerabilities, security issues and malicious applications.

**Mitigation:**

Only run containers from trusted registries. When using ACR, enable Azure Defender to scan your registry for security vulnerabilities on each pushed container image and expose detailed findings per image.

1. Use container scanning tools within your CI/CD pipelines to detect vulnerabilities earlier.
2. Use Microsoft Defender for Containers to secure clusters, containers, and applications. Also scan your images for vulnerabilities with Microsoft Defender or any other image scanning solution.

**Recommendation #:** 1

**Category:** Posture and Vulnerability Management  
**Title:** PV-5: Perform vulnerability assessments

Perform vulnerabilities assessment for cloud resources at all tiers in a fixed schedule or on-demand. Track and compare the scan results to verify the vulnerabilities are remediated. The assessment should include all type of vulnerabilities, such as vulnerabilities in Azure services, network, web, operating systems, misconfigurations, and so on.

Be aware of the potential risks associated with the privileged access used by the vulnerability scanners. Follow the privileged access security best practice to secure any administrative accounts used for the scanning.

**Azure Guidance:**

Follow recommendations from Microsoft Defender for Cloud for performing vulnerability assessments on your Azure virtual machines, container images, and SQL servers. Microsoft Defender for Cloud has a built-in vulnerability scanner for virtual machines. Use a third-party solution for performing vulnerability assessments on network devices and applications (e.g., web applications)

Export scan results at consistent intervals and compare the results with previous scans to verify that vulnerabilities have been remediated. When using vulnerability management recommendations suggested by Microsoft Defender for Cloud, you can pivot into the selected scan solution's portal to view historical scan data.

When conducting remote scans, do not use a single, perpetual, administrative account. Consider implementing JIT (Just In Time) provisioning methodology for the scan account. Credentials for the scan account should be protected, monitored, and used only for vulnerability scanning.

Note: Microsoft Defender services (including Defender for servers, containers, App Service, Database, and DNS) embed certain vulnerability assessment capabilities. The alerts generated from Azure Defender services should be monitored and reviewed together with the result from Microsoft Defender for Cloud vulnerability scanning tool.

Note: Set up email notifications in Microsoft Defender for Cloud.

How to implement Microsoft Defender for Cloud vulnerability assessment recommendations:  [https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

Integrated vulnerability scanner for virtual machines:
 [https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)

SQL vulnerability assessment:
 [https://docs.microsoft.com/azure/azure-sql/database/sql-vulnerability-assessment](https://docs.microsoft.com/azure/azure-sql/database/sql-vulnerability-assessment)

Exporting Microsoft Defender for Cloud vulnerability scan results:
 [https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)#exporting-results

**Recommendation #:** 2

**Category:** DevOps Security  
**Title:** DS-2: Ensure software supply chain security

Ensure the enterprise’s SDLC (Software Development Lifecycle) or process include a set of security controls to govern the in-house and third-party software components (including both proprietary and open-source software) where applications have dependencies. Define gating criteria to prevent vulnerable or malicious components being integrated and deployed into the environment.

The software supply chain security controls should at least include the following aspects:

Properly manage a Software Bill of Materials (SBOM) by identifying the upstream dependencies required for the service/resource development, build, integration and deployment phase.
Inventory and track the in-house and third-party software components for known vulnerability when there is a fix available in the upstream.
Assess the vulnerabilities and malware in the software components using static and dynamic application testing for unknown vulnerabilities.
Ensure the vulnerabilities and malware are mitigated using the appropriate approach. This may include source code local or upstream fix, feature exclusion and/or applying compensating controls if the direct mitigation is not available.
If closed source third-party components are used in your production environment, you may have limited visibility to its security posture. You should consider additional controls such as access control, network isolation and endpoint security to minimize the impact if there is a malicious activity or vulnerability associated with the component.


**Azure Guidance:**

For the GitHub platform, ensure the software supply chain security through the following capability or tools from GitHub Advanced Security or GitHub’s native feature:- Use Dependency Graph to scan, inventory and identify all your project’s dependencies and related vulnerabilities through Advisory Database.

- Use Dependabot to ensure that the vulnerable dependency is tracked and remediated, and ensure your repository automatically keeps up with the latest releases of the packages and applications it depends on.
- Use GitHub's native code scanning capability to scan the source code when sourcing the code externally.
- Use Microsoft Defender for Cloud to integrate vulnerability assessment for your container image in the CI/CD workflow.
For Azure DevOps, you can use third-party extensions to implement similar controls to inventory, analyze and remediate the third-party software components and their vulnerabilities


GitHub Dependency Graph:
 [https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)

GitHub Dependabot:
 [https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates)

Identify vulnerable container images in your CI/CD workflows:
 [https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd](https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd)

Azure DevOps Marketplace – supply chain security:
 [https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS](https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS)

**Recommendation #:** 3

**Category:** DevOps Security  
**Title:** DS-6: Enforce security of workload throughout DevOps lifecycle

Ensure the workload is secured throughout the entire lifecycle in development, testing, and deployment stage. Use Microsoft Cloud Security Benchmark to evaluate the controls (such as network security, identity management, privileged access and so on) that can be set as guardrails by default or shift left prior to the deployment stage. In particular, ensure the following controls are in place in your DevOps process:
- Automate the deployment by using Azure or third-party tooling in the CI/CD workflow, infrastructure management (infrastructure as code), and testing to reduce human error and attack surface.
- Ensure VMs, container images and other artifacts are secure from malicious manipulation.
- Scan the workload artifacts (in other words, container images, dependencies, SAST and DAST scans) prior to the deployment in the CI/CD workflow
- Deploy vulnerability assessment and threat detection capability into the production environment and continuously use these capabilities in the run-time.

**Azure Guidance:**

Guidance for Azure VMs:
- Use Azure Shared Image Gallery to share and control access to your images by different users, service principals, or AD groups within your organization. Use Azure role-based access control (Azure RBAC) to ensure that only authorized users can access your custom images.
- Define the secure configuration baselines for the VMs to eliminate unnecessary credentials, permissions, and packages. Deploy and enforce configuration baselines through custom images, Azure Resource Manager templates, and/or Azure Policy guest configuration.

Guidance for Azure container services:
- Use Azure Container Registry (ACR) to create your private container registry where granular access can be restricted through Azure RBAC, so only authorized services and accounts can access the containers in the private registry.
- Use Defender for Containers for vulnerability assessment of the images in your private Azure Container Registry. In addition, you can use Microsoft Defender for Cloud to integrate the container image scans as part of your CI/CD workflows.

For Azure serverless services, adopt similar controls to ensure security controls "shift-left" to the stage prior to deployment.  

Shared Image Gallery overview:
 [https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)

How to implement Microsoft Defender for Cloud vulnerability assessment recommendations:  [https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

Security considerations for Azure Container:
 [https://docs.microsoft.com/azure/container-instances/container-instances-image-security](https://docs.microsoft.com/azure/container-instances/container-instances-image-security)

Azure Defender for container registries:
 [https://docs.microsoft.com/azure/security-center/defender-for-container-registries-introduction](https://docs.microsoft.com/azure/security-center/defender-for-container-registries-introduction)




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

**Recommendation #:** 1

**Category:** Network Security  
**Title:** NS-1: Establish network segmentation boundaries

Ensure that your virtual network deployment aligns to the enterprise segmentation strategy defined in the GS-2 security control. Any workload that could incur higher risk for the organization should be in isolated virtual networks.
Examples of high-risk workload include:
- An application storing or processing highly sensitive data.
- An external network-facing application accessible by the public or users outside of your organization.
- An application using insecure architecture or containing vulnerabilities that cannot be easily remediated.

To enhance the enterprise segmentation strategy, restrict or monitor traffic between internal resources using network controls. For specific, well-defined applications (such as a 3-tier app), this can be a highly secure "deny by default, permit by exception" approach by restricting the ports, protocols, source, and destination IPs of the network traffic. If you have many applications and endpoints interacting with each other, blocking traffic may not scale well, and you may only be able to monitor traffic.

**Azure Guidance:**

Create a virtual network (VNet) as a fundamental segmentation approach in your Azure network, so resources such as VMs can be deployed into the VNet within a network boundary. To further segment the network, you can create subnets inside VNet for smaller sub-networks.

Use network security groups (NSG) as a network layer control to restrict or monitor traffic by port, protocol, source IP address, or destination IP address. Refer to NS-7 Simplify network security configuration to use Adaptive Network Hardening to recommend NSG hardening rules based on threat intelligence and traffic analysis result.

You can also use application security groups (ASGs) to simplify complex configuration. Instead of defining policy based on explicit IP addresses in network security groups, ASGs enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups.


Azure Virtual Network concepts and best practices:
 [https://docs.microsoft.com/azure/virtual-network/concepts-and-best-practices](https://docs.microsoft.com/azure/virtual-network/concepts-and-best-practices)

Add, change, or delete a virtual network subnet:
 [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)

How to create a network security group with security rules:
 [https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

Understand and use application security groups:
 [https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups)

**Recommendation #:** 2

**Category:** Network Security  
**Title:** NS-2: Secure cloud native services with network controls

Secure cloud services by establishing private access points for resources. Additionally, disable or restrict access from public networks when possible.

**Azure Guidance:**

Deploy private endpoints for all Azure resources that support the Private Link feature, to establish a private access point for the resources. Using Private Link will keep the private connection from routing through the public network.

Note: Certain Azure services may also allow private communication through the service endpoint feature, though it is recommended to use Azure Private Link for secure and private access to services hosted on Azure platform.

For certain services, you can choose to deploy VNet integration for the service where you can restrict/isolate the VNET to establish a private access point for the service.

You also have the option to configure the service native network ACL rules or simply disable public network access to block access from the public network.

For Azure VMs, unless there is a strong use case, avoid assigning public IPs/subnet directly to the VM interface and instead use gateway or load balancer services as the front-end for access by the public network.


Understand Azure Private Link:
 [https://docs.microsoft.com/azure/private-link/private-link-overview](https://docs.microsoft.com/azure/private-link/private-link-overview)

Integrate Azure services with virtual networks for network isolation:
 [https://learn.microsoft.com/en-us/azure/virtual-network/vnet-integration-for-azure-services](https://learn.microsoft.com/en-us/azure/virtual-network/vnet-integration-for-azure-services)

**Recommendation #:** 3

**Category:** Network Security  
**Title:** NS-3: Deploy firewall at the edge of enterprise network

Deploy a firewall to perform advanced filtering on network traffic to and from external networks. You can also use firewalls between internal segments to support a segmentation strategy. If required, use custom routes for your subnet to override the system route when you need to force the network traffic to go through a network appliance for security control purpose.

At a minimum, block known bad IP addresses and high-risk protocols, such as remote management (for example, RDP and SSH) and intranet protocols (for example, SMB and Kerberos).

**Azure Guidance:**

Use Azure Firewall to provide fully stateful application layer traffic restriction (such as URL filtering) and/or central management over a large number of enterprise segments or spokes (in a hub/spoke topology).  

If you have a complex network topology, such as a hub/spoke setup, you may need to create user-defined routes (UDR) to ensure the traffic goes through the desired route. For example, you have the option to use an UDR to redirect egress internet traffic through a specific Azure Firewall or a network virtual appliance.

How to deploy Azure Firewall:
 [https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

Virtual network traffic routing:
 [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)

**Recommendation #:** 4

**Category:** Network Security  
**Title:** NS-4: Deploy intrusion detection/intrusion prevention systems (IDS/IPS)

Use network intrusion detection and intrusion prevention systems (IDS/IPS) to inspect the network and payload traffic to or from your workload. Ensure that IDS/IPS is always tuned to provide high-quality alerts to your SIEM solution.

For more in-depth host level detection and prevention capability, use host-based IDS/IPS or a host-based endpoint detection and response (EDR) solution in conjunction with the network IDS/IPS.

**Azure Guidance:**

Use Azure Firewall’s IDPS capability to protect your virtual network to alert on and/or block traffic to and from known malicious IP addresses and domains.

For more in-depth host-level detection and prevention capabilities, deploy host-based IDS/IPS or a host-based endpoint detection and response (EDR) solution, such as Microsoft Defender for Endpoint, at the VM level in conjunction with the network IDS/IPS.



Azure Firewall IDPS:
 [https://docs.microsoft.com/azure/firewall/premium-features#idps](https://docs.microsoft.com/azure/firewall/premium-features#idps)

Microsoft Defender for Endpoint capability:  [https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)


**Recommendation #:** 5

**Category:** Network Security  
**Title:** NS-6: Deploy web application firewall

Deploy a web application firewall (WAF) and configure the appropriate rules to protect your web applications and APIs from application-specific attacks.

**Azure Guidance:**

Use web application firewall (WAF) capabilities in Azure Application Gateway, Azure Front Door, and Azure Content Delivery Network (CDN) to protect applications, services and APIs against application layer attacks at the edge of your network.

Set your WAF in "detection" or "prevention mode," depending on your needs and threat landscape.

Choose a built-in ruleset, such as OWASP Top 10 vulnerabilities, and tune it to your application needs.

How to deploy Azure WAF:
 [https://docs.microsoft.com/azure/web-application-firewall/overview](https://docs.microsoft.com/azure/web-application-firewall/overview)



**Recommendation #:** 6

**Category:** Network Security  
**Title:** NS-8: Detect and disable insecure services and  protocols

Detect and disable insecure services and protocols at the OS, application, or software package layer. Deploy compensating controls if disabling insecure services and protocols are not possible.

**Azure Guidance:**

Use Microsoft Sentinel’s built-in Insecure Protocol Workbook to discover the use of insecure services and protocols such as SSL/TLSv1, SSHv1, SMBv1, LM/NTLMv1, wDigest, weak ciphers in Kerberos, and Unsigned LDAP Binds. Disable insecure services and protocols that do not meet the appropriate security standard.

Note: If disabling insecure services or protocols is not possible, use compensating controls such as blocking access to the resources through network security groups, Azure Firewall, or Azure Web Application Firewall to reduce the attack surface.

Azure Sentinel insecure protocols workbook:
 [https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks)

**Recommendation #:** 7

**Category:** Governance and Strategy  
**Title:** GS-4: Define and implement network security strategy

Establish a cloud network security strategy as part of your organization’s overall security strategy for access control. This strategy should include documented guidance, policy, and standards for the following elements:
- Design a centralized/decentralized network management and security responsibility model to deploy and maintain network resources.
- A virtual network segmentation model aligned with the enterprise segmentation strategy.
- An Internet edge and ingress and egress strategy.
- A hybrid cloud and on-premises interconnectivity strategy.
- A network monitoring and logging strategy.
- An up-to-date network security artifacts (such as network diagrams, reference network architecture).

Azure Security Best Practice 11 - Architecture. Single unified security strategy:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy](https://docs.microsoft.com/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

Azure Security Benchmark - Network Security:
 [https://docs.microsoft.com/security/benchmark/azure/security-controls-v3-network-security](https://docs.microsoft.com/security/benchmark/azure/security-controls-v3-network-security)

Azure network security overview:
 [https://docs.microsoft.com/azure/security/fundamentals/network-overview](https://docs.microsoft.com/azure/security/fundamentals/network-overview)

Enterprise network architecture strategy:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/architecture](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)




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

**Recommendation #:** 1

**Category:** Privileged Access  
**Title:** PA-7: Follow just enough administration (least privilege) principle

Follow the just enough administration (least privilege) principle to manage permissions at fine-grained level. Use features such as role-based access control (RBAC) to manage resource access through role assignments.  

**Azure Guidance:**

Use Azure role-based access control (Azure RBAC) to manage Azure resource access through role assignments. Through RBAC, you can assign roles to users, groups, service principals, and managed identities. There are pre-defined built-in roles for certain resources, and these roles can be inventoried or queried through tools such as Azure CLI, Azure PowerShell, and the Azure portal.

The privileges you assign to resources through Azure RBAC should always be limited to what's required by the roles. Limited privileges will complement the just-in-time (JIT) approach of Entra ID Privileged Identity Management (PIM), and those privileges should be reviewed periodically. If required, you can also use PIM to define a time-bound assignment, which is a condition in a role assignment where a user can only activate the role within the specified start and end dates.

Note: Use Azure built-in roles to allocate permissions and only create custom roles when required.

What is Azure role-based access control (Azure RBAC):
 [https://docs.microsoft.com/azure/role-based-access-control/overview](https://docs.microsoft.com/azure/role-based-access-control/overview)

How to configure RBAC in Azure:
 [https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

How to use Entra ID identity and access reviews:
 [https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

Entra ID Privileged Identity Management - Time-bound assignment:
 [https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#what-does-it-do](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#what-does-it-do)




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

**Recommendation #:** 1

**Category:** Identity Management  
**Title:** IM-1: Use centralized identity and authentication system

Use a centralized identity and authentication system to govern your organization's identities and authentications for cloud and non-cloud resources.

**Azure Guidance:**

Azure Active Directory (Entra ID) is Azure's identity and authentication management service. Standardize on Entra ID to govern organizational identity and authentication in:
- Microsoft cloud resources, such as Azure Storage, Azure Virtual Machines (Linux and Windows), Azure Key Vault, PaaS, and SaaS applications.
- Organizational resources, such as applications on Azure, third-party applications running on corporate network resources, and third-party SaaS applications.
- Enterprise identities in Active Directory by synchronization to Entra ID to ensure a consistent and centrally managed identity strategy.

For the Azure services that apply, avoid use of local authentication methods and instead use Azure Active Directory to centralize your service authentications.

Note: As soon as it is technically feasible, migrate on-premises Active Directory-based applications to Entra ID. This could be an Entra ID Enterprise Directory, Business to Business configuration, or Business to consumer configuration.

Tenancy in Entra ID:
 [https://docs.microsoft.com/azure/active-directory/develop/single-and-multi-tenant-apps](https://docs.microsoft.com/azure/active-directory/develop/single-and-multi-tenant-apps)

How to create and configure an Entra ID instance:
 [https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

Define Entra ID tenants:
 [https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)

Use external identity providers for an application:
 [https://docs.microsoft.com/azure/active-directory/b2b/identity-providers](https://docs.microsoft.com/azure/active-directory/b2b/identity-providers)


**Recommendation #:** 2

**Category:** Identity Management  
**Title:** IM-2: Protect identity and authentication systems

Secure your identity and authentication system as a high priority in your organization's cloud security practice. Common security controls include:
- Restrict privileged roles and accounts
- Require strong authentication for all privileged access
- Monitor and audit high risk activities

**Azure Guidance:**

Use the Entra ID security baseline and the Entra ID Identity Secure Score to evaluate your Entra ID identity security posture, and remediate security and configuration gaps.
The Entra ID Identity Secure Score evaluates Entra ID for the following configurations:
- Use limited administrative roles
- Turn on user risk policy
- Designate more than one global admin
- Enable policy to block legacy authentication
- Ensure all users can complete multi-factor authentication for secure access
- Require MFA for administrative roles
- Enable self-service password reset
- Do not expire passwords
- Turn on sign-in risk policy
- Do not allow users to grant consent to unmanaged applications

Use Entra ID Identity Protection to detect, investigate, and remediate identity-based risks. To similarly protect your on-premises Active Directory domain, use Defender for Identity.

Note: Follow published best practices for all other identity components, including your on-premises Active Directory and any third party capabilities, and the infrastructure (such as operating systems, networks, databases) that host them.

What is the identity secure score in Entra ID:  [https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)

Best Practices for Securing Active Directory:
 [https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/best-practices-for-securing-active-directory](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/best-practices-for-securing-active-directory)

What is Identity Protection?
 [https://learn.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection](https://learn.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection)

What is Microsoft Defender for Identity?
 [https://learn.microsoft.com/en-us/defender-for-identity/what-is](https://learn.microsoft.com/en-us/defender-for-identity/what-is)

**Recommendation #:** 3

**Category:** Identity Management  
**Title:** IM-3: Manage application identities securely and automatically

Use managed application identities instead of creating human accounts for applications to access resources and execute code. Managed application identities provide benefits such as reducing the exposure of credentials. Automate the rotation of credentials to ensure the security of the identities.

**Azure Guidance:**

Use Azure managed identities, which can authenticate to Azure services and resources that support Entra ID authentication. Managed identity credentials are fully managed, rotated, and protected by the platform, avoiding hard-coded credentials in source code or configuration files.

For services that don't support managed identities, use Entra ID to create a service principal with restricted permissions at the resource level. It is recommended to configure service principals with certificate credentials and fall back to client secrets for authentication.


Azure managed identities:
 [https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Services that support managed identities for Azure resources:
 [https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

Azure service principal:
 [https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)

Create a service principal with certificates:
 [https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell)

**Recommendation #:** 4

**Category:** Identity Management  
**Title:** IM-4: Authenticate server and services

Authenticate remote servers and services from your client side to ensure you are connecting to trusted server and services. The most common server authentication protocol is Transport Layer Security (TLS), where the client-side (often a browser or client device) verifies the server by verifying the server’s certificate was issued by a trusted certificate authority.

Note: Mutual authentication can be used when both the server and the client authenticate one-another.

**Azure Guidance:**

Many Azure services support TLS authentication by default. For services that don't support this by default or support TLS disabling, ensure it is always enabled to support the server/service authentication. Your client application should also be designed to verify server/service identity (by verifying the server’s certificate issued by a trusted certificate authority) in the handshake stage.

Note: Services such as API Management and API Gateway support TLS mutual authentication.

Enforce Transport Layer Security (TLS) for a storage account:
 [https://docs.microsoft.com/azure/storage/common/transport-layer-security-configure-minimum-version?tabs=portal#use-azure-policy-to-enforce-the-minimum-tls-version](https://docs.microsoft.com/azure/storage/common/transport-layer-security-configure-minimum-version?tabs=portal#use-azure-policy-to-enforce-the-minimum-tls-version)

**Recommendation #:** 5

**Category:** Identity Management  
**Title:** IM-5: Use single sign-on (SSO) for application access

Use single sign-on (SSO) to simplify the user experience for authenticating to resources including applications and data across cloud services and on-premises environments.

**Azure Guidance:**

Use Entra ID for workload application workload access (customer facing)  through Entra ID single sign-on (SSO), reducing the need for duplicate accounts. Entra ID provides identity and access management to Azure resources (in the management plane including CLI, PowerShell, portal), cloud applications, and on-premises applications.

Entra ID also supports SSO for enterprise identities such as corporate user identities, as well as external user identities from trusted third-party and public users.

Understand application SSO with Entra ID:
 [https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Recommendation #:** 6

**Category:** Identity Management  
**Title:** IM-6: Use strong authentication controls

Enforce strong authentication controls (strong passwordless authentication or multi-factor authentication) with your centralized identity and authentication management system for all access to resources. Authentication based on password credentials alone is considered legacy, as it is insecure and does not stand up to popular attack methods.

When deploying strong authentication, configure administrators and privileged users first, to ensure the highest level of the strong authentication method, quickly followed by rolling out the appropriate strong authentication policy to all users.

Note: If legacy password-based authentication is required for legacy applications and scenarios, ensure password security best practices such as complexity requirements, are followed.

**Azure Guidance:**

Entra ID supports strong authentication controls through passwordless methods and multi-factor authentication (MFA).
- Passwordless authentication: Use passwordless authentication as your default authentication method. There are three options available in passwordless authentication: Windows Hello for Business, Microsoft Authenticator app phone sign-in, and FIDO2 security keys. In addition, customers can use on-premises authentication methods such as smart cards.
- Multi-factor authentication: Azure MFA can be enforced on all users, select users, or at the per-user level based on sign-in conditions and risk factors. Enable Azure MFA and follow Microsoft Defender for Cloud identity and access management recommendations for MFA setup.

If legacy password-based authentication is still used for Entra ID authentication, be aware that cloud-only accounts (user accounts created directly in Azure) have a default baseline password policy. And hybrid accounts (user accounts that come from on-premises Active Directory) follow the on-premises password policies.

For third-party applications and services that may have default IDs and passwords, disable or change them during initial service setup.

How to enable MFA in Azure:
 [https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

Introduction to passwordless authentication options for Azure Active Directory:
 [https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)

Entra ID default password policy:
 [https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts)

Eliminate bad passwords using Entra ID Password Protection:  [https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad)

Block legacy authentication:
 [https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication)

**Recommendation #:** 7

**Category:** Identity Management  
**Title:** IM-7: Restrict resource access based on  conditions

Explicitly validate trusted signals to allow or deny user access to resources, as part of a zero-trust access model. Signals to validate should include strong authentication of user account, behavioral analytics of user account, device trustworthiness, user or group membership, locations and so on.

**Azure Guidance:**

Use Entra ID conditional access for more granular access controls based on user-defined conditions, such as requiring user logins from certain IP ranges (or devices) to use MFA. Entra ID Conditional Access allows you to enforce access controls on your organization’s apps based on certain conditions.

Define the applicable conditions and criteria for Entra ID conditional access in the workload. Consider the following common use cases:
- Requiring multi-factor authentication for users with administrative roles
- Requiring multi-factor authentication for Azure management tasks
- Blocking sign-ins for users attempting to use legacy authentication protocols
- Requiring trusted locations for Entra ID Multi-Factor Authentication registration
- Blocking or granting access from specific locations
- Blocking risky sign-in behaviors
- Requiring organization-managed devices for specific applications

Note: Granular authentication session management controls can also be implemented through Entra ID conditional access policies such as sign-in frequency and persistent browser session.

Azure Conditional Access overview:
 [https://docs.microsoft.com/azure/active-directory/conditional-access/overview](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

Common Conditional Access policies:
 [https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common)

Conditional Access insights and reporting:
 [https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting)

Configure authentication session management with Conditional Access:
 [https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime)

**Recommendation #:** 8

**Category:** Identity Management  
**Title:** IM-8: Restrict the exposure of credential and secrets

Ensure that application developers securely handle credentials and secrets:
- Avoid embedding the credentials and secrets into the code and configuration files
- Use key vault or a secure key store service to store the credentials and secrets
- Scan for credentials in source code.  

Note: This is often governed and enforced through a secure software development lifecycle (SDLC) and DevOps security process

**Azure Guidance:**

When using a managed identity is not an option, ensure that secrets and credentials are stored in secure locations such as Azure Key Vault, instead of embedding them into the code and configuration files.

If you use Azure DevOps and GitHub for your code management platform:
- Implement Azure DevOps Credential Scanner to identify credentials within the code.
- For GitHub, use the native secret scanning feature to identify credentials or other forms of secrets within the code.

Clients such as Azure Functions, Azure Apps services, and VMs can use managed identities to access Azure Key Vault securely. See Data Protection controls related to the use of Azure Key Vault for secrets management.

Note: Azure Key Vault provides automatic rotation for supported services. For secrets which cannot be automatically rotated, ensure they are manually rotated periodically and purged when no longer in use.

How to setup Credential Scanner:
 [https://secdevtools.azurewebsites.net/helpcredscan.html](https://secdevtools.azurewebsites.net/helpcredscan.html)

GitHub secret scanning:
 [https://docs.github.com/github/administering-a-repository/about-secret-scanning](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Recommendation #:** 9

**Category:** Identity Management  
**Title:** IM-9: Secure user access to  existing applications

In a hybrid environment, where you have on-premises applications or non-native cloud applications using legacy authentication, consider solutions such as cloud access security broker (CASB), application proxy, single sign-on (SSO)  to govern the access to these applications for the following benefits:
- Enforce a centralized strong authentication
- Monitor and control risky end-user activities
- Monitor and remediate risky legacy applications activities
- Detect and prevent sensitive data transmission

**Azure Guidance:**

Protect your on-premises and non-native cloud applications using legacy authentication by connecting them to:
- Entra ID Application Proxy and configure header-based authentication to allow single sign-on (SSO) access to the applications for remote users  while explicitly validating the trustworthiness of both remote users and devices with Entra ID Conditional Access. If required, use a third-party Software-Defined Perimeter (SDP) solution which can offer similar functionality.
- Microsoft Defender for Cloud Apps which serves as a cloud access security broker (CASB) service to monitor and block user access to unapproved third-party SaaS applications.
- Your existing third-party application delivery controllers and networks.

Note: VPNs are commonly used to access legacy applications and often only have basic access control and limited session monitoring.

Entra ID Application Proxy:
 [https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy#what-is-application-proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy#what-is-application-proxy)

Microsoft Cloud App Security best practices:
 [https://docs.microsoft.com/cloud-app-security/best-practices](https://docs.microsoft.com/cloud-app-security/best-practices)

Entra ID secure hybrid access:
 [https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)


**Recommendation #:** 10

**Category:** Privileged Access  
**Title:** PA-1: Separate and limit highly privileged/administrative users

Identify all high business impact accounts. Limit the number of privileged/administrative accounts in cloud control plane, management plane and data/workload plane.

**Azure Guidance:**

You must secure all roles with direct or indirect administrative access to Azure hosted resources.

Azure Active Directory (Entra ID) is Azure's default identity and access management service. The most critical built-in roles in Entra ID are Global Administrator and Privileged Role Administrator, because users assigned to these two roles can delegate administrator roles. With these privileges, users can directly or indirectly read and modify every resource in your Azure environment:
- Global Administrator / Company Administrator: Users with this role have access to all administrative features in Entra ID as well as services that use Entra ID identities.
- Privileged Role Administrator: Users with this role can manage role assignments in Entra ID, as well as within Entra ID Privileged Identity Management (PIM). In addition, this role allows management of all aspects of PIM and administrative units.

Outside of Entra ID, Azure has built-in roles that can be critical for privileged access at the resource level.
- Owner: Grants full access to manage all resources, including the ability to assign roles in Azure RBAC.
- Contributor: Grants full access to manage all resources, but does not allow you to assign roles in Azure RBAC, manage assignments in Azure Blueprints, or share image galleries.
- User Access Administrator: Lets you manage user access to Azure resources.
Note: You may have other critical roles that need to be governed if you use custom roles in the Entra ID level or resource level with certain privileged permissions assigned.

In addition, users with the following three roles in Azure Enterprise Agreement (EA) portal should also be restricted as they can be used to directly or indirectly manage Azure subscriptions.
- Account Owner: Users with this role can manage subscriptions, including the creation and deletion of subscriptions.
- Enterprise Administrator: Users assigned with this role can manage (EA) portal users.
- Department Administrator: Users assigned with this role can change account owners within the department.

Lastly, ensure that you also restrict privileged accounts in other management, identity, and security systems that have administrative access to your business-critical assets, such as Active Directory Domain Controllers (DCs), security tools, and system management tools with agents installed on business-critical systems. Attackers who compromise these management and security systems can immediately weaponize them to compromise business critical assets.

Administrator role permissions in Entra ID:
 [https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

Use Azure Privileged Identity Management security alerts:
 [https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-configure-security-alerts](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-configure-security-alerts)

Securing privileged access for hybrid and cloud deployments in Entra ID:
 [https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

**Recommendation #:** 11

**Category:** Privileged Access  
**Title:** PA-2: Avoid standing access for user accounts and permissions

Instead of creating standing privileges, use just-in-time (JIT) mechanism to assign privileged access to the different resource tiers.

**Azure Guidance:**

Enable just-in-time (JIT) privileged access to Azure resources and Entra ID using Entra ID Privileged Identity Management (PIM). JIT is a model in which users receive temporary permissions to perform privileged tasks, which prevents malicious or unauthorized users from gaining access after the permissions have expired. Access is granted only when users need it. PIM can also generate security alerts when there is suspicious or unsafe activity in your Entra ID organization.

Restrict inbound traffic to your sensitive virtual machines (VM) management ports with Microsoft Defender for Cloud's just-in-time (JIT) for VM access feature. This ensures privileged access to the VM is granted only when users need it.

Azure PIM just-in-time access deployment:
 [https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

Understanding just-in-time (JIT) VM access:
 [https://learn.microsoft.com/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks](https://learn.microsoft.com/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks)

**Recommendation #:** 12

**Category:** Privileged Access  
**Title:** PA-3: Manage lifecycle of identities and entitlements

Use an automated process or technical control to manage the identity and access lifecycle including the request, review, approval, provision, and deprovision.

**Azure Guidance:**

Use Entra ID entitlement management features to automate access request workflows (for Azure resource groups). This enables workflows for Azure resource groups to manage access assignments, reviews, expiration, and dual or multi-stage approval.

Use Permissions Management to detect, automatically right-size, and continuously monitor unused and excessive permissions assigned to user and workload identities across multi-cloud infrastructures.

What are Entra ID access reviews:
 [https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

What is Entra ID entitlement management:
 [https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview)

Overview of Permissions Management:
 [https://learn.microsoft.com/azure/active-directory/cloud-infrastructure-entitlement-management/overview](https://learn.microsoft.com/azure/active-directory/cloud-infrastructure-entitlement-management/overview)

**Recommendation #:** 13

**Category:** Privileged Access  
**Title:** PA-4: Review and reconcile user access regularly

Conduct regular review of privileged account entitlements. Ensure the access granted to the accounts are valid for administration of control plane, management plane, and workloads.

**Azure Guidance:**

Review all privileged accounts and the access entitlements in Azure including Azure tenants, Azure services, VM/IaaS, CI/CD processes, and enterprise management and security tools.

Use Entra ID access reviews to review Entra ID roles, Azure resource access roles, group memberships, and access to enterprise applications. Entra ID reporting can also provide logs to help discover stale accounts, or accounts which have not been used for certain amount of time.

In addition, Entra ID Privileged Identity Management can be configured to alert when an excessive number of administrator accounts are created for a specific role, and to identify administrator accounts that are stale or improperly configured.

Create an access review of Azure resource roles in Privileged Identity Management (PIM):
 [https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-start-access-review](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-start-access-review)

How to use Entra ID identity and access reviews:
 [https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)


**Recommendation #:** 14

**Category:** Privileged Access  
**Title:** PA-5: Set up emergency access

Set up emergency access to ensure that you are not accidentally locked out of your critical cloud infrastructure (such as your identity and access management system) in an emergency.

Emergency access accounts should be rarely used and can be highly damaging to the organization if compromised, but their availability to the organization is also critically important for the few scenarios when they are required.

**Azure Guidance:**

To prevent being accidentally locked out of your Entra ID organization, set up an emergency access account (e.g., an account with Global Administrator role) for access when normal administrative accounts cannot be used. Emergency access accounts are usually highly privileged, and they should not be assigned to specific individuals. Emergency access accounts are limited to emergency or "break glass"' scenarios where normal administrative accounts can't be used.

Ensure that the credentials (such as password, certificate, or smart card) for emergency access accounts are kept secure and known only to individuals who are authorized to use them only in an emergency. Additional controls may also be used, such dual controls (e.g., splitting the credential into two pieces and giving it to separate persons) to enhance the security of this process. Monitor the sign-in and audit logs to ensure that emergency access accounts are only used when authorized.

Manage emergency access accounts in Entra ID:
 [https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)


**Recommendation #:** 15

**Category:** Privileged Access  
**Title:** PA-6: Use privileged access workstations / channel for administrative tasks

Secured, isolated workstations are critically important for the security of sensitive roles like administrator, developer, and critical service operator.  

**Azure Guidance:**

Use Azure Active Directory, Microsoft Defender, and/or Microsoft Intune to deploy privileged access workstations (PAW) on-premises or in Azure for privileged tasks. The PAW should be centrally managed to enforce secured configuration, including strong authentication, software and hardware baselines, and restricted logical and network access.

You may also use Azure Bastion which is a fully platform-managed PaaS service that can be provisioned inside your virtual network. Azure Bastion allows RDP/SSH connectivity to your virtual machines directly from the Azure portal using a web browser.

Understand privileged access workstations:
 [https://learn.microsoft.com/en-us/security/privileged-access-workstations/overview](https://learn.microsoft.com/en-us/security/privileged-access-workstations/overview)

Privileged access workstations deployment:
 [https://docs.microsoft.com/security/compass/privileged-access-deploymenthttps](https://docs.microsoft.com/security/compass/privileged-access-deploymenthttps)

**Recommendation #:** 16

**Category:** Privileged Access  
**Title:** PA-7: Follow just enough administration (least privilege) principle

Follow the just enough administration (least privilege) principle to manage permissions at fine-grained level. Use features such as role-based access control (RBAC) to manage resource access through role assignments.  

**Azure Guidance:**

Use Azure role-based access control (Azure RBAC) to manage Azure resource access through role assignments. Through RBAC, you can assign roles to users, groups, service principals, and managed identities. There are pre-defined built-in roles for certain resources, and these roles can be inventoried or queried through tools such as Azure CLI, Azure PowerShell, and the Azure portal.

The privileges you assign to resources through Azure RBAC should always be limited to what's required by the roles. Limited privileges will complement the just-in-time (JIT) approach of Entra ID Privileged Identity Management (PIM), and those privileges should be reviewed periodically. If required, you can also use PIM to define a time-bound assignment, which is a condition in a role assignment where a user can only activate the role within the specified start and end dates.

Note: Use Azure built-in roles to allocate permissions and only create custom roles when required.

What is Azure role-based access control (Azure RBAC):
 [https://docs.microsoft.com/azure/role-based-access-control/overview](https://docs.microsoft.com/azure/role-based-access-control/overview)

How to configure RBAC in Azure:
 [https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

How to use Entra ID identity and access reviews:
 [https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

Entra ID Privileged Identity Management - Time-bound assignment:
 [https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#what-does-it-do](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#what-does-it-do)

**Recommendation #:** 17

**Category:** Privileged Access  
**Title:** PA-8: Determine access process for cloud provider support



Establish an approval process and access path for requesting and approving vendor support request and temporary access to your data through a secure channel.

**Azure Guidance:**

In support scenarios where Microsoft needs to access your data, use Customer Lockbox to review and either approve or reject each data access request made by Microsoft.

Understand Customer Lockbox:
 [https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

**Recommendation #:** 18

**Category:** Governance and Strategy  
**Title:** GS-6: Define and implement identity and privileged access strategy

Establish a cloud identity and privileged access approach as part of your organization’s overall security access control strategy. This strategy should include documented guidance, policy, and standards for the following aspects:
- Centralized identity and authentication system (such as Entra ID) and its interconnectivity with other internal and external identity systems
- Privileged identity and access governance (such as access request, review and approval)
- Privileged accounts in emergency (break-glass) situation
- Strong authentication (passwordless authentication and multifactor authentication) methods in different use cases and conditions
- Secure access by administrative operations through web portal/console, command-line and API.

For exception cases, where an enterprise system isn’t used, ensure adequate security controls are in place for identity, authentication and access management, and governed. These exceptions should be approved and periodically reviewed by the enterprise team. These exceptions are typically in cases such as:
- Use of a non-enterprise designated identity and authentication system, such as cloud-based third-party systems (may introduce unknown risks)
- Privileged users authenticated locally and/or use non-strong authentication methods

Azure Security Benchmark - Identity management:
 [https://docs.microsoft.com//security/benchmark/azure/security-controls-v3-identity-management](https://docs.microsoft.com//security/benchmark/azure/security-controls-v3-identity-management)

Azure Security Benchmark - Privileged access:
 [https://docs.microsoft.com/security/benchmark/azure/security-controls-v3-privileged-access](https://docs.microsoft.com/security/benchmark/azure/security-controls-v3-privileged-access)

Azure Security Best Practice 11 - Architecture. Single unified security strategy:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy](https://docs.microsoft.com/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

Azure identity management security overview:
 [https://docs.microsoft.com/azure/security/fundamentals/identity-management-overview](https://docs.microsoft.com/azure/security/fundamentals/identity-management-overview)




---

## Threat #9

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Inbound Internet connections  
**Threat:** As a result of the vulnerability of not encrypting data, plaintext data could be intercepted during transit via a man-in-the-middle (MitM), downgrade or cross-protocol attacks attack. Sensitive data could be exposed or tampered to allow further exploits.

**Mitigation:**

All products and services must encrypt data in transit using approved cryptographic protocols and algorithms.

1. Use TLS to encrypt all HTTP-based network traffic. Use other mechanisms, such as IPSec, to encrypt non-HTTP network traffic that contains customer or confidential data.
2. Use only TLS 1.2 or TLS 1.3. Use ECDHE-based ciphers suites and NIST curves. Use strong keys. Enable HTTP Strict Transport Security (HSTS). Turn off TLS compression and do not use ticket-based session resumption.

**Recommendation #:** 1

**Category:** Data Protection  
**Title:** DP-3: Encrypt sensitive data in transit

Protect the data in transit against 'out of band' attacks (such as traffic capture) using encryption to ensure that attackers cannot easily read or modify the data.

Set the network boundary and service scope where data in transit encryption is mandatory inside and outside of the network. While this is optional for traffic on private networks, this is critical for traffic on external and public networks.

**Azure Guidance:**

Enforce secure transfer in services such as Azure Storage, where a native data in transit encryption feature is built in.

Enforce HTTPS for web application workloads and services by ensuring that any clients connecting to your Azure resources use transport layer security (TLS) v1.2 or later. For remote management of VMs, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol.

For remote management of Azure virtual machines, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol. For secure file transfer, use the SFTP/FTPS service in Azure Storage Blob, App Service apps, and Function apps, instead of using the regular FTP service.

Note: Data in transit encryption is enabled for all Azure traffic traveling between Azure datacenters. TLS v1.2 or later is enabled on most Azure services by default. And some services such as Azure Storage and Application Gateway can enforce TLS v1.2 or later on the server side.


Double encryption for Azure data in transit:
 [https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit)

Understand encryption in transit with Azure:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

Information on TLS Security:
 [https://docs.microsoft.com/security/engineering/solving-tls1-problem](https://docs.microsoft.com/security/engineering/solving-tls1-problem)

Enforce secure transfer in Azure storage:
 [https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account)




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

**Recommendation #:** 1

**Category:** Logging and threat detection  
**Title:** LT-1: Enable threat detection capabilities

To support threat detection scenarios, monitor all known resource types for known and expected threats and anomalies. Configure your alert filtering and analytics rules to extract high-quality alerts from log data, agents, or other data sources to reduce false positives.

**Azure Guidance:**

Use the threat detection capability of Microsoft Defender for Cloud for the respective Azure services.

For threat detection not included in Microsoft Defender services, refer to Microsoft Cloud Security Benchmark service baselines for the respective services to enable the threat detection or security alert capabilities within the service. Ingest alerts and log data from Microsoft Defender for Cloud, Microsoft 365 Defender, and log data from other resources into your Azure Monitor or Microsoft Sentinel instances to build analytics rules, which hunt detect threats and create alerts that match specific criteria across your environment.

For Operational Technology (OT) environments that include computers that control or monitor Industrial Control System (ICS) or Supervisory Control and Data Acquisition (SCADA) resources, use Microsoft Defender for IoT to inventory assets and detect threats and vulnerabilities.

For services that do not have a native threat detection capability, consider collecting the data plane logs and analyze the threats through Microsoft Sentinel.

Introduction to Microsoft Defender for Cloud:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction)

Microsoft Defender for Cloud security alerts reference guide:
 [https://docs.microsoft.com/azure/security-center/alerts-reference](https://docs.microsoft.com/azure/security-center/alerts-reference)

Create custom analytics rules to detect threats:
 [https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats-custom](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats-custom)

Threat indicators for cyber threat intelligence in Microsoft Sentinel:
 [https://docs.microsoft.com/azure/architecture/example-scenario/data/sentinel-threat-intelligence](https://docs.microsoft.com/azure/architecture/example-scenario/data/sentinel-threat-intelligence)


**Recommendation #:** 2

**Category:** Logging and threat detection  
**Title:** LT-2: Enable threat detection for  identity and access management

Detect threats for identities and access management by monitoring the user and application sign-in and access anomalies. Behavioral patterns such as excessive number of failed login attempts, and deprecated accounts in the subscription, should be alerted.

**Azure Guidance:**

Entra ID provides the following logs that can be viewed in Entra ID reporting or integrated with Azure Monitor, Microsoft Sentinel or other SIEM/monitoring tools for more sophisticated monitoring and analytics use cases:
- Sign-ins: The sign-ins report provides information about the usage of managed applications and user sign-in activities.
- Audit logs: Provides traceability through logs for all changes done by various features within Entra ID. Examples of audit logs include changes made to any resources within Entra ID like adding or removing users, apps, groups, roles and policies.
- Risky sign-ins: A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.
- Users flagged for risk: A risky user is an indicator for a user account that might have been compromised.

Entra ID also provides an Identity Protection module to detect and remediate risks related to user accounts and sign-in behaviors. Examples of risks include leaked credentials, sign-in from anonymous or malware linked IP addresses, password spray. The policies in Entra ID Identity Protection allow you to enforce risk-based MFA authentication in conjunction with Azure Conditional Access on user accounts.

In addition, Microsoft Defender for Cloud can be configured to alert on deprecated accounts in the subscription and suspicious activities such as an excessive number of failed authentication attempts. In addition to the basic security hygiene monitoring, Microsoft Defender for Cloud's Threat Protection module can also collect more in-depth security alerts from individual Azure compute resources (such as virtual machines, containers, app service), data resources (such as SQL DB and storage), and Azure service layers. This capability allows you to see account anomalies inside the individual resources.

Note: If you are connecting your on-premises Active Directory for synchronization, use the Microsoft Defender for Identity solution to consume your on-premises Active Directory signals to identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.

Audit activity reports in Entra ID:
 [https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs)

Enable Azure Identity Protection:
 [https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

Threat protection in Microsoft Defender for Cloud:
 [https://docs.microsoft.com/azure/security-center/threat-protection](https://docs.microsoft.com/azure/security-center/threat-protection)

Overview of Microsoft Defender for Identity:
 [https://learn.microsoft.com/defender-for-identity/what-is](https://learn.microsoft.com/defender-for-identity/what-is)

**Recommendation #:** 3

**Category:** Logging and threat detection  
**Title:** LT-3: Enable logging for security investigation

Enable logging for your cloud resources to meet the requirements for security incident investigations and security response and compliance purposes.

**Azure Guidance:**

Enable logging capability for resources at the different tiers, such as logs for Azure resources, operating systems and applications inside in your VMs and other log types.

Be mindful about different types of logs for security, audit, and other operational logs at the management/control plane and data plane tiers. There are three types of the logs available at the Azure platform:
- Azure resource log: Logging of operations that are performed within an Azure resource (the data plane). For example, getting a secret from a key vault or making a request to a database. The content of resource logs varies by the Azure service and resource type.
- Azure activity log: Logging of operations on each Azure resource at the subscription layer, from the outside (the management plane). You can use the Activity Log to determine what, who, and when for any write operations (PUT, POST, DELETE) taken on the resources in your subscription. There is a single Activity log for each Azure subscription.
- Azure Active Directory logs: Logs of the history of sign-in activity and audit trail of changes made in the Azure Active Directory for a particular tenant.

You can also use Microsoft Defender for Cloud and Azure Policy to enable resource logs and log data collecting on Azure resources.

Understand logging and different log types in Azure:
 [https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

Understand Microsoft Defender for Cloud data collection:
 [https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

Enable and configure antimalware monitoring:
 [https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets](https://docs.microsoft.com/azure/security/fundamentals/antimalware#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets)

Operating systems and application logs inside in your compute resources:
 [https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest](https://docs.microsoft.com/azure/azure-monitor/agents/data-sources#operating-system-guest)

**Recommendation #:** 4

**Category:** Logging and threat detection  
**Title:** LT-4: Enable network logging for security investigation


Enable logging for your network services to support network-related incident investigations, threat hunting, and security alert generation. The network logs may include logs from network services such as IP filtering, network and application firewall, DNS, flow monitoring and so on.

**Azure Guidance:**

Enable and collect network security group (NSG) resource logs, NSG flow logs, Azure Firewall logs, and Web Application Firewall (WAF) logs, and logs from virtual machines via the network traffic data collection agent for security analysis to support incident investigations, and security alert generation. You can send the flow logs to an Azure Monitor Log Analytics workspace and then use Traffic Analytics to provide insights.

Collect DNS query logs to assist in correlating other network data.

How to enable network security group flow logs:
 [https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

Azure Firewall logs and metrics:
 [https://docs.microsoft.com/azure/firewall/logs-and-metrics](https://docs.microsoft.com/azure/firewall/logs-and-metrics)

Azure networking monitoring solutions in Azure Monitor:
 [https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics)

Gather insights about your DNS infrastructure with the DNS Analytics solution:
 [https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)



**Recommendation #:** 5

**Category:** Logging and threat detection  
**Title:** LT-5: Centralize security log management and analysis

Centralize logging storage and analysis to enable correlation across log data. For each log source, ensure that you have assigned a data owner, access guidance, storage location, what tools are used to process and access the data, and data retention requirements.

Use Cloud native SIEM if you don't have an existing SIEM solution for CSPs. or aggregate logs/alerts into your existing SIEM.

**Azure Guidance:**

Ensure that you are integrating Azure activity logs into a centralized Log Analytics workspace. Use Azure Monitor to query and perform analytics and create alert rules using the logs aggregated from Azure services, endpoint devices, network resources, and other security systems.

In addition, enable and onboard data to Microsoft Sentinel which provides security information event management (SIEM) and security orchestration automated response (SOAR) capabilities.

How to collect platform logs and metrics with Azure Monitor:
 [https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

How to onboard Azure Sentinel:
 [https://docs.microsoft.com/azure/sentinel/quickstart-onboard](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Recommendation #:** 6

**Category:** Logging and threat detection  
**Title:** LT-6: Configure log storage retention

Plan your log retention strategy according to your compliance, regulation, and business requirements. Configure the log retention policy at the individual logging services to ensure the logs are archived appropriately.

**Azure Guidance:**

Logs such as Azure Activity Logs are retained for 90 days and then deleted. Create a diagnostic setting and route the logs to another location (such as Azure Monitor Log Analytics workspace, Event Hubs or Azure Storage) based on requirements. This strategy also applies to other resource logs and resources managed by the organization such as logs in the operating systems and applications inside VMs.

The available log retention options are:

- Use Azure Monitor Log Analytics workspace for a log retention period of up to 1 year or per response team requirements.
- Use Azure Storage, Data Explorer or Data Lake for long-term and archival storage for greater than 1 year and to meet security compliance requirements.
- Use Azure Event Hubs to forward logs to an external resource outside of Azure.

Note: Microsoft Sentinel uses Log Analytics workspace as its backend for log storage. Consider a long-term storage strategy when planning to retain SIEM logs for extended periods.

Change the data retention period in Log Analytics:
 [https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

How to configure retention policy for Azure Storage account logs:
 [https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

Microsoft Defender for Cloud alerts and recommendations export:  [https://docs.microsoft.com/azure/security-center/continuous-export](https://docs.microsoft.com/azure/security-center/continuous-export)

**Recommendation #:** 7

**Category:** Logging and threat detection  
**Title:** LT-7: Use approved time synchronization sources

Use approved time synchronization sources for logging time stamps which include date, time and time zone information.

**Azure Guidance:**

Microsoft maintains time sources for most Azure PaaS and SaaS services. For compute resources operating systems, use a Microsoft default NTP server for time synchronization unless there is a specific requirement. If there is a need to set up a custom network time protocol (NTP) server, ensure the UDP service port 123 is secured.

All logs generated by resources within Azure provide time stamps with the time zone specified by default.

How to configure time synchronization for Azure Windows compute resources:
 [https://docs.microsoft.com/azure/virtual-machines/windows/time-sync](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync)

How to configure time synchronization for Azure Linux compute resources:  
 [https://docs.microsoft.com/azure/virtual-machines/linux/time-sync](https://docs.microsoft.com/azure/virtual-machines/linux/time-sync)

How to disable inbound UDP for Azure services:
 [https://support.microsoft.com/help/4558520/how-to-disable-inbound-udp-for-azure-services](https://support.microsoft.com/help/4558520/how-to-disable-inbound-udp-for-azure-services)

**Recommendation #:** 8

**Category:** DevOps Security  
**Title:** DS-7: Enable logging and monitoring in DevOps

Ensure your logging and monitoring scope includes non-production environments and CI/CD workflow elements used in DevOps (and any other development processes). The vulnerabilities and threats targeting these environments can introduce significant risks to your production environment if they are not monitored properly. The events from the CI/CD build, test and deployment workflow should also be monitored to identify any deviations in the CI/CD workflow jobs.

Follow Microsoft Cloud Security Benchmark – Logging and Threat Detection as the guideline to implement your logging and monitoring controls for workload.

**Azure Guidance:**

Enable and configure the audit logging capabilities in non-production and CI/CD tooling environments (such as Azure DevOps and GitHub) used throughout the DevOps process.

The events generated from Azure DevOps and the GitHub CI/CD workflow, including the build, test and deployment jobs, should also be monitored to identify any anomalous results.

Ingest the above logs and events into Microsoft Sentinel or other SIEM tools through a logging stream or API to ensure the security incidents are properly monitored and triaged for handling.

Azure DevOps - audit streaming:
 [https://docs.microsoft.com/azure/devops/organizations/audit/auditing-streaming?view=azure-devops](https://docs.microsoft.com/azure/devops/organizations/audit/auditing-streaming?view=azure-devops)

GitHub logging:
 [https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization)

**Recommendation #:** 9

**Category:** Governance and Strategy  
**Title:** GS-7: Define and implement logging, threat detection and incident response strategy

Establish a logging, threat detection and incident response strategy to rapidly detect and remediate threats and meet compliance requirements. Security operations (SecOps / SOC) team should prioritize high quality alerts and seamless experiences so that they can focus on threats rather than log integration and manual steps.
This strategy should include documented policy, procedure and standards for the following aspects:
- The security operations (SecOps) organization's role and responsibilities
- A well-defined and regularly tested incident response plan and handling process aligning with NIST SP 800-61 (Computer Security Incident Handling Guide) or other industry frameworks.
- Communication and notification plan with your customers, suppliers, and public parties of interest.
- Simulate both expected and unexpected security events within your cloud environment to understand the effectiveness of your preparation. Iterate on the outcome of your simulation to improve the scale of your response posture, reduce time to value, and further reduce risk.
- Preference of using extended detection and response (XDR) capabilities such as Azure Defender capabilities to detect threats in the various areas.
- Use of cloud native capability (e.g., as Microsoft Defender for Cloud) and third-party platforms for incident handling, such as logging and threat detection, forensics, and attack remediation and eradication.
- Prepare the necessary runbooks, both manual and automated, to ensure reliable and consistent responses.
- Define key scenarios (such as threat detection, incident response, and compliance) and set up log capture and retention to meet the scenario requirements.
- Centralized visibility of and correlation information about threats, using SIEM, native cloud threat detection capability, and other sources.
- Post-incident activities, such as lessons learned and evidence retention.

Azure Security Benchmark - Logging and threat detection:
 [https://docs.microsoft.com/azure/security/benchmarks/security-benchmark-v3-logging-threat-detection](https://docs.microsoft.com/azure/security/benchmarks/security-benchmark-v3-logging-threat-detection)

Azure Security Benchmark - Incident response:
 [https://docs.microsoft.com/azure/security/benchmarks/security-benchmark-v3-incident-response](https://docs.microsoft.com/azure/security/benchmarks/security-benchmark-v3-incident-response)

Azure Security Best Practice 4 - Process. Update Incident Response Processes for Cloud:
 [https://aka.ms/AzSec4](https://aka.ms/AzSec4)

Entra IDoption Framework, logging, and reporting decision guide:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

Azure enterprise scale, management, and monitoring:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

NIST SP 800-61 Computer Security Incident Handling Guide:
 [https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)




---

## Threat #11

**Principle:** Integrity  
**Affected Asset:** All services  
**Threat:** Sending malicious or unnecessary traffic to Azure resources can lead to a compromise of that resource or create a vector for the attacker to bypass other security controls.

**Mitigation:**

Use and configure firewalls to protect against malicious incoming and outgoing network traffic.

1. Use Application Gateway before publicly accessible Azure Services and enable the Web App Firewall (WAF). By default, the WAF will use Azure-managed rules such as the OWASP ruleset. Custom rules should be defined if the default rules do not meet the customer’s needs.
2. Turn on DDoS protection feature.

**Recommendation #:** 1

**Category:** Network Security  
**Title:** NS-6: Deploy web application firewall

Deploy a web application firewall (WAF) and configure the appropriate rules to protect your web applications and APIs from application-specific attacks.

**Azure Guidance:**

Use web application firewall (WAF) capabilities in Azure Application Gateway, Azure Front Door, and Azure Content Delivery Network (CDN) to protect applications, services and APIs against application layer attacks at the edge of your network.

Set your WAF in "detection" or "prevention mode," depending on your needs and threat landscape.

Choose a built-in ruleset, such as OWASP Top 10 vulnerabilities, and tune it to your application needs.

How to deploy Azure WAF:
 [https://docs.microsoft.com/azure/web-application-firewall/overview](https://docs.microsoft.com/azure/web-application-firewall/overview)



**Recommendation #:** 2

**Category:** Network Security  
**Title:** NS-3: Deploy firewall at the edge of enterprise network

Deploy a firewall to perform advanced filtering on network traffic to and from external networks. You can also use firewalls between internal segments to support a segmentation strategy. If required, use custom routes for your subnet to override the system route when you need to force the network traffic to go through a network appliance for security control purpose.

At a minimum, block known bad IP addresses and high-risk protocols, such as remote management (for example, RDP and SSH) and intranet protocols (for example, SMB and Kerberos).

**Azure Guidance:**

Use Azure Firewall to provide fully stateful application layer traffic restriction (such as URL filtering) and/or central management over a large number of enterprise segments or spokes (in a hub/spoke topology).  

If you have a complex network topology, such as a hub/spoke setup, you may need to create user-defined routes (UDR) to ensure the traffic goes through the desired route. For example, you have the option to use an UDR to redirect egress internet traffic through a specific Azure Firewall or a network virtual appliance.

How to deploy Azure Firewall:
 [https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

Virtual network traffic routing:
 [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)

**Recommendation #:** 3

**Category:** Network Security  
**Title:** NS-4: Deploy intrusion detection/intrusion prevention systems (IDS/IPS)

Use network intrusion detection and intrusion prevention systems (IDS/IPS) to inspect the network and payload traffic to or from your workload. Ensure that IDS/IPS is always tuned to provide high-quality alerts to your SIEM solution.

For more in-depth host level detection and prevention capability, use host-based IDS/IPS or a host-based endpoint detection and response (EDR) solution in conjunction with the network IDS/IPS.

**Azure Guidance:**

Use Azure Firewall’s IDPS capability to protect your virtual network to alert on and/or block traffic to and from known malicious IP addresses and domains.

For more in-depth host-level detection and prevention capabilities, deploy host-based IDS/IPS or a host-based endpoint detection and response (EDR) solution, such as Microsoft Defender for Endpoint, at the VM level in conjunction with the network IDS/IPS.



Azure Firewall IDPS:
 [https://docs.microsoft.com/azure/firewall/premium-features#idps](https://docs.microsoft.com/azure/firewall/premium-features#idps)

Microsoft Defender for Endpoint capability:  [https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)


**Recommendation #:** 4

**Category:** Network Security  
**Title:** NS-5: Deploy DDOS protection

Deploy distributed denial of service (DDoS) protection to protect your network and applications from attacks.

**Azure Guidance:**

DDoS Protection Basic is automatically enabled to protect the Azure underlying platform infrastructure (e.g., Azure DNS) and requires no configuration from the users.

For higher levels of protection of your application layer (Layer 7) attacks such as HTTP floods and DNS floods, enable the DDoS standard protection plan on your VNet to protect resources that are exposed to the public networks.

Manage Azure DDoS Protection Standard using the Azure portal:
 [https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

**Recommendation #:** 5

**Category:** Network Security  
**Title:** NS-1: Establish network segmentation boundaries

Ensure that your virtual network deployment aligns to the enterprise segmentation strategy defined in the GS-2 security control. Any workload that could incur higher risk for the organization should be in isolated virtual networks.
Examples of high-risk workload include:
- An application storing or processing highly sensitive data.
- An external network-facing application accessible by the public or users outside of your organization.
- An application using insecure architecture or containing vulnerabilities that cannot be easily remediated.

To enhance the enterprise segmentation strategy, restrict or monitor traffic between internal resources using network controls. For specific, well-defined applications (such as a 3-tier app), this can be a highly secure "deny by default, permit by exception" approach by restricting the ports, protocols, source, and destination IPs of the network traffic. If you have many applications and endpoints interacting with each other, blocking traffic may not scale well, and you may only be able to monitor traffic.

**Azure Guidance:**

Create a virtual network (VNet) as a fundamental segmentation approach in your Azure network, so resources such as VMs can be deployed into the VNet within a network boundary. To further segment the network, you can create subnets inside VNet for smaller sub-networks.

Use network security groups (NSG) as a network layer control to restrict or monitor traffic by port, protocol, source IP address, or destination IP address. Refer to NS-7 Simplify network security configuration to use Adaptive Network Hardening to recommend NSG hardening rules based on threat intelligence and traffic analysis result.

You can also use application security groups (ASGs) to simplify complex configuration. Instead of defining policy based on explicit IP addresses in network security groups, ASGs enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups.


Azure Virtual Network concepts and best practices:
 [https://docs.microsoft.com/azure/virtual-network/concepts-and-best-practices](https://docs.microsoft.com/azure/virtual-network/concepts-and-best-practices)

Add, change, or delete a virtual network subnet:
 [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)

How to create a network security group with security rules:
 [https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

Understand and use application security groups:
 [https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups)

**Recommendation #:** 6

**Category:** Network Security  
**Title:** NS-7: Simplify network security configuration

When managing a complex network environment, use tools to simplify, centralize and enhance the network security management.

**Azure Guidance:**

Use the following features to simplify the implementation and management of the virtual network, NSG rules, and Azure Firewall rules:
- Use Azure Virtual Network Manager to group, configure, deploy, and manage virtual networks and NSG rules across regions and subscriptions.
- Use Microsoft Defender for Cloud Adaptive Network Hardening to recommend NSG hardening rules that further limit ports, protocols and source IPs based on threat intelligence and traffic analysis result.
- Use Azure Firewall Manager to centralize the firewall policy and route management of the virtual network. To simplify the firewall rules and network security groups implementation, you can also use the Azure Firewall Manager Azure Resource Manager (ARM) template.

Adaptive Network Hardening in Microsoft Defender for Cloud:
 [https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

Azure Firewall Manager:
 [https://docs.microsoft.com/azure/firewall-manager/overview](https://docs.microsoft.com/azure/firewall-manager/overview)

Create an Azure Firewall and a firewall policy - ARM template
 [https://docs.microsoft.com/azure/firewall-manager/quick-firewall-policy](https://docs.microsoft.com/azure/firewall-manager/quick-firewall-policy)

**Recommendation #:** 7

**Category:** Logging and threat detection  
**Title:** LT-4: Enable network logging for security investigation


Enable logging for your network services to support network-related incident investigations, threat hunting, and security alert generation. The network logs may include logs from network services such as IP filtering, network and application firewall, DNS, flow monitoring and so on.

**Azure Guidance:**

Enable and collect network security group (NSG) resource logs, NSG flow logs, Azure Firewall logs, and Web Application Firewall (WAF) logs, and logs from virtual machines via the network traffic data collection agent for security analysis to support incident investigations, and security alert generation. You can send the flow logs to an Azure Monitor Log Analytics workspace and then use Traffic Analytics to provide insights.

Collect DNS query logs to assist in correlating other network data.

How to enable network security group flow logs:
 [https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

Azure Firewall logs and metrics:
 [https://docs.microsoft.com/azure/firewall/logs-and-metrics](https://docs.microsoft.com/azure/firewall/logs-and-metrics)

Azure networking monitoring solutions in Azure Monitor:
 [https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics)

Gather insights about your DNS infrastructure with the DNS Analytics solution:
 [https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)






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

**Recommendation #:** 1

**Category:** Network Security  
**Title:** NS-5: Deploy DDOS protection

Deploy distributed denial of service (DDoS) protection to protect your network and applications from attacks.

**Azure Guidance:**

DDoS Protection Basic is automatically enabled to protect the Azure underlying platform infrastructure (e.g., Azure DNS) and requires no configuration from the users.

For higher levels of protection of your application layer (Layer 7) attacks such as HTTP floods and DNS floods, enable the DDoS standard protection plan on your VNet to protect resources that are exposed to the public networks.

Manage Azure DDoS Protection Standard using the Azure portal:
 [https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)




---

## Threat #13

**Principle:** Integrity  
**Threat:** Compromises to your software supply chain, including the packages and libraries your software depends on, can result in a multitude of severe breaches and disruptive incidents. Attackers exploiting vulnerabilities within these components can gain unauthorized access, execute malicious code, or disrupt critical systems, posing significant risks to the security and functionality of your software infrastructure.

**Mitigation:**

1. Create a Software Bill of Materials (SBOM) with every release. SBOMs document any dependencies of a piece of software and can be used to track down dependencies with bugs, malicious code, or known breaches.
2. Cryptographically sign your releases and any related artifacts, such as SBOMs or vulnerability scans. Verify those signatures before running anything to ensure integrity.

**Recommendation #:** 1

**Category:** DevOps Security  
**Title:** DS-2: Ensure software supply chain security

Ensure the enterprise’s SDLC (Software Development Lifecycle) or process include a set of security controls to govern the in-house and third-party software components (including both proprietary and open-source software) where applications have dependencies. Define gating criteria to prevent vulnerable or malicious components being integrated and deployed into the environment.

The software supply chain security controls should at least include the following aspects:

Properly manage a Software Bill of Materials (SBOM) by identifying the upstream dependencies required for the service/resource development, build, integration and deployment phase.
Inventory and track the in-house and third-party software components for known vulnerability when there is a fix available in the upstream.
Assess the vulnerabilities and malware in the software components using static and dynamic application testing for unknown vulnerabilities.
Ensure the vulnerabilities and malware are mitigated using the appropriate approach. This may include source code local or upstream fix, feature exclusion and/or applying compensating controls if the direct mitigation is not available.
If closed source third-party components are used in your production environment, you may have limited visibility to its security posture. You should consider additional controls such as access control, network isolation and endpoint security to minimize the impact if there is a malicious activity or vulnerability associated with the component.


**Azure Guidance:**

For the GitHub platform, ensure the software supply chain security through the following capability or tools from GitHub Advanced Security or GitHub’s native feature:- Use Dependency Graph to scan, inventory and identify all your project’s dependencies and related vulnerabilities through Advisory Database.

- Use Dependabot to ensure that the vulnerable dependency is tracked and remediated, and ensure your repository automatically keeps up with the latest releases of the packages and applications it depends on.
- Use GitHub's native code scanning capability to scan the source code when sourcing the code externally.
- Use Microsoft Defender for Cloud to integrate vulnerability assessment for your container image in the CI/CD workflow.
For Azure DevOps, you can use third-party extensions to implement similar controls to inventory, analyze and remediate the third-party software components and their vulnerabilities


GitHub Dependency Graph:
 [https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)

GitHub Dependabot:
 [https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates)

Identify vulnerable container images in your CI/CD workflows:
 [https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd](https://docs.microsoft.com/azure/security-center/defender-for-container-registries-cicd)

Azure DevOps Marketplace – supply chain security:
 [https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS](https://marketplace.visualstudio.com/search?term=tag%3ASupply%20Chain%20Security&target=VSTS)




---

## Threat #14

**Principle:** Confidentiality and Integrity  
**Affected Asset:** Internal network traffic  
**Threat:** Unencrypted network traffic within the tenant boundary may cause compliance issues if it contains sensitive data.

**Mitigation:**

All products and services must encrypt data in transit using approved cryptographic protocols and algorithms.

1. Use TLS to encrypt all HTTP-based network traffic. Use other mechanisms, such as IPSec, to encrypt non-HTTP network traffic that contains customer or confidential data.
2. Use only TLS 1.2 or TLS 1.3. Use ECDHE-based ciphers suites and NIST curves. Use strong keys. Enable HTTP Strict Transport Security (HSTS). Turn off TLS compression and do not use ticket-based session resumption.

**Recommendation #:** 1

**Category:** Data Protection  
**Title:** DP-3: Encrypt sensitive data in transit

Protect the data in transit against 'out of band' attacks (such as traffic capture) using encryption to ensure that attackers cannot easily read or modify the data.

Set the network boundary and service scope where data in transit encryption is mandatory inside and outside of the network. While this is optional for traffic on private networks, this is critical for traffic on external and public networks.

**Azure Guidance:**

Enforce secure transfer in services such as Azure Storage, where a native data in transit encryption feature is built in.

Enforce HTTPS for web application workloads and services by ensuring that any clients connecting to your Azure resources use transport layer security (TLS) v1.2 or later. For remote management of VMs, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol.

For remote management of Azure virtual machines, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol. For secure file transfer, use the SFTP/FTPS service in Azure Storage Blob, App Service apps, and Function apps, instead of using the regular FTP service.

Note: Data in transit encryption is enabled for all Azure traffic traveling between Azure datacenters. TLS v1.2 or later is enabled on most Azure services by default. And some services such as Azure Storage and Application Gateway can enforce TLS v1.2 or later on the server side.


Double encryption for Azure data in transit:
 [https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit)

Understand encryption in transit with Azure:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

Information on TLS Security:
 [https://docs.microsoft.com/security/engineering/solving-tls1-problem](https://docs.microsoft.com/security/engineering/solving-tls1-problem)

Enforce secure transfer in Azure storage:
 [https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account)




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

**Recommendation #:** 1

**Category:** Identity Management  
**Title:** IM-8: Restrict the exposure of credential and secrets

Ensure that application developers securely handle credentials and secrets:
- Avoid embedding the credentials and secrets into the code and configuration files
- Use key vault or a secure key store service to store the credentials and secrets
- Scan for credentials in source code.  

Note: This is often governed and enforced through a secure software development lifecycle (SDLC) and DevOps security process

**Azure Guidance:**

When using a managed identity is not an option, ensure that secrets and credentials are stored in secure locations such as Azure Key Vault, instead of embedding them into the code and configuration files.

If you use Azure DevOps and GitHub for your code management platform:
- Implement Azure DevOps Credential Scanner to identify credentials within the code.
- For GitHub, use the native secret scanning feature to identify credentials or other forms of secrets within the code.

Clients such as Azure Functions, Azure Apps services, and VMs can use managed identities to access Azure Key Vault securely. See Data Protection controls related to the use of Azure Key Vault for secrets management.

Note: Azure Key Vault provides automatic rotation for supported services. For secrets which cannot be automatically rotated, ensure they are manually rotated periodically and purged when no longer in use.

How to setup Credential Scanner:
 [https://secdevtools.azurewebsites.net/helpcredscan.html](https://secdevtools.azurewebsites.net/helpcredscan.html)

GitHub secret scanning:
 [https://docs.github.com/github/administering-a-repository/about-secret-scanning](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Recommendation #:** 2

**Category:** DevOps Security  
**Title:** DS-3: Secure DevOps infrastructure

Ensure the DevOps infrastructure and pipeline follow security best practices across environments including your build, test, and production stages. This typically includes the security controls for following scope:

- Artifact repositories that store source code, built packages and images, project artifacts and business data.
- Servers, services, and tooling that host CI/CD pipelines.
- CI/CD pipeline configuration.

**Azure Guidance:**

As part of applying the Microsoft Cloud Security Benchmark to your DevOps infrastructure security controls, prioritize the following controls:
- Protect artifacts and the underlying environment to ensure the CI/CD pipelines don’t become avenues to insert malicious code. For example, review your CI/CD pipeline to identify any misconfiguration in core areas of Azure DevOps such as Organization, Projects, Users, Pipelines (Build & Release), Connections, and Build Agent to identify any misconfigurations such as open access, weak authentication, insecure connection setup and so on. For GitHub, use similar controls to secure the Organization permission levels.
- Ensure your DevOps infrastructure is deployed consistently across development projects. Track compliance of your DevOps infrastructure at scale by using Microsoft Defender for Cloud (such as Compliance Dashboard, Azure Policy, Cloud Posture Management) or your own compliance monitoring tools.
- Configure identity/role permissions and entitlement policies in Entra ID, native services, and CI/CD tools in your pipeline to ensure changes to the pipelines are authorized.
- Avoid providing permanent “standing” privileged access to the human accounts such as developers or testers by using features such as Azure managed identifies and just-in-time access.
- Remove keys, credentials, and secrets from code and scripts used in CI/CD workflow jobs and keep them in a key store or Azure Key Vault.
- If you run self-hosted build/deployment agents, follow Microsoft Cloud Security Benchmark controls including network security, posture and vulnerability management, and endpoint security to secure your environment.

Note: Refer to the Logging and Threat Detection, DS-7, and the Posture and Vulnerability Management sections to use services such as Azure Monitor and Microsoft Sentinel to enable governance, compliance, operational auditing, and risk auditing for your DevOps infrastructure.

DevSecOps controls overview – secure pipelines:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/secure/devsecops-controls](https://docs.microsoft.com/azure/cloud-adoption-framework/secure/devsecops-controls)

Secure your GitHub organization:
 [https://docs.github.com/en/code-security/getting-started/securing-your-organization](https://docs.github.com/en/code-security/getting-started/securing-your-organization)

Azure DevOps pipeline – Microsoft hosted agent security considerations:
 [https://docs.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#security](https://docs.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#security)




**Recommendation #:** 3

**Category:** Data Protection  
**Title:** DP-8: Ensure security of key and certificate repository

Ensure the security of the key vault service used for the cryptographic key and certificate lifecycle management. Harden your key vault service through access control, network security, logging and monitoring and backup to ensure keys and certificates are always protected using the maximum security.

**Azure Guidance:**

Secure your cryptographic keys and certificates by hardening your Azure Key Vault service through the following controls:
- Implement access control using RBAC policies in Azure Key Vault Managed HSM at the key level to ensure the least privilege and separation of duties principles are followed. For example, ensure separation of duties are in place for users who manage encryption keys so they do not have the ability to access encrypted data, and vice versa. For Azure Key Vault Standard and Premium, create unique vaults for different applications to ensure the least privilege and separation of duties principles are followed.
- Turn on Azure Key Vault logging to ensure critical management plane and data plane activities are logged.
 - Secure the Azure Key Vault using Private Link and Azure Firewall to ensure minimal exposure of the service
- Use managed identity to access keys stored in Azure Key Vault in your workload applications.
- When purging data, ensure your keys are not deleted before the actual data, backups and archives are purged.
- Backup your keys and certificates using Azure Key Vault. Enable soft delete and purge protection to avoid accidental deletion of keys.When keys need to be deleted, consider disabling keys instead of deleting them to avoid accidental deletion of keys and cryptographic erasure of  data.
- For bring your own key (BYOK) use cases, generate keys in an on-premises HSM and import them to maximize the lifetime and portability of the keys.
- Never store keys in plaintext format outside of the Azure Key Vault. Keys in all key vault services are not exportable by default.
- Use HSM-backed key types (RSA-HSM) in Azure Key Vault Premium and Azure Managed HSM for the hardware protection and the strongest FIPS levels.

Enable Microsoft Defender for Key Vault for Azure-native, advanced threat protection for Azure Key Vault, providing an additional layer of security intelligence.

Azure Key Vault overview:
 [https://docs.microsoft.com/azure/key-vault/general/overview](https://docs.microsoft.com/azure/key-vault/general/overview)

Azure Key Vault security best practices:
 [https://docs.microsoft.com/azure/key-vault/general/best-practices](https://docs.microsoft.com/azure/key-vault/general/best-practices)

Use managed identity to access Azure Key Vault:
 [https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad)

Overview of Microsoft Defender for Key Vault:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-key-vault-introduction](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-key-vault-introduction)




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

**Recommendation #:** 1

**Category:** Endpoint security  
**Title:** ES-2: Use modern anti-malware software

Use anti-malware solutions (also known as endpoint protection) capable of real-time protection and periodic scanning.

**Azure Guidance:**

Microsoft Defender for Cloud can automatically identify the use of a number of popular anti-malware solutions for your virtual machines and on-premises machines with Azure Arc configured and report the endpoint protection running status and make recommendations.

Microsoft Defender Antivirus is the default anti-malware solution for Windows server 2016 and above. For Windows server 2012 R2, use Microsoft Antimalware extension to enable SCEP (System Center Endpoint Protection).   For Linux VMs, use Microsoft Defender for Endpoint on Linux for the endpoint protection feature.

For both Windows and Linux, you can use Microsoft Defender for Cloud to discover and assess the health status of the anti-malware solution.

Note: You can also use Microsoft Defender for Cloud's Defender for Storage to detect malware uploaded to Azure Storage accounts.

Supported endpoint protection solutions:
 [https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows#supported-endpoint-protection-solutions-](https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows#supported-endpoint-protection-solutions-)

How to configure Microsoft Antimalware for Cloud Services and virtual machines:
 [https://docs.microsoft.com/azure/security/fundamentals/antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)



**Recommendation #:** 2

**Category:** Endpoint security  
**Title:** ES-3: Ensure anti-malware software and signatures are updated

Ensure anti-malware signatures are updated rapidly and consistently for the anti-malware solution.

**Azure Guidance:**

Follow recommendations in Microsoft Defender for Cloud to keep all endpoints up to date with the latest signatures. Microsoft Antimalware (for Windows) and Microsoft Defender for Endpoint (for Linux) will automatically install the latest signatures and engine updates by default.

For third-party solutions, ensure the signatures are updated in the third-party anti-malware solution.

How to deploy Microsoft Antimalware for Cloud Services and virtual machine:  
 [https://docs.microsoft.com/azure/security/fundamentals/antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

Endpoint protection assessment and recommendations in Microsoft Defender for Cloud:
 [https://docs.microsoft.com/azure/security-center/security-center-endpoint-protection](https://docs.microsoft.com/azure/security-center/security-center-endpoint-protection)





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

**Recommendation #:** 1

**Category:** Data Protection  
**Title:** DP-1: Discover, classify, and label sensitive data



Establish and maintain an inventory of the sensitive data, based on the defined sensitive data scope. Use tools to discover, classify and label the in- scope sensitive data.

**Azure Guidance:**

Use tools such as Microsoft Purview, which combines the former Azure Purview and Microsoft 365 compliance solutions, and Azure SQL Data Discovery and Classification to centrally scan, classify, and label the sensitive data that reside in the Azure, on-premises, Microsoft 365, and other locations.

Data classification overview:
 [https://docs.microsoft.com/azure/cloud-adoption-framework/govern/policy-compliance/data-classification](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/policy-compliance/data-classification)

Labeling in the Microsoft Purview Data Map:
 [https://docs.microsoft.com/azure/purview/create-sensitivity-label](https://docs.microsoft.com/azure/purview/create-sensitivity-label)

Tag sensitive information using Azure Information Protection:
 [https://docs.microsoft.com/azure/information-protection/what-is-information-protection](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)

How to implement Azure SQL Data Discovery:
 [https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

Microsoft Purview data sources:
 [https://docs.microsoft.com/azure/purview/purview-connector-overview#purview-data-sources](https://docs.microsoft.com/azure/purview/purview-connector-overview#purview-data-sources)

**Recommendation #:** 2

**Category:** Data Protection  
**Title:** DP-2: Monitor anomalies and threats targeting sensitive data

Monitor for anomalies around sensitive data, such as unauthorized transfer of data to locations outside of enterprise visibility and control. This typically involves monitoring for anomalous activities (large or unusual transfers) that could indicate unauthorized data exfiltration.

**Azure Guidance:**

Use Azure Information protection (AIP) to monitor the data that has been classified and labeled.

Use Microsoft Defender for Storage, Microsoft Defender for SQL, Microsoft Defender for open-source relational databases, and Microsoft Defender for Cosmos DB to alert on anomalous transfer of information that might indicate unauthorized transfers of sensitive data information.  

Note: If required for compliance of data loss prevention (DLP), you can use a host-based DLP solution from Azure Marketplace or a Microsoft 365 DLP solution to enforce detective and/or preventative controls to prevent data exfiltration.

Enable Azure Defender for SQL:
 [https://docs.microsoft.com/azure/azure-sql/database/azure-defender-for-sql](https://docs.microsoft.com/azure/azure-sql/database/azure-defender-for-sql)

Enable Azure Defender for Storage:
 [https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center)

Enable Microsoft Defender for Azure Cosmos DB:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-enable-cosmos-protections?tabs=azure-portal](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-enable-cosmos-protections?tabs=azure-portal)

Enable Microsoft Defender for open-source relational databases and respond to alerts:
 [https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-usage](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-databases-usage)

**Recommendation #:** 3

**Category:** Data Protection  
**Title:** DP-3: Encrypt sensitive data in transit

Protect the data in transit against 'out of band' attacks (such as traffic capture) using encryption to ensure that attackers cannot easily read or modify the data.

Set the network boundary and service scope where data in transit encryption is mandatory inside and outside of the network. While this is optional for traffic on private networks, this is critical for traffic on external and public networks.

**Azure Guidance:**

Enforce secure transfer in services such as Azure Storage, where a native data in transit encryption feature is built in.

Enforce HTTPS for web application workloads and services by ensuring that any clients connecting to your Azure resources use transport layer security (TLS) v1.2 or later. For remote management of VMs, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol.

For remote management of Azure virtual machines, use SSH (for Linux) or RDP/TLS (for Windows) instead of an unencrypted protocol. For secure file transfer, use the SFTP/FTPS service in Azure Storage Blob, App Service apps, and Function apps, instead of using the regular FTP service.

Note: Data in transit encryption is enabled for all Azure traffic traveling between Azure datacenters. TLS v1.2 or later is enabled on most Azure services by default. And some services such as Azure Storage and Application Gateway can enforce TLS v1.2 or later on the server side.


Double encryption for Azure data in transit:
 [https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/double-encryption#data-in-transit)

Understand encryption in transit with Azure:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

Information on TLS Security:
 [https://docs.microsoft.com/security/engineering/solving-tls1-problem](https://docs.microsoft.com/security/engineering/solving-tls1-problem)

Enforce secure transfer in Azure storage:
 [https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer?toc=/azure/storage/blobs/toc.json#require-secure-transfer-for-a-new-storage-account)

**Recommendation #:** 4

**Category:** Data Protection  
**Title:** DP-4: Enable data at rest encryption by default  

To complement access controls, data at rest should be protected against 'out of band' attacks (such as accessing underlying storage) using encryption. This helps ensure that attackers cannot easily read or modify the data.

**Azure Guidance:**

Many Azure services have data at rest encryption enabled by default at the infrastructure layer using a service-managed key. These service-managed keys are generated on the customer’s behalf and automatically rotated every two years.

Where technically feasible and not enabled by default, you can enable data at rest encryption in the Azure services, or in your VMs at the storage level, file level, or database level.

Understand encryption at rest in Azure:  [https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#encryption-at-rest-in-microsoft-cloud-services](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#encryption-at-rest-in-microsoft-cloud-services)

Data at rest double encryption in Azure:  [ [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)]( [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models))

Encryption model and key management table:
 [ [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)]( [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models))

**Recommendation #:** 5

**Category:** Data Protection  
**Title:** DP-5: Use customer-managed key option in data at rest encryption when required

If required for regulatory compliance, define the use case and service scope where customer-managed key option is needed. Enable and implement data at rest encryption using customer-managed key in services.

**Azure Guidance:**

Azure also provides an encryption option using keys managed by yourself (customer-managed keys) for most services.

Azure Key Vault Standard, Premium, and Managed HSM are natively integrated with many Azure Services for customer-managed key use cases. You may use Azure Key Vault to generate your key or bring your own keys.

However, using the customer-managed key option requires additional operational effort to manage the key lifecycle. This may include encryption key generation, rotation, revoke, and access control, etc.

Encryption model and key management table:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)

Services that support encryption using customer-managed key:  [https://docs.microsoft.com/azure/security/fundamentals/encryption-models](https://docs.microsoft.com/azure/security/fundamentals/encryption-models)#supporting-services

How to configure customer managed encryption keys in Azure Storage:  [https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal)

**Recommendation #:** 6

**Category:** Data Protection  
**Title:** DP-6: Use a secure key management process

Document and implement an enterprise cryptographic key management standard, processes, and procedures to control your key lifecycle. When there is a need to use customer-managed key in the services, use a secured key vault service for key generation, distribution, and storage. Rotate and revoke your keys based on the defined schedule and when there is a key retirement or compromise.

**Azure Guidance:**

Use Azure Key Vault to create and control your encryption keys life cycle, including key generation, distribution, and storage. Rotate and revoke your keys in Azure Key Vault and your service based on the defined schedule and when there is a key retirement or compromise. Require a certain cryptographic type and minimum key size when generating keys.

When there is a need to use customer-managed key (CMK) in the workload services or applications, ensure you follow the best practices:
- Use a key hierarchy to generate a separate data encryption key (DEK) with your key encryption key (KEK) in your key vault.
- Ensure keys are registered with Azure Key Vault and implemented via key IDs in each service or application.

To maximize the key material lifetime and portability, bring your own key (BYOK) to the services (i.e., importing HSM-protected keys from your on-premises HSMs into Azure Key Vault). Follow the recommended guideline to perform the key generation and key transfer.

Note: Refer to the below for the FIPS 140-2 level for Azure Key Vault types and FIPS compliance/validation level.
- Software-protected keys in vaults (Premium & Standard SKUs):  FIPS 140-2 Level 1
- HSM-protected keys in vaults (Premium SKU): FIPS 140-2 Level 2
- HSM-protected keys in Managed HSM: FIPS 140-2 Level 3
Azure Key Vault Premium uses a shared HSM infrastructure in the backend. Azure Key Vault Managed HSM uses dedicated, confidential service endpoints with a dedicated HSM for when you need a higher level of key security.

Azure Key Vault overview:
 [https://docs.microsoft.com/azure/key-vault/general/overview](https://docs.microsoft.com/azure/key-vault/general/overview)

Azure data encryption at rest--Key Hierarchy:
 [https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#key-hierarchy](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#key-hierarchy)

BYOK(Bring Your Own Key) specification:
 [https://docs.microsoft.com/azure/key-vault/keys/byok-specification](https://docs.microsoft.com/azure/key-vault/keys/byok-specification)




---

## Threat #27

**Principle:** Privacy  
**Affected Asset:** All data stores  
**Threat:** The application does not allow the deletion of a user’s profile from storage.

**Mitigation:**

If required by regulation, implement the ability to delete a user’s profile, and all associated data, from all the datastores.

---
