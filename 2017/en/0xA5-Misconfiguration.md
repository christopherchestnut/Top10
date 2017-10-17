# A5 Security Misconfiguration

| Threat agents/Attack vectors | Security Weakness           | Impacts               |
| -- | -- | -- |
| Access Lvl \| Exploitability | Prevalence \| Detectability | Technical \| Business |
| Even Anonymous attackers access default accounts, unused pages, unpatched flaws, unprotected files and directories, etc. to gain unauthorized access to or knowledge of the system. | Security misconfiguration can happen at any level of an application stack, including the platform, web server, application server, database, frameworks, and custom code. Automated scanners are useful for detecting  misconfigurations, use of default accounts or configurations, unnecessary services, legacy options etc. | Such flaws frequently give attackers unauthorized access to some system data or functionality. Occasionally, such flaws result in a complete system compromise. The business impact depends on the protection needs of your application and data. |


## Am I vulnerable to attack?

Is your application missing the proper security hardening across any part of the application stack? Including:

1. Are any unnecessary features enabled or installed (e.g., ports, services, pages, accounts, privileges)?

2. Are default accounts and their passwords still enabled and unchanged?

3. Does your error handling reveal stack traces or other overly informative error messages to users?

4. Do you still use ancient configs with updated software?
Do you adhere on obsolete backward compatibility?

5. Are the security settings in your application servers, application frameworks (e.g., Struts, Spring, ASP.NET), libraries, databases, etc. not set to secure values?

6. Does the server not send any security directives to client agents (e.g. in headers) or are they not set to secure values?

7. Is any of your software out of date? (see 2017-A9)

Without a concerted, repeatable application security configuration process, systems are at a higher risk.

## How do I prevent

The primary recommendations are to establish all of the following:
1. A repeatable hardening process that makes it fast and easy to deploy another environment that is properly locked down. Development, QA, and production environments should all be configured identically (with different passwords used in each environment). This process should be automated to minimize the effort required to setup a new secure environment.

2. A process for keeping abreast of and deploying all new software updates and patches in a timely manner to each deployed environment. This process needs to include all components and libraries as well (see 2017-A9). Get get accustomed to new security features.

3. A strong application architecture that provides effective, secure separation between components.

4. An automated process to verify independently the effectiveness of the configurations and settings in all environments.

## Example Scenarios

Scenario #1: The app server admin console is automatically installed and not removed. Default accounts aren't changed. Attacker discovers the standard admin pages are on your server, logs in with default passwords, and takes over.

Scenario #2: Directory listing is not disabled on your web server. An attacker discovers they can simply list directories to find any file. The attacker finds and downloads all your compiled Java classes, which they decompile and reverse engineer to get all your custom code. Attacker then finds a serious access control flaw in your application.

Scenario #3: App server configuration allows stack traces to be returned to users, potentially exposing underlying flaws such as framework versions that are known to be vulnerable.

Scenario #4: App server comes with sample applications that are not removed from your production server. These sample applications have well known security flaws attackers can use to compromise your server.

Scenario #5: The default configuration or a copied old one activates old vulnerable protocol versions or options that can be misused by an attacker or malware.

## References

### OWASP

* [OWASP Development Guide: Chapter on Configuration]()
* [OWASP Code Review Guide: Chapter on Error Handling]()
* [OWASP Testing Guide: Configuration Management]()
* [OWASP Testing Guide: Testing for Error Codes]()
For additional requirements in this area, see the [ASVS requirements areas for Security Configuration (V11 and V19)]().
* TBD:
* [OWASP Proactive Controls - TBA]()
* [OWASP Secure Headers Project - TBA]()
* [OWASP Application Security Verification Standard - TBA]()
* [OWASP Testing Guide - TBA]()
* [OWASP Cheat Sheet - TBA]()

### External

* NIST Guide to General Server Hardening
* CWE Entry 2 on Environmental Security Flaws
* CIS Security Configuration Guides/Benchmarks
