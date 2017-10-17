# A6 Access Control

| Threat agents/Attack vectors | Security Weakness           | Impacts               |
| -- | -- | -- |
| Access Lvl \| Exploitability | Prevalence \| Detectability | Technical \| Business |
| Exploitation of access control is a core skill of penetration testers. SAST and DAST tools can detect the abscense of access control, but not verify if it is functional. Access control is detectable using manual means, or possibly through automation for the abscence of access controls in certain frameworks. | Access control weaknesses are common due to the lack of automated detection, and lack of effective functional testing by application developers. | The technical impact is anonymous attackers acting as users or administrators, users using privileged functions, or creating, accessing, updating or deleting every record. |

## Am I vulnerable to attack?

Access control is the process of ensuring that users cannot act outside of their role or granted permissions, such that they can only access secured information and functionality that they are explicitly granted access. Commonly, applications fail to enforce access control in a wide variety of ways, but typically this can lead to critical unauthorized information disclosure, modification or destruction of all data within a system, or performing a business function well outside of the limits of the user. 

Common access control vulnerabilities include:

* Missing or ineffective presentation access control, accessing hidden, disabled, or privileged functionality through modifying the URL, internal app state, or the HTML page, or simply using a custom API attack tool
* Missing or ineffective controller access control, such as not checking that the web, mobile or API caller has privileges or capability to access that function
* Missing or ineffective model access control, where the primary key can be changed to another's users record, such as viewing or editing someone else's account 
* Missing or ineffective domain model access control, where the business logic should enforce limits, such as cinema booking system not permitting individuals from booking out an entire cinema
* Elevation of privilege. Acting as a user without being logged in, or acting as an admin whilst logged in as a user
* Segregation of duty violations. Such as initating and approving a business flow not normally visible to the original user
* Metadata manipulation. Where a JWT access control token can be replayed or modified, or a cookie or hidden field manipulated to elevate privileges (such as changing `role=user` cookie to `admin`)
* Spidering an application using a proxy such as OWASP Zap, whilst logged on as a high privilege user, and then testing each page and controller whilst not logged in, or logged in as a low privilege user, or if directory browsing, revision control system files and thumbnails might be available to the tool

Access control testing is not currently amenable to automated static or dynamic testing, but when identified, it is a severe attack as the attacker has spent considerable effort manually testing the access control matrix before mounting an attack. Such attackers are usually highly competent, effective, and malicous in nature.

## How do I prevent

Access control is only effective if enforced in trusted server-side code or server-less API, where the attacker cannot modify the access control check or metadata.

* Implement the priciples of deny by default and principle of complete mediation in your architecture, with the exception of public resources
* Centralized Implementation. Implement access control mechanisms once and re-use them throughout the application.
* Presentation layer access control must be enforced on trusted API endpoints or with server-side access control checks
* Controllers should enforce role-based, claims, or capability based access controls
* Model access controls should enforce record ownership, rather than accepting that the user can create, read, update or delete any record
* Domain access controls are unique to each application, but business limit requirements should be enforced by domain models
* Disable web server directory listing, and ensure file metadata such as `.git`, `.Thumbs.db` or `.DS_Store` is not present within web roots
* Log access control failures, such that alerting adminsitrators of unauthorized access is possible

Large and high performing organizations should consider:

* Implementing segregation of duties checks in risky or high value business flows
* Rate limiting API and controller access to minimize the harm from automated attack tooling
* Monitoring and escalate access control failures to operational staff as quickly as possible, particularly where access control failures are occuring extremely rapidly, such as with a scraping tool or similar

Developers and QA staff should include functional access control unit and integration tests to demonstrate that access controls are in place, in use, and effective using a variety of user principals, including anonymous access, users acting within their rights, direct object reference attacks - including creating, reading, updating and deleting records, users attempting to elevate privileges or acting outside their authority, and access control metadata attacks.

NB: Automated access control testing by SAST and DAST tools is not currently possible without providing human context. Such testing should not be relied upon to validate access controls are in place, in use and effective.

## Example Scenarios

Scenario #1: The application uses unverified data in a SQL call that is accessing account information:

<code>
  pstmt.setString(1, request.getParameter("acct"));
  ResultSet results = pstmt.executeQuery( );
</code>

An attacker simply modifies the 'acct' parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user's account.

* `http://example.com/app/accountInfo?acct=notmyacct`

Scenario #2: An attacker simply force browses to target URLs. Admin rights are also required for access to the admin page.

* `http://example.com/app/getappInfo`
* `http://example.com/app/admin_getappInfo`

If an unauthenticated user can access either page, it's a flaw. If a non-admin can access the admin page, this is also a flaw.

## References

### OWASP

* [OWASP Proactive Controls - Access Controls](https://www.owasp.org/index.php/OWASP_Proactive_Controls#6:_Implement_Access_Controls)
* [OWASP Application Security Verification Standard - V4 Access Control](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project#tab=Home)
* [OWASP Testing Guide - Access Control](https://www.owasp.org/index.php/Testing_for_Authorization)
* [OWASP Cheat Sheet - Access Control](https://www.owasp.org/index.php/Access_Control_Cheat_Sheet)

### External

* [CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')]()
* [CWE-284: Improper Access Control (Authorization)](https://cwe.mitre.org/data/definitions/284.html)
* [CWE-285: Improper Authorization](https://cwe.mitre.org/data/definitions/285.html)
* [CWE-639: Authorization Bypass Through User-Controlled Key](https://cwe.mitre.org/data/definitions/639.html)
