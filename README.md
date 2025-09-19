# 🛡️ Enhanced Shai-Hulud Security Scanner Pipeline

[![Azure DevOps](https://img.shields.io/badge/Azure%20DevOps-Pipeline-blue)](https://azure.microsoft.com/en-us/services/devops/)
[![Security](https://img.shields.io/badge/Security-npm%20Supply%20Chain-red)](https://github.com/advisories)
[![Version](https://img.shields.io/badge/Version-2.0.0-green)](https://github.com/your-org/shai-hulud-scanner)

> **Advanced multi-stage security scanning for npm package compromise detection with threat intelligence, SBOM generation, and comprehensive reporting.**

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Quick Start](#-quick-start)
- [Pipeline Stages](#-pipeline-stages)
- [Configuration](#-configuration)
- [Threat Intelligence](#-threat-intelligence)
- [Security Reports](#-security-reports)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [Security](#-security)

## 🔍 Overview

The Enhanced Shai-Hulud Security Scanner is an enterprise-grade Azure DevOps pipeline designed to protect your development environment from npm supply chain attacks. Named after the threat identified by Palo Alto Networks Unit 42, this scanner detects compromised packages, malicious artifacts, and security vulnerabilities across your codebase.

### 🎯 Why This Matters

The [Shai-Hulud worm](https://unit42.paloaltonetworks.com/shai-hulud-supply-chain-attack/) represents a significant evolution in supply chain threats, compromising over 180 npm packages through automated propagation. This pipeline provides comprehensive protection against such attacks and maintains up-to-date threat intelligence.

## ✨ Features

### 🔧 Core Security Capabilities
- **Multi-stage security scanning** with parallel execution
- **Real-time threat intelligence** from GitHub Security Advisories
- **Comprehensive package detection** across filesystem and dependencies
- **Malware scanning** with hash-based detection
- **SBOM generation** for compliance and auditing
- **Automated reporting** with detailed findings

### 🌐 Threat Intelligence Sources
- ✅ **Static threat database** (your `compromised.txt` file)
- ✅ **GitHub Security Advisories** (live API integration)
- ✅ **npm audit database** (fallback source)
- ✅ **Automatic deduplication** and sorting

### 📊 Enterprise Features
- **Configurable parameters** for different environments
- **Agent pool compatibility** with existing infrastructure
- **Artifact management** for stage-to-stage data sharing
- **Parallel scanning** for improved performance
- **Comprehensive logging** and debugging

## 🚀 Quick Start

### Prerequisites
- Azure DevOps organization with pipeline permissions
- Self-hosted agent pool (default: `clh-pool`) or Microsoft-hosted agents
- `compromised.txt` file in your repository root

### Basic Setup

1. **Add the pipeline file** to your repository:
   ```yaml
   # Save as azure-pipeline-enhanced.yml in your repo root
   ```

2. **Create your threat database**:
   ```bash
   # Create compromised.txt with known threats
   echo "@ctrl/tinycolor@99.99.99" > compromised.txt
   ```

3. **Create a new pipeline** in Azure DevOps:
   - Go to Pipelines → New Pipeline
   - Choose your repository
   - Select "Existing Azure Pipelines YAML file"
   - Point to `/azure-pipeline-enhanced.yml`

4. **Run the pipeline**:
   - Pipeline will automatically trigger on `main`, `develop`, or `release/*` branches
   - Or manually trigger with custom parameters

### 📱 Quick Test
```bash
# The pipeline includes a built-in test package to verify functionality
# Look for "✅ Test environment created successfully" in the logs
```

## 🏗️ Pipeline Stages

### Stage 0: 🔧 Pipeline Initialization
**Duration**: ~1-2 minutes
- Environment validation and setup
- Tool availability checks (Node.js, npm, jq, curl)
- Compromised packages file validation

### Stage 1: 📡 Threat Intelligence Gathering
**Duration**: ~2-3 minutes
- Loads your existing `compromised.txt` file (516+ threats)
- Fetches latest threats from GitHub Security Advisories
- Attempts fallback to npm audit API if GitHub is unavailable
- Deduplicates and sorts final threat list (typically 520+ threats)

### Stage 2: 🧪 Controlled Testing Environment
**Duration**: ~1 minute
- Creates test packages for scanner validation
- Ensures detection mechanisms are working
- Prepares controlled environment for security testing

### Stage 3: 🔍 Parallel Security Scanning
**Duration**: ~5-10 minutes (parallel execution)

#### 🗂️ Filesystem Security Scan
- Scans Node.js installation directories
- Checks for installed compromised packages
- Smart path exclusion to avoid system directories

#### 📦 Dependency Security Scan
- Analyzes `package.json`, `yarn.lock`, `pnpm-lock.yaml`
- Detects compromised packages in dependency declarations
- Validates locked dependency versions

#### 🦠 Malware & Artifact Scan
- Searches for known malicious files (`shai-hulud-workflow.yml`)
- Hash-based detection of malicious bundles
- Pattern matching for suspicious artifacts

### Stage 4: 📋 SBOM Generation
**Duration**: ~1-2 minutes
- Generates Software Bill of Materials in SPDX format
- Extracts dependency information for compliance
- Creates audit trail for security reviews

### Stage 5: 📊 Security Report Generation
**Duration**: ~1 minute
- Comprehensive Markdown reports
- Threat summary tables
- Detailed findings with remediation guidance
- Publishes artifacts for download

### Stage 6: 🚨 Alerting (Only on Threats)
**Duration**: ~1 minute
- Triggers only when threats are detected
- Extensible for integration with notification systems
- Provides build URLs and artifact links

### Stage 7: 🧹 Cleanup
**Duration**: ~30 seconds
- Removes temporary test packages
- Cleans sensitive data while preserving reports
- Ensures clean environment for next run

## ⚙️ Configuration

### Pipeline Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `agentPool` | string | `clh-pool` | Agent pool for pipeline execution |
| `threatFeedUrl` | string | `https://api.github.com/advisories` | External threat intelligence source |
| `scanDepth` | number | `3` | Maximum directory depth for filesystem scanning |
| `timeoutMinutes` | number | `15` | Timeout for individual scan jobs |
| `enableParallelScans` | boolean | `true` | Enable parallel execution of security scans |
| `generateSBOM` | boolean | `true` | Generate Software Bill of Materials |

### Environment Variables

```yaml
variables:
  - name: compromisedPackagesFile
    value: 'compromised.txt'  # Your threat database file
  - name: pipelineVersion
    value: '2.0.0'           # Scanner version
```

### Customizing for Your Environment

#### Using Different Agent Pools
```yaml
# For Microsoft-hosted agents
parameters:
  - name: agentPool
    default: 'ubuntu-latest'

# For specific self-hosted pools
parameters:
  - name: agentPool
    default: 'your-custom-pool'
```

#### Adjusting Scan Sensitivity
```yaml
# More aggressive scanning
parameters:
  - name: scanDepth
    default: 5
  - name: timeoutMinutes
    default: 30

# Faster scanning for CI
parameters:
  - name: scanDepth
    default: 2
  - name: timeoutMinutes
    default: 10
```

## 🧠 Threat Intelligence

### Static Threat Database
Your `compromised.txt` file should contain one threat per line in the format:
```
@package/name@version
package-name@version
```

Example:
```
@ctrl/tinycolor@4.1.1
@ctrl/tinycolor@4.1.2
malicious-package@1.0.0
```

### Dynamic Threat Feeds

#### GitHub Security Advisories
- **URL**: `https://api.github.com/advisories`
- **Format**: JSON API with vulnerability details
- **Parsing**: Extracts npm-specific advisories automatically
- **Fallback**: Continues with static database if API fails

#### npm Audit Database
- **URL**: `https://registry.npmjs.org/-/npm/v1/security/advisories`
- **Purpose**: Secondary source when GitHub API is unavailable
- **Integration**: Automatic fallback mechanism

### Threat Intelligence Flow
```
Static Database (516) → GitHub API (+7) → Deduplication → Final List (521)
```

## 📊 Security Reports

### Scan Summary Report
Generated as `security-summary.md` with:
- ✅ **Clean status** for each scan component
- ❌ **Threat details** if any are found
- 📈 **Statistics** and threat counts
- 🔗 **Build information** and timestamps

### Example Clean Report
```markdown
# 🛡️ Shai-Hulud Security Scan Report

**Repository:** your-repo-name
**Build ID:** 123
**Scan Date:** 2025-09-19-14-30-15
**Scanner Version:** 2.0.0

## 📋 Scan Summary

| Component | Status | Details |
|-----------|--------|---------|
| Filesystem Scan | ✅ CLEAN | 0 threats detected |
| Dependency Scan | ✅ CLEAN | 0 threats detected |
| Malware Scan | ✅ CLEAN | Suspicious artifacts check |
```

### Artifacts Published
- `security-reports/` - Complete scan results
- `security-reports/final-threats.txt` - Final threat database used
- `security-reports/sbom/` - Software Bill of Materials
- `security-reports/final/security-summary.md` - Executive summary

## 🔧 Troubleshooting

### Common Issues

#### ❌ "Invalid compromised packages file format"
**Solution**: Ensure your `compromised.txt` follows the correct format:
```bash
# Good format
@package/name@1.0.0
package-name@2.0.0

# Bad format (missing version)
@package/name
package-name
```

#### ❌ "Could not fetch external threat intelligence"
**Cause**: Network restrictions or GitHub API rate limits
**Solution**: 
- Check firewall rules for `api.github.com`
- Pipeline continues with static database
- Consider using authenticated GitHub API calls for higher rate limits

#### ❌ "Agent pool not found"
**Solution**: Update the `agentPool` parameter:
```yaml
parameters:
  - name: agentPool
    default: 'your-existing-pool'
```

#### ❌ "jq not available"
**Cause**: Missing `jq` tool on build agent
**Solution**:
- Install `jq` on self-hosted agents
- Or use Microsoft-hosted agents (have `jq` pre-installed)

### Performance Optimization

#### Faster Scans
```yaml
# Reduce scan depth for faster execution
parameters:
  - name: scanDepth
    default: 2
  - name: timeoutMinutes
    default: 8
```

#### More Thorough Scans
```yaml
# Increase depth for comprehensive scanning
parameters:
  - name: scanDepth
    default: 5
  - name: timeoutMinutes
    default: 20
```

### Debugging

#### Enable Verbose Logging
The pipeline includes comprehensive logging by default. Look for these log sections:
- 🔍 Environment audit information
- 📊 Threat intelligence statistics
- 🔍 Detailed scanning progress
- 📈 Final threat counts and summaries

#### Check Artifacts
Download the published artifacts to review:
- Threat lists used during scanning
- Detailed scan results per component
- Generated SBOM for dependency analysis

## 🤝 Contributing

### Reporting Issues
1. Check existing issues in the repository
2. Provide pipeline logs and error messages
3. Include your `compromised.txt` format and agent environment
4. Specify Azure DevOps version and agent type

### Enhancing Threat Intelligence
1. **Add new threat sources**: Extend the threat intelligence gathering stage
2. **Improve parsing logic**: Enhance jq filters for better API data extraction
3. **Add scan types**: Implement additional security checks

### Code Contributions
1. Fork the repository
2. Create feature branch (`feature/new-scan-type`)
3. Test with your Azure DevOps environment
4. Submit pull request with detailed description

## 🔒 Security

### Secure Configuration
- **Rotate credentials** regularly (npm tokens, GitHub PATs)
- **Use managed identities** where possible
- **Restrict agent pool access** to authorized users
- **Monitor pipeline execution** for unusual activity

### Threat Response
If the pipeline detects threats:
1. **Stop deployments** immediately
2. **Isolate affected systems**
3. **Rotate all credentials** (npm, GitHub, cloud)
4. **Audit recent changes** and commits
5. **Review GitHub repositories** for unauthorized "Shai-Hulud" repos

### Best Practices
- **Run scans on every commit** to main branches
- **Include in pull request validation**
- **Monitor threat intelligence updates**
- **Regularly update your `compromised.txt` file**
- **Review scan reports** for trends and patterns

## 📚 References

- [Palo Alto Networks Unit 42 - Shai-Hulud Analysis](https://unit42.paloaltonetworks.com/shai-hulud-supply-chain-attack/)
- [GitHub Security Advisories API](https://docs.github.com/en/rest/security-advisories)
- [npm Security Best Practices](https://docs.npmjs.com/about-security-best-practices)
- [SPDX Specification](https://spdx.github.io/spdx-spec/)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Documentation**: This README and inline pipeline comments
- **Issues**: GitHub Issues for bug reports and feature requests
- **Security**: Report security vulnerabilities through responsible disclosure

---

> **⚠️ Important**: This scanner is designed to detect known threats and provide early warning. It should be part of a comprehensive security strategy that includes regular updates, credential rotation, and security awareness training.

**Made with ❤️ for the open-source community | Keep your supply chain secure! 🛡️**
