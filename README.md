# AccuKnox Secret Scan GitHub Action

This GitHub Action scans your repository for secrets and uploads the result to your tenet in AccuKnox SaaS.

## Learn More

- [About Accuknox](https://www.accuknox.com/)

---

## Inputs

| **Input Name**  | **Description** | **Optional/Required** | **Default Value** |
|-----------------|-----------------|-----------------------|-------------------|
| `token` | The token for authenticating with the CSPM panel. | Required | `None` |
| `tenant_id` | The ID of the tenant. | Required | `None` |
| `label` | The label created in AccuKnox SaaS. | Required | `None` |
| `endpoint` | The URL of the CSPM panel to push the scan results to. | Required | `cspm.demo.accuknox.com` |
| `secret_scan_type` | Source type for scanning (`git`, `huggingface`, `s3`). | Required | `git` |
| `branch` | Branch to scan. Use name of the branch or `all-branches`. | Optional | `HEAD branch` |
| `exclude-paths` | Paths to exclude from the scan. | Optional | `None` |
| `args` | Additional arguments to pass to the CLI. | Optional | `None` |
| `dataset` | Dataset name (required if `secret_scan_type` is `huggingface`). | Optional | `None` |
| `huggingface_token` | Hugging Face token (required if `secret_scan_type` is `huggingface`). | Optional | `None` |
| `bucket_name` | S3 bucket name (required if `secret_scan_type` is `s3`). | Optional | `None` |
| `aws_access_key_id` | AWS Access Key ID (required if `secret_scan_type` is `s3`). | Optional | `None` |
| `aws_secret_access_key` | AWS Secret Access Key (required if `secret_scan_type` is `s3`). | Optional | `None` |
| `use_extended_ruleset` | Enable extended regex rules for detecting sensitive data. | Optional | `false` |
| `results` | Specifies which type(s) of results to output: `verified`, `unknown`, `unverified`, `filtered_unverified`. Defaults to all types. | Optional | `all` |
| `fail` | Fail the pipeline if secrets are found. | Optional | `false` |
| `upload_artifact` | Upload scan results as artifact	 | Optional | `true` |

---

### List of available flags

```
All available Flags:
      --log-level=0         Logging verbosity on a scale of 0 (info) to 5 (trace). Can be disabled with "-1".
      --concurrency=20           Number of concurrent workers.
      --no-verification     Don't verify the results.
      --allow-verification-overlap
                                 Allow verification of similar credentials across detectors
      --filter-unverified   Only output first unverified result per chunk per detector if there are more than one results.
      --verifier=VERIFIER ...    Set custom verification endpoints.
      --custom-verifiers-only   Only use custom verification endpoints.
      --archive-max-size=ARCHIVE-MAX-SIZE
                                 Maximum size of archive to scan. (Byte units eg. 512B, 2KB, 4MB)
      --archive-max-depth=ARCHIVE-MAX-DEPTH
                                 Maximum depth of archive to scan.
      --archive-timeout=ARCHIVE-TIMEOUT
                                 Maximum time to spend extracting an archive.
      --include-detectors="all"  Comma separated list of detector types to include. Protobuf name or IDs may be used, as well as ranges.
      --exclude-detectors=EXCLUDE-DETECTORS
                                 Comma separated list of detector types to exclude. Protobuf name or IDs may be used, as well as ranges. IDs defined here take precedence over the include list.
  -i, --include-paths=INCLUDE-PATHS
                                 Path to file with newline separated regexes for files to include in scan.
      --exclude-globs=EXCLUDE-GLOBS
                                 Comma separated list of globs to exclude in scan. This option filters at the `git log` level, resulting in faster scans.
```

---

## Minimalist Sample Configuration

```yaml
name: AccuKnox Secret Scan
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  schedule:
    - cron: "0 0 * * *"

jobs:
  accuknox-secret-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Required to get the full commit history.

      - name: AccuKnox Secret Scan
        uses: accuknox/secret-scan-action@v1.0.0
        with:
          token: ${{ secrets.ACCUKNOX_TOKEN }}
          tenant_id: ${{ secrets.ACCUKNOX_TENANT_ID }}
          label: ${{ secrets.ACCUKNOX_LABEL }}
          endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          fail: true
```

# License

This action is released under the [Apache License](https://github.com/accuknox/secret-scan-action/blob/main/LICENSE).
