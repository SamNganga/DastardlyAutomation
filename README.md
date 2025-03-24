# Dastardly Automation with GitHub Workflows

This repository demonstrates how to automate Dastardly scans using **GitHub Actions**. The workflow is designed to run security scans against a target URL periodically and on-demand, publishing test reports for further analysis.

## 📌 Features
- Automated Dastardly scans triggered by:
  - Push events to the `main` branch.
  - Pull requests to the `main` branch.
  - Scheduled runs every **Friday at 11 PM (UTC)**.
  - Manual trigger via **workflow_dispatch**.
- JUnit-style report generation and publishing using `mikepenz/action-junit-report`.

## 🚀 Getting Started
### Prerequisites
- A GitHub repository where you want to set up the Dastardly automation.
- Basic understanding of GitHub Actions.

### Setup Instructions
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   ```
2. Navigate to the repository folder:
   ```bash
   cd your-repo
   ```
3. Create a `.github/workflows/dastardly-scan.yml` file with the following content:

```yaml
name: Dastardly CI Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 23 * * 5'  # Every Friday at 11 PM (UTC)
  workflow_dispatch:

jobs:
  dastardly_scan:
    runs-on: ubuntu-latest

    steps:
      - name: Wait for 5 minutes (Optional)
        run: sleep 300  # Delay to allow target to stabilize

      - name: Run Dastardly Action Step
        continue-on-error: true
        uses: PortSwigger/dastardly-github-action@main
        with:
          target-url: 'https://customer.uat.irembopay.com/'

      - name: Publish Test Report
        if: always()
        uses: mikepenz/action-junit-report@v3
        with:
          report_paths: '**/dastardly-report.xml'
          require_tests: true
```

## 📂 Directory Structure
```
.
├── .github
│   └── workflows
│       └── dastardly-scan.yml
└── README.md
```

## 📖 Usage
- The workflow will automatically run on:
  - Pushes to the `main` branch.
  - Pull requests to the `main` branch.
  - Every Friday at 11 PM.
- You can also trigger the workflow manually from the **GitHub Actions tab**.

## 📈 Viewing Reports
Reports are generated in the JUnit XML format and can be accessed via the **Actions tab** in your repository. Optionally, you can set up artifact storage to persist reports.

## 📝 Contributing
Feel free to submit pull requests to improve this automation. Issues and feature requests are welcome!

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

