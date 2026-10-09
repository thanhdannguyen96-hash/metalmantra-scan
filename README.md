# Metal Mantra scan

Reusable GitHub Actions workflow for [Metal Mantra](https://metalmantra.io). It runs Metal Mantra's static agent scanner on
the calling repository's own runner and reports only rule ids and counts. Your source never leaves the runner.

Because GitHub signs the run's identity token with the path of this workflow, Metal Mantra can tell that these exact steps
ran, so the scanner and the checkout were not altered by the repository owner. Reports from this workflow are labelled
"CI-attested with Metal Mantra's pinned scanner".

## Use

Register your private repository at <https://metalmantra.io/scan/private>. It gives you a caller workflow like this:

```yaml
name: Metal Mantra agent signal
on:
  push:
    branches: [main]
  schedule:
    - cron: "0 6 * * 1"
  workflow_dispatch:
permissions:
  contents: read
  id-token: write
jobs:
  signal:
    uses: thanhdannguyen96-hash/metalmantra-scan/.github/workflows/scan.yml@v1
    with:
      registration: <your registration id>
```

Pull request runs cannot publish a report. The scan is an automated static signal, never a security certification.
