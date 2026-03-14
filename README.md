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
        uses: accuknox/secret-scan-action@latest
        with:
          branch: "main"                              # Branch to scan
          results: ""                                 # Types of results: verified, unknown, unverified, filtered_unverified
          exclude_paths: "tests/,docs/"               # Paths to exclude
          additional_arguments: ""                    # Extra arguments for the scanner
          base_command: ""                            # Override default Docker command
          upload_results: true                        # Upload results to GitHub artifact
          accuknox_token: ${{ secrets.ACCUKNOX_TOKEN }}
          accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          accuknox_label: ${{ secrets.ACCUKNOX_LABEL }}
          soft_fail: true
```

## ⚙️ Configuration Options (Inputs)

| Input                  | Description                                                   | Optional/Required | Default |
|------------------------|---------------------------------------------------------------|------------------|---------|
| `branch`               | Git branch to scan. Use `all-branches` to scan all branches   | Optional         | Latest commit SHA |
| `results`              | Result types to include: `verified`, `unknown`, `unverified`, `filtered_unverified` | Optional | All types included |
| `exclude_paths`        | Comma-separated list of paths to exclude from scanning        | Optional         | "" |
| `additional_arguments` | Extra arguments to pass to the secret scanning tool           | Optional         | "" |
| `base_command`         | Override the default command (Docker/local CLI)               | Optional         | Docker-based |
| `accuknox_token`                | API token for AccuKnox SaaS                                    | Required         | — |
| `accuknox_endpoint`             | AccuKnox API endpoint                                          | Optional         | cspm.demo.accuknox.com |
| `accuknox_label`                | Label used in AccuKnox SaaS to organize results               | Required         | — |
| `soft_fail`            | Prevent CI from failing on secret detection                   | Optional         | false |
| `upload_results`       | Upload scan results as GitHub artifact                        | Optional         | true |

---

## 🔍 How It Works

1. **Developer pushes code** → Workflow triggers.  
2. **Secret Scanner runs** → Docker executes the scan or local CLI if overridden.  
3. **Secrets detected** → The tool generates a JSON results file.  
4. **Artifact upload** → If `upload_results: true`, the results file is uploaded as a GitHub artifact.  
5. **Upload findings** → Results are sent to AccuKnox using the provided token & label.  
6. **Review findings** → Dashboard → Issues → Findings → Filter by *Secret Findings*.  
7. **Pipeline decision** → If `soft_fail: false`, the pipeline fails on detected secrets.  

---

## 🛠️ Troubleshooting & Best Practices

| Issue                                | Cause                                     | Solution |
|--------------------------------------|------------------------------------------|----------|
| `Missing required input: token`       | GitHub secret not set                     | Add `ACCUKNOX_TOKEN` |
| `Failed to connect to endpoint`      | Incorrect API URL or network issue       | Verify endpoint URL |
| No scan results in AccuKnox Console  | Missing label or invalid credentials      | Verify label and token values |
| Workflow fails even with minor secrets | `soft_fail` not set                        | Set `soft_fail: true` to continue despite findings |
| Empty scan report                     | Wrong directory or branch scanned        | Verify `exclude_paths` and `branch` inputs |

---

## 📖 Support & Documentation

📚 Docs: [AccuKnox Documentation](https://accuknox.com)  

📧 Support: support@accuknox.com  

---

## 🏁 Conclusion

The **AccuKnox Secret Scan GitHub Action** ensures hardcoded secrets are detected early, preventing leaks and enforcing security best practices.  

🔐 **Shift Left with AccuKnox – Catch Secrets Before They Leak!** 🚀
