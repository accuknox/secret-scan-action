# 🔑 Automate Secret Scanning with AccuKnox GitHub Action  

The **AccuKnox Secret Scan GitHub Action** detects **hardcoded secrets, credentials, API keys, and other sensitive information** within Git repositories.  
It integrates seamlessly with the **AccuKnox Console**, providing centralized visibility, risk tracking, and remediation across your development lifecycle.  

Catch secrets before they leak — prevent breaches and protect your infrastructure with **shift-left security**.  

---

## 🎯 Key Features  
✅ **Hardcoded Secret Detection** – Identify API keys, passwords, tokens, and other secrets in your code.  
🔒 **Shift Left Security** – Integrate secret scanning directly into CI/CD pipelines.  
📥 **Seamless Console Integration** – Upload findings to the AccuKnox dashboard for visibility and remediation tracking.  
⚙️ **Flexible Configuration** – Supports branch selection, path exclusions, and custom scan arguments.  
🚦 **Fail Builds on Violations** – Configure pipelines to fail on detected secrets or continue in soft-fail mode.  

---

## ⚠️ Prerequisites  
Before using this GitHub Action, ensure:  

- 🔐 **AccuKnox Console Access** – Sign in to your AccuKnox tenant.  
- 🗝️ **API Token** – Retrieve from AccuKnox Console (see Token Generation).  
- 🏷️ **Label in Console** – Create a label to tag scan reports.  
- 🔑 **GitHub Secrets Configured** – Store API token, endpoint, and label securely.  

---

## 📌 Installation & Usage  

### Step 1: Retrieve AccuKnox Credentials  
1. Log in to AccuKnox Console.  
2. Navigate to **Settings → Tokens → Create Token**.  
   - Save the token as `Accuknox_token`.  
3. Create a **label** under **Dashboard → Labels** for scan results.  

---

### Step 2: Configure GitHub Secrets  
In your repository → **Settings → Secrets and variables → Actions → New repository secret**  

| Secret Name        | Description |
|---------------------|-------------|
| `ACCUKNOX_TOKEN`   | AccuKnox API token |
| `ACCUKNOX_ENDPOINT`| AccuKnox API endpoint (e.g., `cspm.demo.accuknox.com`) |
| `ACCUKNOX_LABEL`   | Label for grouping results in AccuKnox Console |  

---

### Step 3: Define Your GitHub Workflow  

Create `.github/workflows/secret-scan.yml`:

```yaml
name: AccuKnox Secret Scan Workflow

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  secret-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Secret Scan
        uses: accuknox/secret-scan-action@v0.0.1
        with:
          branch: "main"                             # Branch to scan
          results: ""                                # Types of results: verified, unknown, unverified, filtered_unverified
          exclude_paths: "tests/,docs/"             # Paths to exclude
          additional_arguments: ""                   # Extra arguments for the scanner
          base_command: ""                           # Override default Docker command
          output_format: json                        # Output format
          output_file_path: "./secret_results.json" # Output file path
          soft_fail: true                            # Continue even if secrets found
```
          token: ${{ secrets.ACCUKNOX_TOKEN }}
          endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          label: ${{ secrets.ACCUKNOX_LABEL }}
