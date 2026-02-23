# OpsZ Security Model

**Last updated:** February 2026

Security is fundamental to OpsZ, not an add-on. This document outlines our security architecture, practices, and compliance readiness.

---

## Security Principles

### 1. Zero-Trust Architecture

OpsZ is built on zero-trust principles from the ground up:

- **No implicit trust** — Every request is authenticated and authorized
- **Outbound-only agents** — No inbound connections or listening ports
- **Short-lived credentials** — Tokens expire automatically, no static secrets
- **Least privilege** — Fine-grained permissions for every operation
- **Comprehensive audit** — Every action is logged and traceable

### 2. Defense in Depth

Multiple layers of security protect your infrastructure:

- **Network security** — TLS encryption for all communication
- **Authentication** — Multiple factors and identity provider integration
- **Authorization** — Role-based access control with approval workflows
- **Data security** — Encryption at rest and in transit
- **Audit logging** — Complete activity records for forensics

### 3. Customer Data Control

Your data stays under your control:

- **On-premises deployment** — Run OpsZ in your own environment
- **No external dependencies** — Platform runs air-gapped if needed
- **Data residency** — Choose where data is stored
- **No vendor lock-in** — Standard formats and export capabilities

---

## Agent Security

### Outbound-Only Design

OpsZ agents make no inbound connections:

- **No listening ports** — Agents don't accept incoming connections
- **No firewall rules** — No need to open inbound ports
- **Outbound TLS only** — All communication is agent-initiated
- **NAT-friendly** — Works through NAT and firewalls without configuration

### Certificate-Based Authentication

Agents authenticate using strong cryptography:

- **Asymmetric cryptography** — Public/private key pairs
- **Certificate enrollment** — Secure initial provisioning
- **Certificate rotation** — Automatic renewal before expiration
- **Revocation support** — Instantly revoke compromised certificates

### Minimal Attack Surface

Agents are designed for security:

- **Single binary** — No dependencies or scripts to secure
- **Minimal permissions** — Runs with least privilege required
- **No shell access** — Agents don't provide shell access to managed systems
- **Read-only by default** — Write operations require explicit authorization

---

## Authentication

### Identity Provider Integration

Integrate with your existing identity systems:

- **OAuth 2.0 / SAML** — Standards-based SSO
- **Okta, Azure AD, Google** — Major identity providers supported
- **LDAP / Active Directory** — Enterprise directory integration
- **Multi-factor authentication** — Enforce MFA for sensitive operations

### Token-Based Security

Modern token architecture for scalability and security:

- **Short-lived tokens** — Automatic expiration (configurable duration)
- **Encrypted tokens** — Token contents are encrypted
- **Signed tokens** — Tamper-proof cryptographic signatures
- **Local validation** — No database lookup required (fast and scalable)
- **Automatic rotation** — Seamless token renewal

### No Static Credentials

OpsZ eliminates static credentials:

- **No passwords on managed systems** — Agents use certificates
- **No API keys in code** — Token-based authentication
- **No shared secrets** — Unique credentials per entity
- **Automatic cleanup** — Revoke access when users leave

---

## Authorization

### Fine-Grained RBAC

Control access at the action level:

- **Action-based permissions** — create, read, update, delete, execute, approve
- **Resource-based permissions** — jobs, workflows, hosts, users, etc.
- **Custom roles** — Define roles matching your organization
- **Role hierarchies** — Inherit permissions through role structure
- **Group-based access** — Assign roles to groups, not just individuals

### Approval Workflows

Multi-level approval for sensitive operations:

- **Configurable approval chains** — Define who must approve what
- **Automated routing** — Approvals go to the right people
- **Audit trail** — Complete history of approval decisions
- **Time limits** — Approvals expire if not acted upon
- **Escalation** — Automatic escalation for overdue approvals

### Temporary Access

Grant time-limited access for specific needs:

- **Just-in-time access** — Grant access only when needed
- **Automatic expiration** — Access revokes automatically
- **Access requests** — Self-service request workflows
- **Emergency access** — Break-glass procedures with full audit

---

## Data Security

### Encryption in Transit

All network communication is encrypted:

- **TLS 1.2+** — Modern encryption protocols
- **Perfect forward secrecy** — Past communications remain secure
- **Certificate validation** — Prevent man-in-the-middle attacks
- **Mutual TLS** — Both client and server authenticate

### Encryption at Rest

Sensitive data is encrypted when stored:

- **Database encryption** — Protect data at storage layer
- **Key management** — Secure key storage and rotation
- **Encrypted backups** — Backups are encrypted
- **Configurable algorithms** — Use approved encryption standards

### Data Minimization

Collect only what you need:

- **Configurable collection** — Choose what data to collect
- **Retention policies** — Automatically delete old data
- **Data masking** — Hide sensitive data in logs
- **PII handling** — Special handling for personal data

---

## Audit & Compliance

### Comprehensive Audit Logging

Every action is recorded:

- **User actions** — API calls, UI interactions, CLI commands
- **System actions** — Workflow execution, agent activity, background jobs
- **Authentication events** — Login, logout, token issuance
- **Authorization events** — Permission checks, approvals, denials
- **Data changes** — What changed, when, and by whom

### Audit Trail Features

Rich audit capabilities:

- **Searchable logs** — Query by user, action, resource, time
- **Tamper-proof logs** — Logs cannot be modified or deleted
- **Long-term retention** — Configurable retention periods
- **Export capabilities** — Export for external SIEM or analysis
- **Real-time alerts** — Notify on suspicious activity

### Compliance Readiness

Built to support regulatory requirements:

- **SOC 2** — Security and availability controls
- **ISO 27001** — Information security management
- **GDPR** — Data privacy and protection
- **HIPAA** — Healthcare data security (when deployed on-premises)
- **PCI DSS** — Payment card industry security

**Note:** Compliance certifications depend on deployment model and configuration. Contact us for compliance guidance.

---

## Network Security

### Zero-Trust Networking

No trust based on network location:

- **Authenticate everything** — Network location doesn't imply trust
- **Encrypt all traffic** — Even internal communication is encrypted
- **Micro-segmentation** — Limit blast radius of compromises
- **Network policies** — Fine-grained network access control (Kubernetes)

### Firewall Requirements

Minimal firewall configuration:

**For agents (managed systems):**
- Outbound HTTPS (TCP 443) to control plane
- No inbound ports required

**For control plane:**
- Inbound HTTPS (TCP 443) for UI and API
- Inbound agent connections (TCP 443 or configured port)
- No other inbound access required

### DDoS Protection

Protect against denial of service:

- **Rate limiting** — Limit requests per user/IP
- **Connection limits** — Prevent connection exhaustion
- **Request validation** — Reject malformed requests
- **Load balancing** — Distribute load across replicas

---

## Vulnerability Management

### Secure Development

Security built into development process:

- **Code review** — All code reviewed before merge
- **Static analysis** — Automated security scanning
- **Dependency scanning** — Check for vulnerable libraries
- **Secret scanning** — Prevent accidental credential commits
- **Security testing** — Regular penetration testing

### Vulnerability Response

Rapid response to security issues:

- **Security updates** — Quick patches for vulnerabilities
- **Coordinated disclosure** — Responsible disclosure process
- **Security advisories** — Notify customers of issues
- **Automatic updates** — Agents update automatically (optional)

### Third-Party Security

Vet dependencies and integrations:

- **Dependency audits** — Regular review of third-party libraries
- **Minimal dependencies** — Only essential dependencies included
- **Vendor security** — Evaluate security of external services
- **Supply chain security** — Verify integrity of software supply chain

---

## Incident Response

### Security Monitoring

Continuous security monitoring:

- **Anomaly detection** — Identify unusual behavior
- **Failed login monitoring** — Detect brute force attempts
- **Privilege escalation detection** — Alert on suspicious privilege use
- **Data exfiltration detection** — Monitor for unusual data access

### Incident Handling

Prepared for security incidents:

- **Incident response plan** — Documented procedures
- **Forensic logging** — Detailed logs for investigation
- **Containment procedures** — Rapidly contain breaches
- **Communication plan** — Customer notification procedures

---

## Security Best Practices

### For Administrators

Recommendations for running OpsZ securely:

1. **Enable MFA** for all users, especially administrators
2. **Use SSO** integration with your identity provider
3. **Implement approval workflows** for sensitive operations
4. **Regular access reviews** — Remove unnecessary permissions
5. **Monitor audit logs** — Review logs regularly for anomalies
6. **Keep software updated** — Apply security updates promptly
7. **Restrict network access** — Use firewall rules and network policies
8. **Encrypt backups** — Protect backup data
9. **Test disaster recovery** — Regularly test backup/restore procedures
10. **Security training** — Train users on security best practices

### For Users

Security tips for platform users:

1. **Protect your credentials** — Don't share passwords or tokens
2. **Use strong passwords** — Or better yet, use SSO
3. **Log out when done** — Especially on shared systems
4. **Review permissions** — Only request access you need
5. **Report suspicious activity** — Contact security team if something seems wrong

---

## Security Certifications

**Current Status:**
- Security audit in progress
- Penetration testing completed Q4 2025
- SOC 2 Type 1 targeted for Q2 2026

**Future Plans:**
- SOC 2 Type 2 (Q4 2026)
- ISO 27001 certification (2027)
- Additional certifications based on customer needs

---

## Responsible Disclosure

Found a security issue? We appreciate responsible disclosure:

**Contact:** [security@opsz.io](mailto:security@opsz.io)

**PGP Key:** Available at [https://opsz.io/security/pgp-key.txt](https://opsz.io/security/pgp-key.txt)

**Response time:** We'll acknowledge within 24 hours

**Bounty program:** Coming soon

---

## Security Resources

- **Security documentation:** [https://docs.opsz.io/security](https://docs.opsz.io/security)
- **Security advisories:** [https://opsz.io/security/advisories](https://opsz.io/security/advisories)
- **Best practices guide:** [https://docs.opsz.io/security/best-practices](https://docs.opsz.io/security/best-practices)
- **Compliance documentation:** Contact your customer success manager

---

**Questions about security?** [security@opsz.io](mailto:security@opsz.io)

**Need compliance documentation?** [compliance@opsz.io](mailto:compliance@opsz.io)
