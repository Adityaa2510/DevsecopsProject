## 1. Introduction to DevSecOps

### What is DevSecOps?

DevSecOps stands for **Development, Security, and Operations**. It is a software development approach that integrates security practices throughout the entire Software Development Life Cycle (SDLC), rather than treating security as a final step before deployment.

In traditional software development, developers focus on building features, operations teams focus on deployment and reliability, and security teams often review the application near the end. This can lead to vulnerabilities being discovered late, when fixing them is more expensive and time-consuming.

DevSecOps integrates security into planning, coding, building, testing, deploying, and monitoring. Security checks are automated wherever possible and combined with human reviews for complex risks.

### Core objectives

* **Shift left:** Identify and fix vulnerabilities early in development.
* **Automation:** Integrate security checks into CI/CD pipelines.
* **Continuous monitoring:** Detect threats and misconfigurations after deployment.
* **Collaboration:** Make developers, security engineers, and operations teams jointly responsible for security.
* **Risk reduction:** Reduce the likelihood and impact of security incidents.
* **Secure delivery:** Maintain a balance between delivery speed, reliability, and security.

### DevOps vs. DevSecOps

| DevOps                                              | DevSecOps                                                                     |
| --------------------------------------------------- | ----------------------------------------------------------------------------- |
| Integrates development and operations.              | Integrates development, security, and operations.                             |
| Focuses on automation, collaboration, and delivery. | Adds continuous security validation and risk management.                      |
| May perform security testing in separate stages.    | Embeds security checks throughout the SDLC.                                   |
| Automates build, test, and deployment workflows.    | Also automates code, dependency, infrastructure, and runtime security checks. |
| Focuses on delivery speed and reliability.          | Focuses on secure, reliable, and efficient delivery.                          |

### Shift-left security

Shift-left security means moving security activities earlier in the software development process. For example, instead of discovering a hardcoded password during a production audit, a secret-scanning tool can identify it during a pull request.

Early detection helps reduce remediation effort, prevents vulnerable code from progressing through the pipeline, and gives developers faster feedback.

### DevSecOps lifecycle

1. **Plan:** Define security requirements, risks, and compliance needs.
2. **Code:** Follow secure coding practices and scan source code.
3. **Build:** Compile and test the application with automated security checks.
4. **Test:** Perform SAST, DAST, SCA, API, and other security testing.
5. **Release:** Verify artifacts, approvals, and release requirements.
6. **Deploy:** Use secure configurations and controlled deployment processes.
7. **Operate:** Monitor infrastructure, workloads, logs, and alerts.
8. **Improve:** Remediate findings, learn from incidents, and update controls.

---

## 2. Foundations of Application and Infrastructure Security

### How web applications work

A web application generally consists of a client, a server, and a data store. A user interacts with a browser or application, which sends a request to a server. The server processes the request, may communicate with a database or other services, and returns a response.

Common components include:

* **Frontend:** The user interface, commonly built with HTML, CSS, and JavaScript.
* **Backend:** Business logic, authentication, authorization, and application APIs.
* **Database:** Stores application information.
* **Web server or reverse proxy:** Receives and routes incoming HTTP requests.
* **Infrastructure:** Virtual machines, containers, Kubernetes clusters, networks, and cloud services.

Each component introduces potential security risks. For example, the frontend can expose sensitive data, the backend can contain injection vulnerabilities, databases can be improperly exposed, and infrastructure can be misconfigured.

### IP addresses, DNS, and ports

An IP address identifies a device or network interface for communication. DNS translates human-readable domain names into IP addresses.

A port identifies a service or application endpoint on a host. Common examples include:

| Port | Common use                               |
| ---- | ---------------------------------------- |
| 22   | SSH                                      |
| 53   | DNS                                      |
| 80   | HTTP                                     |
| 443  | HTTPS                                    |
| 5432 | PostgreSQL                               |
| 6379 | Redis                                    |
| 8080 | Common alternative HTTP application port |

Open ports are not necessarily vulnerabilities, but exposed services should be intentional, properly configured, and restricted to the required sources.

### HTTP and HTTPS

HTTP is a protocol used to transfer information between clients and servers. HTTPS uses TLS to protect communication in transit.

TLS provides:

* **Confidentiality:** Helps prevent unauthorized parties from reading transmitted data.
* **Integrity:** Helps detect tampering with transmitted data.
* **Authentication:** Enables clients to verify a server's identity using certificates.

HTTPS does not automatically secure an application against broken access control, injection, insecure authentication, or other application-level vulnerabilities.

### Cookies, sessions, and tokens

A cookie is a browser-stored value sent with matching requests. Applications often use cookies to maintain a session identifier.

A session allows a server to associate a user with authenticated state. A token is a credential or representation of claims that may be used to authenticate or authorize requests.

Secure session handling commonly includes:

* `Secure` cookies to restrict transmission to HTTPS.
* `HttpOnly` cookies to prevent ordinary JavaScript access.
* Appropriate `SameSite` settings to reduce certain cross-site request risks.
* Session expiration and rotation.
* Server-side invalidation where applicable.
* Protection against token leakage and replay.

### APIs

An API allows applications and services to communicate through defined interfaces. APIs may use REST, GraphQL, or other protocols.

API security should consider authentication, authorization, input validation, rate limiting, data exposure, logging, and protection against abuse. An API should not assume that a request is safe simply because it comes from a frontend application.

### Common infrastructure components

| Component       | Purpose                                      | Security consideration                                              |
| --------------- | -------------------------------------------- | ------------------------------------------------------------------- |
| Reverse proxy   | Routes incoming requests to backend services | Restrict administrative access and configure secure headers and TLS |
| Load balancer   | Distributes traffic across servers           | Protect management interfaces and configure health checks securely  |
| Firewall        | Controls network traffic                     | Use least-privilege network rules                                   |
| VPN             | Provides protected remote connectivity       | Require strong authentication and controlled access                 |
| CDN             | Distributes content closer to users          | Configure origin protection and caching carefully                   |
| WAF             | Filters potentially malicious web traffic    | Tune rules and monitor false positives                              |
| Secrets manager | Stores and controls access to credentials    | Apply least privilege, rotation, and audit logging                  |

---

## 3. DevSecOps Security Tool Categories

Security tools support different stages of the SDLC. No single tool can identify every vulnerability, so organizations generally use multiple complementary tools.

### 3.1 Static Application Security Testing (SAST)

SAST analyzes source code, bytecode, or other application representations without executing the application. It searches for potentially insecure coding patterns, such as injection risks, unsafe functions, and insecure data handling.

**Common tools:**

* SonarQube
* Semgrep
* CodeQL
* Checkmarx
* Fortify

**Where it is used:**

* Developer workstations
* Pull requests
* CI pipelines
* Scheduled code reviews

**Typical workflow:**

1. A developer pushes code.
2. A SAST tool analyzes the changed code or project.
3. The tool reports findings with file locations and rule details.
4. Developers review findings, validate the risk, and remediate issues.
5. The pipeline applies agreed quality or security gates.

**Advantages:**

* Detects potential vulnerabilities early.
* Does not require a running application.
* Can identify risky code paths and insecure patterns.
* Supports automated code review.

**Limitations:**

* May report false positives.
* May not understand all runtime behavior.
* Findings need contextual review.
* Results depend on supported languages, rules, and configuration.

### 3.2 Dynamic Application Security Testing (DAST)

DAST tests a running application by sending requests and examining responses. It evaluates the application from an external perspective and can detect issues that become visible during execution.

**Common tools:**

* OWASP ZAP
* Burp Suite

**Typical findings:**

* Reflected or stored cross-site scripting
* Certain injection vulnerabilities
* Insecure security headers
* Authentication and session weaknesses
* Other observable web application issues

**Typical workflow:**

1. Deploy the application to a test environment.
2. Define the authorized scope and test accounts.
3. Configure the DAST scanner.
4. Crawl or discover application endpoints.
5. Run safe, authorized tests.
6. Review findings and validate them.
7. Remediate and retest.

DAST generally requires a running target and may not identify the exact source-code location of a vulnerability. It can also miss issues in inaccessible or untested application paths.

### 3.3 Software Composition Analysis (SCA)

SCA identifies third-party libraries, packages, and dependencies used by an application. It compares detected components and versions with vulnerability information and may also report license or dependency risks.

**Common tools:**

* Snyk
* OWASP Dependency-Check
* Dependabot

**Why SCA matters:**

Modern applications depend heavily on external packages. A project may contain secure custom code while still being exposed through an outdated or vulnerable library.

**Typical workflow:**

1. Detect dependencies from manifests, lockfiles, or built artifacts.
2. Identify package names and versions.
3. Match components against vulnerability data.
4. Review severity, exploitability, and dependency usage.
5. Upgrade, patch, replace, or mitigate affected packages.
6. Re-scan to confirm remediation.

SCA results can be affected by incomplete dependency metadata, inaccurate component identification, and whether a vulnerable component is reachable in the application's actual usage.

### 3.4 Secret scanning

Secret scanning searches source code, repositories, configuration files, and other content for exposed credentials, such as API keys, tokens, passwords, and private keys.

**Common tools:**

* Gitleaks
* TruffleHog

**Common secret exposure locations:**

* Git commits
* Environment files
* Application configuration
* CI/CD logs
* Container image layers
* Infrastructure-as-code files

**Recommended response to an exposed secret:**

1. Treat the credential as compromised.
2. Revoke or rotate it.
3. Investigate where and when it was exposed.
4. Review logs and access history for suspicious use.
5. Remove the secret from active code and history where appropriate.
6. Move the credential to a secrets manager or secure injection mechanism.
7. Add preventive scanning and policy checks.

Removing a secret from the latest commit is not enough if it remains in repository history or has already been copied.

### 3.5 Infrastructure-as-Code (IaC) scanning

IaC tools inspect infrastructure definitions before resources are deployed. These definitions may include Terraform, Kubernetes manifests, and other configuration files.

**Common tools:**

* Checkov
* tfsec

**Examples of issues:**

* Publicly accessible storage
* Overly permissive security groups
* Missing encryption settings
* Excessive IAM permissions
* Insecure Kubernetes configurations

**Typical workflow:**

1. Developer writes IaC configuration.
2. Scanner checks the configuration against security policies.
3. Findings are reported with resource and rule details.
4. The developer corrects unsafe settings.
5. CI/CD validates the configuration before deployment.

IaC scanning helps identify misconfigurations before they become live infrastructure, but it cannot always determine the final deployed state or every runtime exposure.

### 3.6 Container image scanning

Container scanning inspects images for known vulnerable operating-system packages, language dependencies, and other configuration risks.

**Common tools:**

* Trivy
* Grype

**Typical workflow:**

1. Build the container image.
2. Scan the image before publishing or deployment.
3. Identify vulnerable packages, secrets, or misconfigurations supported by the scanner.
4. Update affected base images or dependencies.
5. Rebuild and re-scan.
6. Apply release policies based on risk.

A container image scan is a point-in-time assessment. Images should be rebuilt and rescanned as vulnerabilities and package information change.

### 3.7 Kubernetes policy and configuration security

Kubernetes security includes workload configuration, identity and access management, network boundaries, secrets, and cluster-level controls.

**Common tools:**

* OPA Gatekeeper
* Kyverno

These tools help enforce policies such as:

* Requiring approved container registries.
* Restricting privileged containers.
* Requiring resource limits.
* Enforcing labels and approved configurations.
* Restricting unsafe workload settings.

Policy enforcement can be applied during admission, so disallowed configurations can be rejected before they are accepted into the cluster.

### 3.8 Runtime security

Runtime security monitors applications and infrastructure while they are executing. It can identify suspicious behavior that static configuration scans or build-time testing may not reveal.

**Common tool:**

* Falco

Runtime monitoring may detect unusual process execution, unexpected access to sensitive files, suspicious container activity, or other behaviors covered by configured rules.

Runtime alerts require investigation and tuning. An alert does not automatically prove that a compromise has occurred.

### 3.9 Cloud Security Posture Management (CSPM)

CSPM tools assess cloud environments for configuration risks, policy violations, and security posture issues.

Common review areas include:

* Publicly exposed resources
* Identity and access permissions
* Encryption configuration
* Logging and audit settings
* Network exposure
* Compliance-related configuration

Cloud-native security services and third-party CSPM products can help identify misconfigurations, but findings still need validation against business requirements and the actual architecture.

### 3.10 SIEM

A Security Information and Event Management (SIEM) system collects, correlates, and analyzes logs and security events from multiple sources.

Common data sources include:

* Servers and endpoints
* Firewalls and network sensors
* Cloud audit logs
* Identity providers
* Applications
* Kubernetes workloads

A SIEM helps security teams identify suspicious activity, investigate incidents, and maintain visibility across environments.

### 3.11 Secrets management

Secrets managers provide a controlled way to store, retrieve, rotate, and audit sensitive credentials.

Examples of secrets include database passwords, API keys, signing keys, and service credentials.

A secure secrets-management approach includes:

* Centralized storage
* Least-privilege access
* Rotation and revocation
* Audit logs
* Secure delivery to applications
* Avoidance of hardcoded secrets in source code and images

### 3.12 Web Application Firewall (WAF)

A WAF filters HTTP traffic to identify and block requests that match configured attack patterns or policies.

It can help reduce exposure to common web attacks, but it is not a replacement for secure coding, application testing, authentication, or authorization controls.

---

## 4. Comparing Major Security Testing Approaches

| Approach           | Full form                            | Main target                        | When used                         | Main limitation                                          |
| ------------------ | ------------------------------------ | ---------------------------------- | --------------------------------- | -------------------------------------------------------- |
| SAST               | Static Application Security Testing  | Source code or code representation | During coding and CI              | May produce false positives and miss runtime-only issues |
| DAST               | Dynamic Application Security Testing | Running application                | Test or staging environment       | Requires a reachable application and suitable coverage   |
| SCA                | Software Composition Analysis        | Third-party dependencies           | During development and builds     | Depends on component detection and vulnerability data    |
| Secret scanning    | —                                    | Credentials and sensitive values   | Commit, PR, and repository checks | Cannot guarantee detection of every secret format        |
| IaC scanning       | Infrastructure-as-Code scanning      | Infrastructure definitions         | Before deployment                 | May not capture final runtime state                      |
| Container scanning | —                                    | Container images                   | Build and release stages          | May not detect every runtime threat                      |
| Runtime security   | —                                    | Executing workloads                | Production and runtime            | Requires tuning and incident investigation               |

### SonarQube vs. Semgrep

| SonarQube                                                              | Semgrep                                                          |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Provides code-quality and security analysis capabilities.              | Focuses on pattern-based static analysis and customizable rules. |
| Commonly used for continuous code-quality management.                  | Commonly used for fast, targeted code security checks.           |
| Provides project dashboards and quality gates.                         | Supports custom rules for specific code patterns.                |
| Can support multiple languages depending on edition and configuration. | Language support depends on available analysis modes and rules.  |

### SAST vs. DAST

SAST analyzes code without executing the application, while DAST tests the behavior of a running application. SAST can help developers locate potentially unsafe code patterns early; DAST can expose issues observable through real application requests and responses. They are complementary, and using both can improve coverage.

---

## 5. Secure CI/CD Pipeline Blueprint

A secure CI/CD pipeline embeds security controls at each stage, so code and artifacts are checked before they reach production.

### Stage 1: Developer workstation

**Activities:**

* Use approved development environments.
* Follow secure coding practices.
* Scan code and dependencies locally where practical.
* Prevent secrets from being committed.
* Keep development tools and dependencies updated.

**Example controls:**

* IDE security extensions
* Pre-commit secret scanning
* Local SAST checks
* Dependency review

### Stage 2: Pull request

**Activities:**

* Review code changes.
* Run automated tests and security scans.
* Validate dependency changes.
* Check IaC changes.
* Require approvals for sensitive changes.

**Example controls:**

* SAST using SonarQube, Semgrep, or CodeQL
* Secret scanning using Gitleaks
* SCA using Dependabot or Snyk
* IaC scanning using Checkov

A pull-request policy may prevent merging when a defined security threshold is exceeded, subject to documented exceptions and review.

### Stage 3: Build

**Activities:**

* Compile or package the application.
* Run unit and integration tests.
* Validate build configuration.
* Generate build metadata.
* Protect build credentials and artifacts.

**Security considerations:**

* Use trusted build environments.
* Restrict CI runner permissions.
* Avoid exposing secrets in logs.
* Ensure dependencies are obtained from approved sources.

### Stage 4: Package and container image

**Activities:**

* Build the container image.
* Scan the image and its dependencies.
* Validate the base image.
* Store artifacts in a controlled registry.
* Apply artifact integrity and provenance controls where available.

**Example tools:**

* Trivy
* Grype
* Gitleaks

### Stage 5: Staging

**Activities:**

* Deploy to a controlled test environment.
* Run DAST and API security tests.
* Validate configuration and access controls.
* Perform integration and acceptance testing.
* Review security findings before release.

**Example tools:**

* OWASP ZAP
* Burp Suite
* Checkov
* Kubernetes policy tools

Testing must remain within the authorized scope and use safe test data and accounts.

### Stage 6: Production release

**Activities:**

* Verify release approvals.
* Confirm security requirements are met.
* Deploy approved artifacts.
* Use controlled access and secure configuration.
* Keep rollback and recovery procedures available.

**Security controls:**

* Role-based access control
* Approval gates
* Protected deployment credentials
* Signed or verified artifacts where supported
* Restricted deployment permissions

### Stage 7: Runtime monitoring

**Activities:**

* Collect logs and security events.
* Monitor application and infrastructure behavior.
* Detect suspicious activity.
* Investigate alerts.
* Patch, contain, and recover as necessary.

**Example tools and services:**

* Falco for runtime behavior monitoring
* SIEM platforms for log correlation
* Cloud security monitoring services
* CSPM tools for posture visibility

### Example pipeline sequence

1. Developer pushes a change.
2. CI starts automated tests and static analysis.
3. Secret and dependency scans run.
4. IaC configuration is checked.
5. An approved build is packaged.
6. Container images are scanned.
7. Artifacts are deployed to staging.
8. DAST and API tests run.
9. Release policy and approvals are checked.
10. The approved artifact is deployed to production.
11. Runtime monitoring and alerting continue.

### Security gates

A security gate is a defined condition that must be satisfied before the pipeline proceeds. For example, a team may block deployment when a critical vulnerability is confirmed and no approved exception exists.

Security gates should:

* Have clearly defined thresholds.
* Use consistent severity criteria.
* Distinguish validated issues from false positives.
* Provide actionable feedback to developers.
* Include a documented exception process.
* Be reviewed periodically to ensure they remain useful.

---

## 6. Website Security Review Checklist

A website security review should begin with a clearly authorized scope. Testing should only target systems, accounts, and environments for which permission has been granted.

### 6.1 Scope and reconnaissance

* Identify approved domains, subdomains, APIs, and IP ranges.
* Document test accounts and permitted actions.
* Identify application entry points and exposed services.
* Record excluded systems and prohibited test types.
* Establish rate limits and testing windows.

### 6.2 Transport security

* Confirm HTTPS is enforced.
* Review TLS certificate validity and configuration.
* Check HTTP-to-HTTPS redirection.
* Review secure cookie settings.
* Validate that sensitive data is not transmitted in plaintext.

### 6.3 Authentication

* Review password and account recovery flows.
* Check multi-factor authentication where required.
* Review login rate limiting and lockout behavior.
* Verify session creation and invalidation.
* Review authentication error messages for unnecessary information disclosure.

### 6.4 Authorization

* Check whether users can access only their permitted resources.
* Test access controls with authorized test accounts.
* Review role-based and object-level permissions.
* Verify that administrative functions are properly restricted.
* Check that authorization is enforced server-side.

### 6.5 Input validation

* Validate input on the server side.
* Use parameterized queries where applicable.
* Apply context-appropriate output encoding.
* Restrict accepted formats and sizes.
* Review file paths, query parameters, headers, and API request bodies.

### 6.6 Sessions and tokens

* Use secure session identifiers.
* Rotate sessions after sensitive authentication events where appropriate.
* Set suitable expiration and invalidation behavior.
* Protect tokens from exposure in URLs, logs, or client-side storage.
* Review cookie attributes and cross-site request protections.

### 6.7 File uploads

* Restrict file types and sizes.
* Validate file content, not just extensions.
* Generate safe server-side filenames.
* Store uploads outside executable web paths where possible.
* Apply malware scanning where appropriate.
* Restrict access to uploaded content.

### 6.8 API security

* Enforce authentication and authorization.
* Validate all request fields.
* Apply rate limits and abuse protections.
* Avoid unnecessary sensitive data exposure.
* Review error handling and logging.
* Check that API permissions are enforced independently of the frontend.

### 6.9 Security headers

Review the use and configuration of appropriate headers, such as:

* Content-Security-Policy
* Strict-Transport-Security
* X-Content-Type-Options
* Referrer-Policy
* Frame-related protections, such as suitable CSP directives

Headers should be configured to match the application's behavior and requirements. A header alone does not guarantee application security.

### 6.10 Errors and logging

* Avoid exposing stack traces or secrets to users.
* Log security-relevant events.
* Protect logs from unauthorized access and modification.
* Avoid storing passwords, tokens, and other secrets in logs.
* Define alerting and incident response procedures.

---

## 7. Secure Cloud Review Checklist

Cloud security reviews should assess identity, network exposure, data protection, logging, and workload configuration. The exact controls depend on the cloud provider and the architecture.

### 7.1 AWS

Review:

* IAM roles, policies, and permission boundaries.
* Public access to storage resources.
* Security group and network access rules.
* Encryption for data at rest and in transit.
* Audit logging and monitoring.
* Secrets storage and rotation.
* Container and Kubernetes workload configuration.

Relevant AWS services may include:

* AWS IAM for identity and access management.
* AWS CloudTrail for account activity logging.
* Amazon GuardDuty for threat detection.
* AWS Security Hub for security findings and posture visibility.
* Amazon Inspector for supported vulnerability assessments.
* AWS Config for configuration tracking and compliance evaluation.
* AWS Secrets Manager for secrets storage and rotation.

### 7.2 Microsoft Azure

Review:

* Microsoft Entra ID identities and access policies.
* Role assignments and privileged access.
* Network security groups and public exposure.
* Storage access and encryption.
* Audit logs and threat monitoring.
* Secrets and certificates.
* Kubernetes and container configurations.

Relevant Azure services may include:

* Microsoft Entra ID for identity and access management.
* Microsoft Defender for Cloud for cloud security posture and workload protection capabilities.
* Azure Key Vault for secrets, keys, and certificates.
* Azure Monitor for telemetry and monitoring.
* Microsoft Sentinel for SIEM and security analytics.

### 7.3 Google Cloud

Review:

* IAM roles and service accounts.
* Publicly accessible storage.
* Firewall and network configuration.
* Encryption and key management.
* Audit logging and monitoring.
* Secret storage.
* Workload and container configuration.

Relevant services may include:

* Cloud IAM
* Security Command Center
* Cloud Audit Logs
* Cloud Key Management Service
* Secret Manager
* Cloud Logging and Cloud Monitoring

### Cloud review principles

* Apply least privilege.
* Restrict public exposure to justified use cases.
* Enable appropriate audit logs.
* Encrypt sensitive information.
* Use managed identity or workload identity where applicable.
* Protect and rotate credentials.
* Monitor configuration drift.
* Review findings regularly and track remediation.

---

## 8. Common Security Risks and Mitigation

### Injection

Injection vulnerabilities occur when untrusted input is interpreted as part of a command or query.

**Mitigation:**

* Use parameterized queries.
* Validate inputs.
* Apply safe APIs.
* Restrict database permissions.
* Avoid unsafe dynamic command construction.

### Cross-site scripting (XSS)

XSS occurs when untrusted content is executed as script in a user's browser.

**Mitigation:**

* Apply context-aware output encoding.
* Sanitize HTML where HTML input is intentionally supported.
* Use a suitable Content Security Policy.
* Avoid unsafe DOM manipulation.

### Broken access control

Broken access control occurs when an application fails to enforce restrictions on what a user can access or modify.

**Mitigation:**

* Enforce authorization server-side.
* Validate access to each protected object.
* Use least-privilege roles.
* Test access boundaries using authorized accounts.
* Deny access by default.

### Server-side request forgery (SSRF)

SSRF occurs when an attacker can influence a server to make requests to unintended destinations.

**Mitigation:**

* Restrict outbound network access.
* Validate and allowlist permitted destinations.
* Block access to internal or metadata services where not needed.
* Revalidate redirects and resolved destinations.
* Apply network segmentation and monitoring.

### Insecure configuration

Misconfigurations can expose services, data, or administrative interfaces.

**Mitigation:**

* Use secure defaults and hardened templates.
* Scan IaC before deployment.
* Restrict network access.
* Disable unused services.
* Monitor configuration drift.

### Vulnerable dependencies

Third-party libraries may contain known vulnerabilities.

**Mitigation:**

* Maintain dependency inventories.
* Run SCA regularly.
* Upgrade or replace vulnerable components.
* Review dependency provenance and update processes.
* Monitor advisories for critical components.

---

## 9. SSRF Testing and Prevention: Interview Notes

### What tools can help test for SSRF?

Tools such as Burp Suite and OWASP ZAP can help inspect and modify HTTP requests in an authorized test environment. Specialized out-of-band interaction services may help identify server-side requests when a response is not directly visible.

Testing should be performed only on systems where explicit authorization exists, with safe targets and controlled callbacks.

### How can SSRF be prevented?

* Avoid accepting complete user-controlled URLs when a narrower input is possible.
* Use allowlists for approved destinations.
* Validate hostname resolution and resolved IP addresses.
* Restrict outbound connections at the network layer.
* Prevent access to sensitive internal services and metadata endpoints.
* Handle redirects carefully.
* Log and investigate unexpected outbound requests.

---

## 10. Secrets and Credential Security

### Why should secrets not be stored in source code?

Secrets committed to source control may remain in repository history, be copied to forks, appear in build logs, or be included in container images. This can expose systems even after the secret is removed from the current code.

### Secure handling practices

* Store secrets in a secrets manager.
* Use short-lived credentials where possible.
* Apply least privilege.
* Rotate and revoke exposed credentials.
* Avoid logging secret values.
* Limit access to CI/CD variables.
* Prevent secrets from entering container layers and build artifacts.
* Audit access to sensitive credentials.

### Example secret-handling workflow

1. Create a credential in a secure secrets manager.
2. Grant the workload narrowly scoped access.
3. Retrieve the credential securely at runtime or through an approved deployment mechanism.
4. Prevent the value from appearing in logs or artifacts.
5. Rotate credentials according to policy and revoke them when no longer required.

---

## 11. Vulnerability Management

Vulnerability management is the continuous process of identifying, assessing, prioritizing, remediating, and verifying security weaknesses.

### Typical lifecycle

1. **Discover:** Identify vulnerabilities using scanners, advisories, audits, and monitoring.
2. **Validate:** Confirm the finding and determine whether it applies to the environment.
3. **Assess:** Consider severity, exposure, exploitability, and business impact.
4. **Prioritize:** Determine remediation order using the organization's risk criteria.
5. **Remediate:** Patch, upgrade, reconfigure, isolate, or otherwise mitigate.
6. **Verify:** Re-scan or retest to confirm that the issue is addressed.
7. **Document:** Record ownership, decisions, exceptions, and completion.

### Severity vs. risk

Severity describes the technical seriousness of a vulnerability under a defined scoring system. Risk also considers the environment, exposure, business context, compensating controls, and likelihood of exploitation.

A critical-rated vulnerability in an unreachable test component may have a different operational priority from a lower-severity issue exposed on a sensitive production service. Prioritization should be based on documented risk criteria rather than severity alone.

### False positives

A false positive is a scanner finding that does not represent a real vulnerability in the assessed context.

Handling process:

* Review the evidence.
* Reproduce or validate the finding safely.
* Check configuration and application context.
* Record the rationale if the finding is not applicable.
* Suppress or tune the rule only through a documented process.
* Reassess exclusions periodically.

---

## 12. Practical DevSecOps Tools and Workflows

### Lab 1: OWASP Juice Shop

**Purpose:** Practice web application security testing in an intentionally vulnerable application.

Activities:

* Deploy the lab locally or in an isolated environment.
* Explore application features.
* Inspect HTTP requests and responses.
* Use a proxy to understand application behavior.
* Review security findings and remediation concepts.

### Lab 2: PortSwigger Web Security Academy

**Purpose:** Learn web security concepts through guided, authorized labs.

Topics include:

* Authentication
* Access control
* Injection
* XSS
* SSRF
* API security

### Lab 3: OWASP WebGoat and DVWA

**Purpose:** Learn common web application weaknesses in intentionally vulnerable environments.

Activities:

* Set up an isolated instance.
* Study a vulnerability category.
* Perform the provided lab exercise.
* Review the underlying cause.
* Implement or document the corresponding mitigation.

### Lab 4: Kubernetes Goat

**Purpose:** Learn Kubernetes security weaknesses in a purpose-built training environment.

Activities:

* Deploy the lab in an isolated cluster.
* Inspect workload configuration.
* Review permissions and network exposure.
* Explore the provided scenarios.
* Apply appropriate security controls.

### Lab 5: CloudGoat and flaws.cloud

**Purpose:** Study cloud security risks using deliberately vulnerable training environments.

Activities:

* Use only the designated training account and environment.
* Review identity and resource configuration.
* Identify the intended misconfiguration.
* Understand its possible impact.
* Apply least-privilege and configuration improvements.
* Tear down lab resources after use.

### Lab 6: Terraform and Checkov

**Purpose:** Learn how to detect insecure infrastructure configuration before deployment.

Activities:

1. Create a small Terraform project.
2. Add a few cloud resources using non-sensitive test configuration.
3. Run Checkov against the project.
4. Review the reported misconfigurations.
5. Correct the configuration.
6. Run the scan again.
7. Document the findings and remediation.

### Lab 7: Trivy

**Purpose:** Learn container image and configuration scanning.

Activities:

1. Build a sample container image.
2. Scan it using Trivy.
3. Review package and vulnerability findings.
4. Update affected dependencies or the base image.
5. Rebuild and rescan.
6. Record the results.

### Lab 8: Gitleaks

**Purpose:** Detect exposed credentials in repositories.

Activities:

1. Create a test repository using dummy values only.
2. Run Gitleaks.
3. Review the findings.
4. Remove the test secret and add preventive checks.
5. Re-scan and document the result.

Never insert real production credentials into a training repository to demonstrate secret detection.

---

## 13. Secure Linux and Source Control Fundamentals

### Linux security basics

Linux security includes user and group permissions, process isolation, service configuration, patching, and auditability.

Important practices:

* Use non-root accounts for routine operations.
* Apply least privilege with file and directory permissions.
* Restrict SSH access and use strong authentication.
* Disable unnecessary services.
* Keep packages and operating-system components updated.
* Monitor authentication and system logs.
* Use host firewall rules appropriate to the workload.

### Git and secure source control

Git tracks changes to source code and supports collaboration through branches, commits, and pull requests.

Secure source-control practices include:

* Protecting main branches.
* Requiring code reviews.
* Using signed commits where required.
* Restricting repository and CI permissions.
* Scanning for secrets.
* Reviewing dependency changes.
* Protecting tokens and webhooks.
* Auditing access and repository settings.

---

## 14. Practical Project: Secure CI/CD Pipeline

### Project objective

Build a secure CI/CD workflow for a sample web application that performs automated security checks before release and supports basic monitoring after deployment.

### Suggested components

| Layer              | Example technology                               |
| ------------------ | ------------------------------------------------ |
| Application        | FastAPI, Node.js, or another small web framework |
| Source control     | GitHub                                           |
| CI/CD              | GitHub Actions                                   |
| SAST               | Semgrep or SonarQube                             |
| SCA                | Dependabot or OWASP Dependency-Check             |
| Secret scanning    | Gitleaks                                         |
| Container scanning | Trivy                                            |
| IaC scanning       | Checkov                                          |
| Containerization   | Docker                                           |
| Deployment         | A local or controlled test environment           |
| DAST               | OWASP ZAP                                        |
| Runtime monitoring | Falco or suitable platform telemetry             |
| Logs and alerts    | Centralized logging or SIEM                      |


## 15. Key Takeaways

* DevSecOps integrates security into development, delivery, and operations.
* Shift-left security supports earlier identification of vulnerabilities.
* SAST, DAST, SCA, secret scanning, IaC scanning, and container scanning cover different risk areas.
* Kubernetes policy tools enforce configuration requirements, while runtime tools help detect suspicious behavior during execution.
* CSPM and SIEM provide broader cloud posture and security-event visibility.
* Secure CI/CD requires controlled access, automated checks, security gates, and clear remediation processes.
* Cloud security reviews should cover identity, network exposure, data protection, logging, secrets, and workload configuration.
* Security findings must be validated, prioritized, remediated, and retested.
* Tools support security work, but they do not replace sound architecture, secure coding, human review, or incident response.
* Practical labs should be performed only in environments where testing is authorized.
