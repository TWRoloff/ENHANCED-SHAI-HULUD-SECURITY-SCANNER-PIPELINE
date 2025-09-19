# Security Policy

## 🛡️ Reporting Security Vulnerabilities

The Enhanced Shai-Hulud Security Scanner is designed to protect against supply chain attacks. We take security seriously and appreciate your help in keeping the project secure.

### ⚠️ Please DO NOT:
- Open public GitHub issues for security vulnerabilities
- Discuss security issues in public forums or chat rooms
- Share exploit code publicly before issues are resolved

### ✅ Please DO:
- Report security issues through responsible disclosure
- Provide clear reproduction steps
- Allow reasonable time for fixes before public disclosure

### 📧 Reporting Process
1. **Email**: Send details to the repository maintainers
2. **Include**: 
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact assessment
   - Suggested fix (if available)
3. **Response**: We'll acknowledge within 48 hours
4. **Resolution**: We aim to fix critical issues within 7 days

## 🔍 Security Scope

### In Scope
- **Pipeline execution vulnerabilities** (code injection, privilege escalation)
- **Credential exposure** in logs or artifacts
- **Malicious input handling** in threat intelligence parsing
- **Data exfiltration** through compromised scanning logic
- **Supply chain attacks** against the scanner itself

### Out of Scope
- **Azure DevOps platform vulnerabilities** (report to Microsoft)
- **Dependencies vulnerabilities** (unless directly exploitable in our context)
- **Social engineering** against users
- **Physical security** of build agents
- **Denial of service** against external APIs (GitHub, npm)

## 🚨 Supported Versions

| Version | Supported |
|---------|-----------|
| 2.x.x   | ✅ Yes     |
| 1.x.x   | ❌ No      |

## 🔧 Security Features

### Current Protections
- **Parameterized configuration** prevents injection attacks
- **Input validation** for threat database format
- **Safe file operations** with restricted paths
- **Error handling** prevents information disclosure
- **Artifact isolation** between pipeline stages

### Planned Enhancements
- **Digital signature verification** for threat intelligence
- **Encrypted credential storage** for API tokens
- **Audit logging** for security events
- **Rate limiting** for external API calls

## 🏆 Security Recognition

We acknowledge security researchers who responsibly disclose vulnerabilities:

### Hall of Fame
*Contributors who have helped improve our security will be listed here.*

### Rewards
While we don't offer monetary rewards, we provide:
- **Public recognition** (with permission)
- **CVE credit** for significant vulnerabilities
- **Collaboration opportunities** on security improvements

## 📚 Security Resources

- [OWASP CI/CD Security Guide](https://owasp.org/www-project-devsecops-guideline/)
- [Azure DevOps Security Best Practices](https://docs.microsoft.com/en-us/azure/devops/organizations/security/)
- [Supply Chain Security Framework](https://slsa.dev/)
- [npm Security Advisories](https://docs.npmjs.com/about-security-advisories)

## 🔄 Security Updates

### Notification Channels
- **GitHub Security Advisories**: For published vulnerabilities
- **Release Notes**: For security-related updates
- **README Updates**: For configuration security improvements

### Update Process
1. **Critical**: Immediate patch release
2. **High**: Patch within 7 days
3. **Medium**: Included in next minor release
4. **Low**: Included in next major release

---

**Remember**: Security is a shared responsibility. Help us keep the npm ecosystem safe! 🛡️
