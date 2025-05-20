---
title: puppet-code-deploy-install-mac
created: 2025-01-17
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - puppet
related_docs:
---

### Instructions for Code Deployment (macOS)

#### Assumptions:

- The macOS username matches `<your HawkID>`.
- Permissions to deploy an environment are associated with `<your HawkID>`.
- The home directory is correctly set via the `$HOME` environment variable.
- The Puppet Enterprise (PE) server is named `<your PE server>`.
- The version of PE is `2021.x.xx` (visible in the console sidebar at the bottom).
- Client tools configuration is **per-user** and not system-wide.

---

### 1. **Install Client Tools**

1. Go to [Puppet Enterprise Client Tools](https://puppet.com/try-puppet/puppet-enterprise-client-tools/).
2. Navigate to the version `2021.x.x` and download the **Client Tools** for macOS.
3. Install the downloaded package by following the installation prompts.

---

### 2. **Create Configuration Files (One-Time Setup)**

Run the following commands in a **terminal**:

```bash
HOMEDIR=$HOME
PE_SERVER=<your PE server>

# Create necessary directories
mkdir -p ~/.puppetlabs/client-tools/
mkdir -p ~/.puppetlabs/client-tools/ssl

# Download the certificate from the PE server
curl -s -k -o ~/.puppetlabs/client-tools/ssl/ca.pem https://${PE_SERVER}:8140/puppet-ca/v1/certificate/ca

# Configure puppet-access
cat >~/.puppetlabs/client-tools/puppet-access.conf <<EOF
{
    "service-url": "https://${PE_SERVER}:4433/rbac-api",
    "token-file": "${HOMEDIR}/.puppetlabs/token",
    "certificate-file": "${HOMEDIR}/.puppetlabs/client-tools/ssl/ca.pem"
}
EOF

# Configure puppet-code (ensuring ~ is expanded correctly)
cat >~/.puppetlabs/client-tools/puppet-code.conf <<EOF
{
    "cacert": "${HOMEDIR}/.puppetlabs/client-tools/ssl/ca.pem",
    "token-file": "${HOMEDIR}/.puppetlabs/token",
    "service-url": "https://${PE_SERVER}:8170/code-manager"
}
EOF

# Add puppet to your path in the .zshrc
export PATH="/opt/puppetlabs/bin:$PATH"


```

---

### 3. **Test Deployment Connectivity**

1. Login to Puppet using the `puppet-access` tool:
    
```bash
/opt/puppetlabs/bin/puppet-access login --lifetime 2h
```
    
    Enter your credentials when prompted:
    
    - **Username**: `<your HawkID>`
    - **Password**: `<your HawkID password>`
    - Access token will be saved to `$HOME/.puppetlabs/token`.
2. Test connectivity and permissions with a dry run:
    
```bash
/opt/puppetlabs/bin/puppet-code deploy --dry-run
```
    
    **Expected Output:**
    
    - Authorization warnings for non-admin users (e.g., `You are not authorized to deploy Puppet environment 'production'`) are normal.
    - This test ensures you can connect appropriately.

---

### 4. **Deploy an Environment**

1. Re-login to Puppet if the token lifetime has expired:
    
```bash
/opt/puppetlabs/bin/puppet-access login --lifetime 2h
```
    
2. Deploy a specific environment:

```bash
/opt/puppetlabs/bin/puppet-code deploy <my environment> --wait
```
    
    **Example Output:**
    
```json
Found 1 environments.
[
  {
	"deploy-signature": "fa12bd02bb6764eceddd74c486a2ea4c3d2e6a65",
	"environment": "<my environment>",
	"file-sync": {
	  "code-commit": "3bbd7fd456e0a0ea029fbfcb2e565c5f58476899",
	  "environment-commit": "741077e5d44d909e9d3de5dffc4defca22e8e730"
	},
	"id": 72,
	"status": "complete"
  }
]
```
    

---

### Notes:

- Replace `<your HawkID>`, `<your PE server>`, and `<my environment>` with your actual values.
- Ensure all dependencies (e.g., curl) are installed on your macOS system.
- Use the **dry-run** step to validate connectivity before making actual deployments.







pdk 3


pdk validate