# Foundations

## Comprehensive Technical Notes

# Table of Contents

1. Computing and Internet Fundamentals
2. Web Application Fundamentals
3. What Happens When You Enter a URL?
4. URL (Uniform Resource Locator)
5. DNS (Domain Name System)
6. IP Address
7. Private IP Address
8. Localhost
9. Network Ports
10. TCP (Transmission Control Protocol)
11. UDP (User Datagram Protocol)
12. HTTP
13. HTTPS
14. TLS (Transport Layer Security)
15. HTTP Request
16. HTTP Response
17. HTTP Methods
18. HTTP Status Codes
19. HTTP Headers
20. Cookies
21. Sessions
22. API (Application Programming Interface)
23. REST API
24. JSON
25. XML
26. Proxy
27. Reverse Proxy
28. Load Balancer
29. Firewall
30. VPN
31. CDN
32. Linux Security Fundamentals
33. Linux Users and Groups
34. Root User
35. sudo
36. Linux File Permissions
37. chmod
38. chown
39. Linux Processes
40. Linux Services
41. Linux Logs
42. SSH
43. Security Risks of Internet-Exposed SSH
44. Patching
45. System Hardening
46. Package Managers
47. Git and Source Control Security
48. Repository
49. Git Branches
50. Pull Requests
51. Branch Protection
52. CODEOWNERS
53. Secrets in Git
54. Git History Rewriting
55. Signed Commits
56. GitHub Actions Security
57. Core DevSecOps Security Principles
58. Important Security Relationships

---

# 1. Computing and Internet Fundamentals

## 1.1 Computer System

A computer system is a combination of hardware, software, an operating system, storage, networking and users that work together to process, store and exchange information.

A computer system consists of several components, each of which can introduce security risks.

| Component        | Description                                | Potential Security Risk              |
| ---------------- | ------------------------------------------ | ------------------------------------ |
| Hardware         | Physical devices and components            | Theft or physical tampering          |
| Operating System | Manages hardware and software              | Misconfiguration or exploitation     |
| Software         | Programs that perform specific tasks       | Vulnerabilities and malicious code   |
| Network          | Connects systems and enables communication | Unauthorized access and interception |
| Storage          | Stores data and application files          | Data leakage or unauthorized access  |
| Users            | People interacting with the system         | Phishing and social engineering      |

**DevSecOps relevance**

An application is not limited to its source code. It depends on infrastructure, containers, cloud resources, identity management, networking, secrets and CI/CD pipelines.

A typical application environment can be represented as:

`Code → Servers → Containers → Secrets → Logs → Cloud Resources → IAM → Networks → CI/CD`

Security must be integrated across the entire application lifecycle rather than being limited to the development phase.

## 1.2 Server

A server is a computer system or software application that provides resources, data or services to other systems, known as clients, over a network.

Common types of servers include:

* **Web server:** Delivers websites and handles HTTP requests.
* **Database server:** Stores, manages and retrieves application data.
* **Authentication server:** Verifies user identities and supports access control.

Servers can operate as physical machines, virtual machines, containers or serverless functions.

**Server security controls**

* Regularly apply security patches.
* Enforce access control and least privilege.
* Harden the operating system and installed software.
* Enable centralized logging and monitoring.
* Encrypt sensitive data.
* Restrict unnecessary network access.

## 1.3 Client

A client is a device, application or service that requests information or functionality from a server.

Examples include web browsers requesting websites, mobile applications accessing APIs and backend services communicating with other services.

**Security principle: Never fully trust the client.**

Client-side validation is useful for improving user experience, but it can be bypassed. For example, hiding an administrative button does not prevent a user from sending a direct request to an administrative endpoint.

Authentication, authorization and critical input validation must therefore be enforced on the server side.

## 1.4 Application

An application is a software program designed to perform specific tasks for users or other systems.

Examples include banking applications, e-commerce platforms, mobile applications, APIs and internal business dashboards.

Application security protects the system against vulnerabilities throughout its lifecycle, including:

* Insecure application design.
* Vulnerable source code.
* Compromised dependencies.
* Incorrect configuration.
* Weak authentication and authorization.
* Unsafe runtime behavior.

# 2. Web Application Fundamentals

## 2.1 Web Application

A web application is software accessed through a web browser or another HTTP-compatible client.

A typical web application follows this architecture:

`Frontend → Backend → API → Database`

It may also integrate authentication systems, session management, external services and cloud infrastructure.

**Common web application vulnerabilities**

* **Broken Access Control:** Users access resources or perform actions beyond their permissions.
* **Cross-Site Scripting (XSS):** Malicious scripts execute in a user's browser.
* **SQL Injection:** Malicious input alters database queries.
* **Cross-Site Request Forgery (CSRF):** A browser is induced to send an unwanted authenticated request.
* **Server-Side Request Forgery (SSRF):** A server is manipulated into making unintended requests.
* **Insecure Session Management:** Sessions are improperly created, stored or protected.
* **File Upload Vulnerabilities:** Unsafe files are uploaded or processed.
* **Security Misconfiguration:** Incorrect settings expose functionality or sensitive information.

## 2.2 Browser

A browser is a client application that retrieves, processes and displays web content.

It communicates with web servers using HTTP or HTTPS and processes resources such as:

* HTML for page structure.
* CSS for presentation.
* JavaScript for client-side functionality.
* Images, fonts and other media.
* API responses and dynamic content.

**Browser security considerations**

* XSS and malicious scripts.
* Clickjacking.
* CSRF.
* Cookie theft.
* Incorrect CORS configuration.
* Mixed-content loading.

Browser security controls, including Content Security Policy (CSP), secure cookies and appropriate cross-origin policies, help reduce these risks.

# 3. What Happens When You Enter a URL?

When a user enters a URL into a browser, several operations occur to retrieve and display the requested resource.

**Request lifecycle**

1. The browser parses the URL to identify the scheme, domain, port and requested resource.
2. It checks relevant browser caches for reusable resources.
3. DNS resolution determines the IP address associated with the domain.
4. The browser establishes a network connection to the destination.
5. A TCP connection is established when TCP is used.
6. For HTTPS, a TLS handshake negotiates secure communication.
7. The browser sends an HTTP request to the server.
8. The server processes the request and returns an HTTP response.
9. The browser interprets the response and renders the page.
10. Additional resources, such as JavaScript, CSS, images and API data, may be requested.

**Security considerations**

Security controls apply throughout the process:

`DNS → TCP → TLS → HTTP → Cookies → Authentication → Authorization → CORS → CSP → Server-side Validation`

For example, TLS protects data during transmission, while server-side authorization determines whether a user can access a requested resource.

# 4. URL (Uniform Resource Locator)

A URL is an address that identifies where a resource can be accessed on a network.

Example:

`https://example.com:443/api/users?id=10#profile`

| Component    | Value         | Purpose                                 |
| ------------ | ------------- | --------------------------------------- |
| Scheme       | `https`       | Specifies the communication protocol    |
| Host         | `example.com` | Identifies the destination              |
| Port         | `443`         | Identifies the network service endpoint |
| Path         | `/api/users`  | Identifies the requested resource       |
| Query string | `id=10`       | Passes parameters                       |
| Fragment     | `profile`     | Identifies a section within a resource  |

**Security relevance**

URLs are important during application security testing because paths, parameters, redirects and parsing behavior can expose vulnerabilities such as IDOR, SSRF, open redirects and injection flaws.

# 5. DNS (Domain Name System)

DNS is a distributed naming system that translates domain names into IP addresses, allowing clients to locate services without remembering numerical addresses.

For example:

`example.com → IP address`

DNS is essential for accessing websites, APIs and other internet-connected services.

**DNS security risks**

* Phishing domains that imitate legitimate websites.
* DNS hijacking that redirects users to malicious destinations.
* Subdomain takeover caused by improperly managed DNS records.
* Misconfigured DNS records that expose infrastructure.
* DNS-based techniques used to bypass SSRF restrictions.

**DevSecOps relevance**

DNS information is useful during authorized reconnaissance because it can reveal subdomains, exposed services and an organization's external attack surface.

# 6. IP Address

An IP address is a numerical identifier assigned to a device or network interface for communication over an IP network.

There are two primary versions:

* **IPv4:** Uses 32-bit addresses, such as `192.168.1.10`.
* **IPv6:** Uses 128-bit addresses, such as `2001:db8::1`.

IP addresses are used in firewall rules, access control lists, network segmentation, logging and threat detection.

**Public vs. private IP addresses**

| Public IP                             | Private IP                                              |
| ------------------------------------- | ------------------------------------------------------- |
| Routable over the public internet     | Used within private networks                            |
| May expose internet-facing services   | Usually not directly reachable from the public internet |
| Commonly assigned to public endpoints | Commonly used for internal resources                    |

# 7. Private IP Address

Private IP addresses are reserved for use within internal networks and are not directly routed across the public internet.

The three private IPv4 ranges are:

| Range                           | CIDR             |
| ------------------------------- | ---------------- |
| `10.0.0.0 – 10.255.255.255`     | `10.0.0.0/8`     |
| `172.16.0.0 – 172.31.255.255`   | `172.16.0.0/12`  |
| `192.168.0.0 – 192.168.255.255` | `192.168.0.0/16` |

**Security relevance**

Private addresses are commonly used for internal databases, application servers and cloud resources. They support network segmentation and reduce direct internet exposure.

A vulnerable public-facing server should not be permitted to send arbitrary requests to internal addresses, as this could enable SSRF attacks.

# 8. Localhost

Localhost refers to the current computer. It is commonly used to access services running locally on the same system.

| IP Version | Address     |
| ---------- | ----------- |
| IPv4       | `127.0.0.1` |
| IPv6       | `::1`       |

**Security relevance**

Localhost is frequently used by development servers, databases and administrative interfaces.

In an SSRF vulnerability, an attacker may attempt to manipulate a server into requesting localhost services that are otherwise inaccessible externally.

# 9. Network Ports

A port is a logical communication endpoint that allows a system to distinguish between network services.

| Service | Default Port |
| ------- | -----------: |
| HTTP    |           80 |
| HTTPS   |          443 |
| SSH     |           22 |
| DNS     |           53 |
| RDP     |         3389 |

**Security relevance**

Open ports form part of a system's attack surface. Security reviews should identify unnecessary exposed ports, publicly accessible services, inappropriate firewall rules and unrestricted cloud security groups or network security groups.

# 10. TCP (Transmission Control Protocol)

TCP is a connection-oriented transport protocol that provides reliable, ordered communication between network applications.

Its main characteristics include:

* Connection-oriented communication.
* Reliable data delivery.
* Ordered transmission.
* Retransmission of lost data.
* Error detection and flow control.

TCP is commonly used by HTTP, HTTPS, SSH, SMTP and many database protocols.

**Security relevance**

TCP port scanning, including scanning performed with Nmap, can help identify accessible services and potential exposure during authorized security assessments.

# 11. UDP (User Datagram Protocol)

UDP is a connectionless transport protocol that sends datagrams without guaranteeing delivery or ordering.

Its characteristics include:

* Connectionless communication.
* Low protocol overhead.
* No built-in retransmission.
* No guarantee of packet ordering.
* Suitability for latency-sensitive applications.

Common UDP applications include DNS, DHCP, streaming and some VPN protocols.

**Security relevance**

Misconfigured UDP services can be abused in denial-of-service amplification attacks. UDP scanning also differs from TCP scanning because UDP does not use a TCP-style connection handshake.

# 12. HTTP (Hypertext Transfer Protocol)

HTTP is an application-layer protocol used for communication between web clients and servers.

It defines how requests and responses are structured and exchanged.

Common HTTP methods include `GET`, `POST`, `PUT`, `PATCH` and `DELETE`.

Common response status codes include `200`, `301`, `403`, `404` and `500`.

**Security relevance**

Web application security testing frequently involves inspecting and modifying HTTP requests and responses using tools such as Burp Suite and OWASP ZAP.

HTTP alone does not encrypt transmitted information, so HTTPS is generally used for sensitive web communication.

# 13. HTTPS

HTTPS is HTTP communication protected by Transport Layer Security (TLS).

It protects data transmitted between a client and a server by providing confidentiality and integrity, with server authentication provided through the TLS certificate-validation process.

**HTTPS helps protect against:**

* Unauthorized reading of data in transit.
* Undetected modification of transmitted data.
* Impersonation of a server when certificate validation is correctly performed.

**Important:** HTTPS does not guarantee that an application is secure.

An HTTPS-enabled website may still contain SQL injection, XSS, IDOR, broken access control and authentication vulnerabilities.

# 14. TLS (Transport Layer Security)

TLS is a cryptographic protocol that protects communication between systems over a network.

Its primary security properties are:

1. **Confidentiality:** Encrypts data to prevent unauthorized reading.
2. **Integrity:** Helps detect unauthorized modification of transmitted data.
3. **Authentication:** Supports verification of the server's identity, and can also authenticate clients when configured.

TLS replaced the older SSL protocols.

**TLS security assessment**

A security review may examine:

* Certificate validity and expiration.
* Supported protocol versions.
* Cipher suite configuration.
* Certificate trust and hostname validation.
* HTTP Strict Transport Security (HSTS).
* Deprecated or weak protocols.

Older versions, such as TLS 1.0 and TLS 1.1, should generally be disabled where applicable.

# 15. HTTP Request

An HTTP request is a message sent by a client to a server to retrieve information or perform an operation.

An HTTP request may contain:

* Method.
* URL path.
* Headers.
* Cookies.
* Query parameters.
* Request body.

Example:

```http
GET /profile?id=10 HTTP/1.1
Host: example.com
Authorization: Bearer <token>
Cookie: session=abc123
```

**Security testing**

Security testers examine request parameters, cookies, authorization headers, methods and request bodies to assess authentication, authorization, input validation, injection and business logic.

# 16. HTTP Response

An HTTP response is a message returned by a server after processing a client request.

A response generally contains:

* Status code.
* Response headers.
* Cookies.
* Response body.

**Security relevance**

Responses may reveal sensitive information, verbose error messages, missing security headers, authentication failures, tokens or inconsistent access-control behavior.

Reviewing responses can help identify weaknesses in application configuration and authorization.

# 17. HTTP Methods

HTTP methods describe the intended operation to be performed on a resource.

| Method  | Typical Purpose                          |
| ------- | ---------------------------------------- |
| GET     | Retrieve data                            |
| POST    | Submit data or create a resource         |
| PUT     | Replace a resource                       |
| PATCH   | Partially update a resource              |
| DELETE  | Delete a resource                        |
| OPTIONS | Discover supported communication options |

**Security considerations**

Security testing should verify that appropriate authorization is enforced for every method, sensitive actions cannot be performed through unexpected methods and API endpoints do not expose dangerous functionality unnecessarily.

# 18. HTTP Status Codes

HTTP status codes indicate the outcome of a request.

| Range | Meaning            |
| ----- | ------------------ |
| 2xx   | Successful request |
| 3xx   | Redirection        |
| 4xx   | Client error       |
| 5xx   | Server error       |

Important examples:

| Code | Meaning                                                |
| ---- | ------------------------------------------------------ |
| 200  | OK                                                     |
| 301  | Moved Permanently                                      |
| 401  | Unauthorized; authentication is required or has failed |
| 403  | Forbidden                                              |
| 404  | Resource Not Found                                     |
| 500  | Internal Server Error                                  |

**Security relevance**

Differences in response codes can sometimes reveal username enumeration, authorization weaknesses, hidden endpoints and differences in application behavior.

# 19. HTTP Headers

HTTP headers provide additional metadata about requests and responses.

**Common request headers**

* `Authorization`
* `Cookie`
* `User-Agent`
* `Origin`
* `Content-Type`

**Common response headers**

* `Set-Cookie`
* `Content-Security-Policy`
* `Strict-Transport-Security`

Headers influence authentication, cookie handling, CORS, caching, content interpretation, clickjacking protection and transport security.

Incorrect header configuration can introduce security weaknesses even when the application itself is functioning correctly.

# 20. Cookies

A cookie is a small piece of information stored by a browser and automatically sent with matching requests, according to its configured scope and attributes.

Cookies are commonly used for session management, user preferences and tracking.

**Important cookie security attributes**

| Attribute | Purpose                                                             |
| --------- | ------------------------------------------------------------------- |
| Secure    | Restricts cookie transmission to secure HTTPS connections           |
| HttpOnly  | Prevents direct access to the cookie through client-side JavaScript |
| SameSite  | Controls when cookies are sent with cross-site requests             |

**Security principle**

Session cookies should use appropriate security attributes, and sensitive information should not be stored directly in cookies without adequate protection.

# 21. Sessions

A session maintains a user's state across multiple HTTP requests. Since HTTP is stateless by default, sessions allow applications to recognize authenticated users across interactions.

For example, after logging in, a user can navigate between pages without repeatedly entering credentials.

**Session security controls**

* Set appropriate session expiration.
* Invalidate sessions on logout.
* Rotate session identifiers after authentication.
* Protect against session fixation.
* Configure secure cookie attributes.
* Protect session identifiers against theft.

# 22. API (Application Programming Interface)

An API is a defined interface that enables different software systems to communicate and exchange data.

Web APIs commonly use HTTP for communication and JSON for data exchange.

**Essential API security controls**

* Authentication to verify identities.
* Authorization to restrict access.
* Input and schema validation.
* Rate limiting to control request volume.
* Logging and monitoring.
* Protection against excessive data exposure.

APIs should enforce security controls on the server, regardless of whether requests originate from browsers, mobile applications or other backend services.

# 23. REST API

REST (Representational State Transfer) is an architectural style for designing networked applications around resources and standard HTTP methods.

Example:

```http
GET /users/10
DELETE /users/10
```

The first request retrieves user 10, while the second requests deletion of that user, provided the requester has the required authorization.

**Security concern: Broken Object-Level Authorization (BOLA)**

BOLA occurs when an API fails to verify whether the authenticated user is authorized to access or modify the particular object identified in a request.

Every request involving a protected resource should be checked against the user's permissions.

# 24. JSON (JavaScript Object Notation)

JSON is a lightweight data-interchange format used to exchange structured information between applications.

Example:

```json
{
  "id": 10,
  "name": "Aditya"
}
```

JSON is commonly used in REST APIs because it is easy for applications to generate, transmit and parse.

**Security relevance**

JSON fields often contain user-controlled input. Applications should validate expected data types, permitted fields, field lengths and authorization requirements.

Security testing may examine injection, mass assignment, hidden fields, authorization bypass and schema violations.

# 25. XML (Extensible Markup Language)

XML is a markup language used to represent and exchange structured data.

It is used in legacy APIs, SOAP-based services, SAML and document-processing systems.

**XML External Entity (XXE)**

XXE is a vulnerability that may occur when an XML parser processes external entities in an unsafe manner. Depending on the configuration, it may expose local resources or cause unintended network requests.

**Security control**

Disable unnecessary external entity processing and configure XML parsers securely.

# 26. Proxy

A proxy is an intermediary that forwards network requests between clients and servers.

Depending on its configuration, a proxy can inspect traffic, modify requests, record activity and apply filtering rules.

**Security testing tools**

* Burp Suite.
* OWASP ZAP.

These tools can intercept HTTP traffic so authorized testers can inspect or modify request parameters, headers, cookies, tokens, methods and bodies.

# 27. Reverse Proxy

A reverse proxy sits between external clients and backend servers, forwarding incoming requests to the appropriate internal service.

Typical architecture:

```text
Client
  |
  v
Reverse Proxy
  |
  v
Backend Server
```

Common reverse proxy technologies include Nginx, Apache, HAProxy, AWS Application Load Balancer and Azure Application Gateway.

**Security functions**

* TLS termination.
* Web Application Firewall integration.
* Request rate limiting.
* Security header management.
* Traffic routing.
* Hiding internal backend topology.

A reverse proxy provides a centralized point for traffic management and security enforcement.

# 28. Load Balancer

A load balancer distributes incoming network traffic across multiple backend servers to improve availability, scalability and resource utilization.

Example:

```text
                 +--> Server 1
Client --> LB ---+--> Server 2
                 +--> Server 3
```

**Benefits**

* High availability.
* Horizontal scalability.
* Traffic distribution.
* Improved resource utilization.

**Security relevance**

Load balancers can terminate TLS, integrate with WAFs and control traffic routing. Incorrect configurations may expose internal services or route traffic to unintended destinations.

# 29. Firewall

A firewall is a network security control that allows or blocks traffic according to predefined rules.

Rules may consider the source, destination, port, protocol and direction of traffic.

Examples include:

* AWS Security Groups.
* Azure Network Security Groups (NSGs).
* Google Cloud firewall rules.

**Security principle**

A firewall reduces the attack surface by allowing only the network traffic that is required.

However, a firewall does not eliminate application-level vulnerabilities. An application behind a firewall can still be vulnerable to SQL injection, broken access control and other security issues.

# 30. VPN (Virtual Private Network)

A VPN creates an encrypted tunnel between users or networks, enabling protected communication across an underlying network.

Organizations commonly use VPNs to provide controlled access to internal resources.

**Security applications**

Administrative interfaces and internal services can be restricted to VPN connections, private networks or bastion hosts instead of being directly exposed to the public internet.

A VPN should be combined with authentication, authorization and appropriate network restrictions.

# 31. CDN (Content Delivery Network)

A CDN is a geographically distributed network of servers that delivers and caches content closer to users.

Common CDN providers include Cloudflare, Akamai, AWS CloudFront, Azure CDN and Google Cloud CDN.

**Security capabilities**

* DDoS protection.
* Web Application Firewall integration.
* TLS termination.
* Bot filtering.
* Content caching.

**Security risks**

Incorrect CDN configuration may expose origin servers, cache sensitive information or allow traffic to bypass intended security controls.

CDN caching policies should therefore be reviewed carefully, particularly for authenticated or personalized content.

---

# 32. Linux Security Fundamentals

## 32.1 Why Linux Is Important in DevSecOps

Linux is widely used in servers, containers, CI runners, cloud instances and Kubernetes nodes.

DevSecOps engineers need practical knowledge of Linux permissions, processes, logs, services, networking, package management and SSH to secure and troubleshoot infrastructure.

Linux security involves maintaining system integrity, controlling access, reducing unnecessary services and monitoring suspicious activity.

# 33. Linux Users and Groups

Linux uses users and groups to control access to files, processes and system resources.

A user represents an individual account, while a group allows permissions to be shared across multiple users.

**Security principle**

Applications and services should generally run under dedicated, low-privilege accounts rather than the root user.

Group membership must also be reviewed because certain groups, such as `sudo` or those with Docker socket access, may provide powerful privileges.

# 34. Root User

The root user is the Linux superuser with extensive administrative privileges.

Root can modify system files, change permissions, install software and manage processes.

**Security risk**

Running applications as root increases the potential impact of an application compromise.

The same principle applies to containers. Containers should avoid running as root unless elevated privileges are genuinely required.

# 35. sudo

`sudo` allows authorized users to execute commands with elevated privileges without directly logging in as root.

Its permissions are managed through the `sudoers` configuration.

**Security best practices**

* Grant only the permissions necessary for a user's responsibilities.
* Regularly audit sudo access.
* Avoid broad or unrestricted permissions.
* Review commands that allow privilege escalation.

Excessive sudo permissions can create opportunities for privilege escalation.

# 36. Linux File Permissions

Linux file permissions determine which users can read, write or execute files and directories.

Permissions are defined for three categories:

1. Owner.
2. Group.
3. Others.

Example:

```text
rwxr-x---
```

| Category | Permission              |
| -------- | ----------------------- |
| Owner    | Read, write and execute |
| Group    | Read and execute        |
| Others   | No permissions          |

Weak file permissions may expose sensitive information, allow unauthorized modification or enable execution of malicious files.

# 37. chmod

`chmod` changes the access permissions of files and directories in Linux.

Example:

```bash
chmod 600 key.pem
```

This grants the owner read and write permissions while denying access to group members and others.

**Security relevance**

Private keys, credentials and sensitive configuration files should have restrictive permissions. Broad permissions, such as `777`, should be avoided unless there is a specific, justified requirement.

# 38. chown

`chown` changes the ownership of a file or directory.

It can assign ownership to a particular user and group.

Example:

```bash
chown appuser:appgroup application.log
```

Correct ownership helps prevent unauthorized modification of application files, binaries, logs and secrets.

# 39. Linux Processes

A process is a running instance of a program. Every process has associated information, including a process ID (PID), owner, memory usage, open files and command-line arguments.

**Security monitoring**

Process inspection can help identify:

* Suspicious commands.
* Unexpected services.
* Cryptocurrency miners.
* Reverse shells.
* Sensitive information exposed in command-line arguments.

Unexpected processes should be investigated to determine whether they are legitimate or potentially malicious.

# 40. Linux Services

A service is a background process that provides functionality to other applications or users. On many Linux systems, services are managed through `systemd`.

Examples include SSH, Nginx, database servers and application daemons.

**Security practices**

* Disable unnecessary services.
* Apply security patches.
* Use secure configurations.
* Monitor service activity.
* Restrict network exposure.

Reducing the number of running services helps minimize the system's attack surface.

# 41. Linux Logs

Linux logs record events related to system activity, authentication, services and applications.

Common log locations include:

```text
/var/log/auth.log
/var/log/syslog
/var/log/secure
```

The exact files depend on the Linux distribution and logging configuration. Systems using `systemd` may also store logs in the system journal.

**Security importance**

Logs help security teams investigate failed login attempts, privilege escalation, service crashes, suspicious commands and security incidents.

Centralized logging and monitoring can make it easier to correlate events across multiple systems.

# 42. SSH (Secure Shell)

SSH is a secure protocol used for remote command-line access and system administration.

Its commonly used default port is 22.

**SSH security practices**

* Prefer key-based authentication.
* Disable password authentication where appropriate.
* Restrict access to approved source IP addresses.
* Avoid direct root login.
* Use MFA where supported.
* Prefer bastion hosts or private access.
* Monitor authentication attempts.
* Rotate keys when required.

SSH configuration should be reviewed regularly to ensure that remote access remains controlled.

# 43. Security Risks of Internet-Exposed SSH

Publicly accessible SSH services may receive continuous automated connection attempts and can be targeted through brute-force attacks, stolen credentials, compromised private keys and exploitation of vulnerable software.

If an attacker obtains valid credentials or a private key, they may gain unauthorized access to the server.

**Cloud protection strategies**

* Restrict SSH through VPNs.
* Use bastion hosts.
* Use managed access mechanisms, such as AWS Systems Manager Session Manager.
* Allow only approved IP ranges.
* Prefer private network access.
* Monitor authentication attempts and suspicious activity.

Reducing direct internet exposure limits the opportunities for unauthorized remote access.

# 44. Patching

Patching is the process of applying software updates to fix vulnerabilities, bugs and stability issues.

Patching applies to operating systems, application dependencies, containers, Kubernetes components and cloud infrastructure.

**DevSecOps patching lifecycle**

```text
Detection
    |
    v
Prioritization
    |
    v
Patch
    |
    v
Testing
    |
    v
Deployment
    |
    v
Verification
```

Automating patch identification, prioritization, testing and deployment helps organizations manage vulnerabilities consistently.

# 45. System Hardening

System hardening is the process of reducing the attack surface by securely configuring operating systems, applications and infrastructure.

**Common hardening activities**

* Disable unnecessary services.
* Install security patches.
* Enforce least privilege.
* Restrict network access.
* Enable logging and monitoring.
* Configure restrictive file permissions.
* Remove unnecessary software.

Security hardening can be guided by recognized standards, such as CIS Benchmarks, where applicable.

# 46. Package Managers

A package manager is a tool that installs, updates, configures and removes software packages.

Examples include:

| Ecosystem          | Package Manager |
| ------------------ | --------------- |
| Debian/Ubuntu      | apt             |
| RHEL-based systems | yum, dnf        |
| Alpine Linux       | apk             |
| JavaScript         | npm             |
| Python             | pip             |
| Java               | Maven           |
| Go                 | go mod          |

**Security risks**

Dependencies may contain known vulnerabilities, malicious packages or outdated components.

**Security controls**

* Use trusted package repositories.
* Maintain lockfiles.
* Perform dependency scanning.
* Pin and control dependency versions.
* Review package provenance where possible.

---

# 47. Git and Source Control Security

## 47.1 Why Git Is Important in DevSecOps

Git is a distributed version control system used to manage source code, infrastructure-as-code, CI/CD workflows, configuration, documentation and change history.

It allows teams to track modifications, collaborate and review changes.

**DevSecOps Git security controls**

* Pull request checks.
* Branch protection.
* Secret scanning.
* Code reviews.
* Signed commits.
* CODEOWNERS.
* Automated security gates.

Repositories may contain sensitive credentials, infrastructure details or malicious changes, making source control security an important part of the software delivery lifecycle.

# 48. Repository

A repository is a storage location for project files and their version history.

It may contain application source code, infrastructure code, CI/CD workflows, documentation and configuration files.

**Security risks**

A repository may accidentally expose:

* API keys and credentials.
* Internal architecture details.
* Vulnerable source code.
* Deployment configurations.

Repository access should be restricted according to user responsibilities, and sensitive information should be kept in appropriate secrets-management systems.

# 49. Git Branches

A Git branch represents an independent line of development, allowing teams to work on features, bug fixes and releases without directly modifying the primary branch.

Branches help teams isolate changes before they are reviewed and merged.

**Security relevance**

Important branches, such as `main`, should be protected against unauthorized or unreviewed changes.

# 50. Pull Requests

A Pull Request (PR) is a request to merge changes from one branch into another.

Pull requests support collaboration by providing a central place for code review, discussion, automated checks and security validation.

**Security checks in pull requests**

* Static Application Security Testing (SAST).
* Software Composition Analysis (SCA).
* Secret scanning.
* Infrastructure-as-Code (IaC) scanning.
* Security policy checks.
* CODEOWNERS reviews.

Pull requests are an important shift-left security checkpoint because security issues can be detected before changes reach production.

# 51. Branch Protection

Branch protection is a collection of repository rules that control how changes can be merged into important branches.

Possible controls include:

* Required code reviews.
* Passing CI checks.
* Signed commits.
* Restrictions on force pushes.
* Restricted write access.

Branch protection reduces the possibility of unauthorized or unreviewed changes reaching production.

# 52. CODEOWNERS

CODEOWNERS is a repository feature that defines which individuals or teams are responsible for reviewing changes to particular files or directories.

For example, an organization may require security-team reviews for:

```text
authentication/
terraform/
.github/workflows/
```

This helps ensure that changes to sensitive components are reviewed by people with relevant technical expertise.

# 53. Secrets in Git

Secrets include API keys, access tokens, passwords, private keys and other credentials that should not be publicly accessible.

Storing secrets in Git is risky because Git maintains a history of previous commits. Deleting a secret from the latest version does not necessarily remove it from earlier commits, forks, clones, caches or CI logs.

Attackers may also scan public repositories for exposed credentials.

**Correct incident response**

```text
Detect
  |
  v
Revoke or Rotate
  |
  v
Remove Exposure
  |
  v
Investigate
```

The first priority is to invalidate or rotate the exposed credential. Removing it from the repository history is an additional remediation step, not a substitute for credential rotation.

Secrets should instead be managed through dedicated secret-management systems or secure CI/CD secret stores.

# 54. Git History Rewriting

Git history rewriting changes existing commits, often to remove accidentally committed sensitive information or unwanted files.

Common tools include:

* `git filter-repo`
* BFG Repo-Cleaner

**Important security principle**

History rewriting does not make a leaked credential safe. Existing copies may still be available in forks, clones, caches or other systems.

When a secret has been exposed:

1. Revoke or rotate the credential.
2. Investigate possible misuse.
3. Rewrite repository history when appropriate.
4. Coordinate repository synchronization with contributors.

# 55. Signed Commits

Signed commits use cryptographic signatures to provide a means of verifying that a commit was created using a particular trusted signing key.

**Benefits**

* Commit integrity verification.
* Accountability.
* Verification of the signing identity.

**Security considerations**

Signed commits require appropriate key management, key protection, organizational policies and verification enforcement.

A valid signature establishes that a particular key signed a commit; it does not, by itself, prove that the code is secure or that the key has not been compromised.

# 56. GitHub Actions Security

GitHub Actions is a CI/CD automation platform that allows workflows to be triggered by repository events, such as commits and pull requests.

Insecure workflow configurations can expose secrets, enable unauthorized actions or introduce supply-chain risks.

**Common security risks**

* Overly permissive workflow tokens.
* Untrusted third-party actions.
* Secrets exposed to untrusted pull requests.
* Command injection.
* Unsafe pull request event handling.
* Unpinned actions.
* Compromised runners.

**Security controls**

* Apply least-privilege token permissions.
* Pin actions to trusted commit SHAs.
* Protect important branches.
* Use environment approvals for sensitive deployments.
* Protect and mask secrets.
* Use secure and appropriately isolated runners.
* Carefully handle pull request events and untrusted input.

CI/CD pipelines should be treated as security-sensitive infrastructure because they may have access to source code, deployment credentials and production environments.

---

# 57. Core DevSecOps Security Principles

DevSecOps integrates security into development, delivery and operations. The following principles provide a foundation for designing and maintaining secure systems.

## 57.1 Least Privilege

The principle of least privilege states that every user, service, application, CI job and cloud identity should receive only the permissions necessary to perform its assigned tasks.

**Example**

An application that only needs to retrieve records from a database should receive read-only database permissions rather than administrative access.

```text
Application
     |
     v
Database
     |
     v
Read Access Only
     |
     v
No Administrative Access
```

This reduces the potential impact of compromised accounts, services or workloads.

## 57.2 Defense in Depth

Defense in depth is a security strategy that uses multiple independent or complementary security controls rather than relying on a single protective mechanism.

Example:

```text
CDN / WAF
    |
    v
Firewall
    |
    v
Reverse Proxy
    |
    v
Application Authentication
    |
    v
Authorization
    |
    v
Database Controls
    |
    v
Logging and Monitoring
```

If one control fails, other layers may still prevent or detect unauthorized activity.

## 57.3 Attack Surface Reduction

Attack surface reduction involves minimizing the number of exposed components, services, interfaces and permissions that an attacker could potentially target.

**Examples**

* Close unnecessary network ports.
* Disable unused services.
* Remove redundant packages.
* Avoid exposing SSH directly to the internet.
* Keep internal services on private networks.
* Restrict cloud security rules.

Reducing exposure can lower the number of potential entry points into a system.

## 57.4 Secure by Default

Secure by default means that systems should start with restrictive security settings rather than relying on users to enable protection manually.

**Examples**

* Private networking by default.
* Least-privilege permissions.
* Secure cookie attributes.
* Protected repository branches.
* Minimal container privileges.

This approach helps prevent insecure configurations from becoming the default operating state.

## 57.5 Shift Left

Shift left is the practice of integrating security activities early in the software development lifecycle, instead of waiting until final testing or production deployment.

**Typical security workflow**

```text
Code
  |
  v
Pull Request
  |
  v
SAST / SCA / Secret Scan / IaC Scan
  |
  v
Build
  |
  v
Container Scan
  |
  v
Deploy
  |
  v
Runtime Monitoring
```

Security checks can identify vulnerabilities earlier, allowing developers to resolve them before they progress through the delivery pipeline.

Shift left complements runtime protection and monitoring rather than replacing them.

---

# 58. Important Security Relationships

## 58.1 Client vs. Server

| Client                                                 | Server                                                     |
| ------------------------------------------------------ | ---------------------------------------------------------- |
| Requests services                                      | Provides services                                          |
| Commonly a browser or mobile application               | Commonly a backend or API                                  |
| Operates in an environment that may be user-controlled | Operates in an environment controlled by the service owner |
| Client-side checks can be bypassed                     | Must enforce critical validation and authorization         |

The server must independently verify every security-sensitive operation, regardless of client-side restrictions.

## 58.2 HTTP vs. HTTPS

| HTTP                                        | HTTPS                                      |
| ------------------------------------------- | ------------------------------------------ |
| Application-layer protocol                  | HTTP protected by TLS                      |
| Does not inherently encrypt data in transit | Encrypts data in transit                   |
| No TLS protection                           | Provides TLS confidentiality and integrity |
| Commonly uses port 80                       | Commonly uses port 443                     |

HTTPS protects communication in transit, but it does not eliminate vulnerabilities in application logic, authentication or authorization.

## 58.3 TCP vs. UDP

| TCP                                  | UDP                                      |
| ------------------------------------ | ---------------------------------------- |
| Connection-oriented                  | Connectionless                           |
| Reliable delivery                    | No built-in delivery guarantee           |
| Preserves data order                 | No built-in ordering guarantee           |
| Retransmits lost data                | No built-in retransmission               |
| Commonly used by HTTP, HTTPS and SSH | Commonly used by DNS, DHCP and streaming |

The choice between TCP and UDP depends on the application's requirements for reliability, latency and communication overhead.

## 58.4 Proxy vs. Reverse Proxy

| Proxy                                                        | Reverse Proxy                                             |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| Acts as an intermediary for clients                          | Acts as an intermediary for backend servers               |
| Client → Proxy → Server                                      | Client → Reverse Proxy → Server                           |
| Often used for client-side traffic management and inspection | Often used for backend routing, protection and management |
| Burp Suite can act as an intercepting proxy                  | Nginx can act as a reverse proxy                          |

A forward proxy primarily represents or serves clients, whereas a reverse proxy provides a controlled entry point to backend services.

## 58.5 Authentication vs. Authorization

**Authentication** verifies who a user or system is.

**Authorization** determines what an authenticated identity is allowed to access or perform.

Example:

```text
User Login
    |
    v
Authentication
    |
    v
Identity Verified
    |
    v
Authorization
    |
    v
Check Permission
    |
    v
Allow or Deny Access to /admin
```

A user may successfully authenticate but still be denied access to an administrative resource because they do not have the required permissions.

---

# Conclusion

DevSecOps combines software development, security and operations to integrate security throughout the application lifecycle. Its foundations include understanding computer systems, networking, web applications, Linux, source control and CI/CD workflows.

The central objective is to build systems that are secure by design, minimize unnecessary exposure, enforce least privilege and continuously detect and address security risks.

A DevSecOps engineer applies these principles across the development process, infrastructure, deployment pipelines and production environments so that security is treated as an ongoing responsibility rather than a final-stage activity.
