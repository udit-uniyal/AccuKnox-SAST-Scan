# AccuKnox SAST Scan GitHub Action

## Learn More

- [About Accuknox](https://www.accuknox.com/)

**Description**  
This GitHub Action runs a Static Application Security Testing (SAST) scan using SonarQube, then uploads the generated report to the AccuKnox CSPM panel. The action can be configured with specific inputs to integrate seamlessly with your DevSecOps pipeline.

## Usage

### Steps for Using AccuKnox SAST Scan Action in a Workflow YAML File

1. **Checkout into the Repo**  
   Use the checkout action to ensure your codebase is available for scanning.
   
2. **Add AccuKnox SAST Scan Action**  
   Use the `accuknox/sast-scan-action` repository with the desired version tag, e.g., `v1.0.0`.

3. **Token Generation from AccuKnox SaaS and Viewing Tenant ID**  
   To obtain the `accuknox_token` and `tenant_id` values needed to authenticate with AccuKnox:
   
   - **Navigate to Tokens**  
     Go to the **Settings** section in the AccuKnox SaaS sidebar.
   
   - **Create Token**  
     In the "Tokens" section, click on **Create Token**. This action will display your `tenant_id` and allow you to generate an access token.
   
   - **Generate the Token**  
     After clicking **Generate**, copy the `accuknox_token` to use in the workflow.

### Example Workflow File

```yaml
name: AccuKnox SAST Scan Workflow
on:
  push:
    branches:
      - main

jobs:
  sast-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run AccuKnox SAST Scan
        uses: accuknox/sast-scan-action@v1.0.0
        with:
          sonar_token: ${{ secrets.SONAR_TOKEN }}
          sonar_host_url: ${{ secrets.SONAR_HOST_URL }}
          accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          tenant_id: ${{ secrets.TENANT_ID }}
          accuknox_token: ${{ secrets.ACCUKNOX_TOKEN }}
          label: "my-sast-scan"
          sonar_project_key: "my-project-key"
```

## Input Values

| Input Value        | Description                                                | Optional/Required | Default Value |
|--------------------|------------------------------------------------------------|--------------------|---------------|
| `sonar_token`      | Personal access token for authenticating with SonarQube.   | Required          | None          |
| `sonar_host_url`   | URL of the SonarQube server to run the SAST scan.          | Required          | None          |
| `accuknox_endpoint`| AccuKnox API endpoint URL to upload the scan results.      | Required          | None          |
| `tenant_id`        | Unique ID of the tenant for AccuKnox CSPM panel.           | Required          | None          |
| `accuknox_token`   | Token for authenticating with AccuKnox API.                | Required          | None          |
| `label`            | Label in AccuKnox SaaS for tagging scan results.           | Required          | None          |
| `sonar_project_key`| Project key in SonarQube for identifying the project.      | Optional          | None          |

## How it Works

- **SonarQube SAST Scan**: The action runs a SAST scan on the specified project in SonarQube, using the provided credentials and project key.
- **AccuKnox Report Generation**: The action uses AccuKnox's Docker container to generate a SAST report.
- **Report Upload**: The generated report is uploaded to the AccuKnox CSPM panel for centralized monitoring and insights.
- **Quality Gate Check**: Verifies if the project meets the set quality standards on SonarQube.

## Notes

- Ensure all necessary secrets (`SONAR_TOKEN`, `SONAR_HOST_URL`, `ACCUKNOX_ENDPOINT`, `TENANT_ID`, and `ACCUKNOX_TOKEN`) are securely stored in your repository's settings.
- AccuKnox panel provides a centralized view of all SAST results, enabling detailed security monitoring and analytics.
  
