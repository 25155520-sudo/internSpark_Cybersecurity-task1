Cybersecurity Website Security Assessment using Nikto and Nmap
Overview:
This project demonstrates a basic cybersecurity assessment performed on a web application and its host system using two widely used security scanning tools:
1.Nikto Web Vulnerability Scanner
2.Nmap Network Scanner
The objective of the assessment was to identify common web application vulnerabilities, exposed services, insecure configurations, and potential attack surfaces.
The assessment focused on:
Web server misconfigurations
Information disclosure issues
Exposed directories
HTTP security weaknesses
Open ports and exposed services
Network-level exposure

Tools Used:
Tool	                         Purpose
Nikto	                         Web server vulnerability scanning
Nmap	                         Network and port scanning
Browser                        Testing	Manual verification of findings

Scope of Assessment:
The assessment covered:
Web application exposure on Port 3000
HTTP response headers
Hidden and publicly accessible directories
Allowed HTTP methods
Open TCP services on the host machine
System-level exposed services

Nikto Scan Findings:
1. Information Disclosure via ETags
Risk Level:
Low
Description:
The web server leaks internal inode information through ETag headers. This may help attackers gather information about the underlying file system and server configuration.
Evidence:
Server leaks inodes via ETags
Remediation:
Disable ETags if not required
Configure ETags securely
Prevent unnecessary metadata exposure

2. Overly Permissive CORS Policy
Risk Level:
Medium
Description:
The server allows unrestricted cross-origin access using: "Access-Control-Allow-Origin: *"
This configuration may allow unauthorized websites to access application resources.
Remediation:
Restrict allowed origins to trusted domains
Avoid using wildcard (*) policies in production

3. Exposure of Hidden Directories
Risk Level:
Medium
Description:
The robots.txt file revealed hidden application paths that were publicly accessible.
Evidence:
"/ftp/" returned HTTP 200
"/public/" returned HTTP 200
Analysis:
Accessible hidden directories may expose:
Internal files
Backup data
Application resources
Sensitive content
Remediation:
Restrict public access to sensitive directories
Disable directory listing
Avoid exposing sensitive paths in robots.txt

4. Informational Header Exposure
Risk Level:
Low
Description:
The application exposes informational HTTP headers including:
x-recruiting: /#/jobs
feature-policy
These headers may reveal internal application information.
Remediation:
Remove unnecessary headers
Limit information disclosure through HTTP responses

5. Missing Strong Server Identification
Risk Level:
Low
Description:
No clear server banner was identified during scanning. While this reduces fingerprinting opportunities, inconsistent server configuration may still exist.
Remediation:
Ensure secure and consistent server configuration
Minimize unnecessary service exposure

6. Broad HTTP Methods Allowed
Risk Level:
Medium
Description:
The server allows multiple HTTP methods including:
"GET, POST, PUT, DELETE, PATCH"
Allowing unnecessary methods increases the application's attack surface.
Remediation:
Restrict methods to only those required
Disable unused HTTP verbs

Nmap Scan Findings
1. Web Application Exposure (Port 3000)
Risk Level:
Medium
Description:
The web application was accessible on Port 3000 and returned an active HTTP response.
Evidence:
"HTTP/1.1 200 OK"
Analysis:
Publicly accessible application ports may increase exposure to:
Unauthorized access attempts
Automated scanning
Exploitation attempts
Remediation:
Restrict access using firewall rules
Deploy behind a reverse proxy
Limit exposure to trusted networks
2. Unrecognized Service on Port 3000
Risk Level:
Low
Description:
The service running on Port 3000 could not be fully identified by the scanner, indicating a non-standard or custom configuration.
Remediation:
Ensure proper service identification
Maintain monitoring and logging
Verify secure configuration

3. SMB Service Exposure (Port 445)
Risk Level:
High
Description:
Port 445 was open on the host system, exposing SMB services.
Analysis:
SMB exposure is commonly targeted in attacks involving:
Unauthorized file access
Lateral movement
Worm propagation
Ransomware attacks
Note:
This service belongs to the host operating system and is outside the web application itself.
Remediation:
Disable SMB if unnecessary
Restrict SMB access using firewall rules
Limit access to trusted internal networks only

4. RPC Service Exposure (Port 135)
Risk Level:
Medium
Description:
Port 135 was open for RPC communication.
Analysis:
RPC services may expose system-level interfaces that attackers can probe or exploit if improperly secured.
Note:
This service originates from the host operating system rather than the web application.
Remediation:
Restrict access to trusted networks
Block unnecessary external access
Harden RPC-related services
Additional Security Observation
Additional open ports including:
Port 135 (RPC)
Port 445 (SMB)
were identified on the host system, indicating system-level exposure beyond the application scope.

Risk Summary:
Finding	                                   Risk Level
SMB Exposure (Port 445)	                   High
Open Web Application Port (3000)	         Medium
Broad HTTP Methods Allowed	               Medium
Exposed Hidden Directories	               Medium
Permissive CORS Policy	                   Medium
RPC Exposure (Port 135)	                   Medium
Information Disclosure via ETags	         Low
Informational Header Exposure	             Low
Unrecognized Service	                     Low

Recommendations:
Web Application Security
Restrict unnecessary HTTP methods
Harden CORS policies
Remove unnecessary headers
Avoid exposing internal directories
Disable directory listing
Network Security
Restrict externally accessible ports
Disable unused services
Harden SMB and RPC exposure
Deploy firewall filtering rules
Monitoring and Hardening
Perform regular vulnerability assessments
Conduct periodic port scans
Enable logging and monitoring
Apply server hardening best practices

Conclusion:
The security assessment successfully identified several web application and host-level security weaknesses using Nikto and Nmap scanning techniques.
The findings demonstrated common security issues including:
Information disclosure
Excessive HTTP permissions
Exposed services
Accessible hidden directories
Network service exposure
Although no critical web application compromise was identified, exposed services such as SMB and permissive configurations increase the attack surface and should be remediated to improve overall security posture.
The assessment highlights the importance of continuous security testing, service hardening, and proactive vulnerability management.
