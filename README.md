# Migration from Azure DevOps to GitHub Actions

## Goals
- Migrate CI/CD from Azure DevOps to GitHub Actions
- Use Azure Portal for hosting, deploy via GitHub Actions
- Centralize code, issues, and CI/CD in GitHub
- Integrate Jira for issue tracking
- Monitor builds/deployments with Datadog
- Enforce code quality with branch protection and PR checks

## Steps
1. Set up GitHub repo and push code
2. Add GitHub Actions workflow for CI/CD
3. Configure environments and secrets
4. (For real projects) Add SSH key for Azure deployment
5. Integrate Jira and Datadog (see below)
6. Set branch protection rules in GitHub

## Jira Integration
- Use [GitHub-Jira integration](https://support.atlassian.com/jira-software-cloud/docs/integrate-with-github/) for traceability

## Datadog Integration
- Use [Datadog GitHub Action](https://github.com/DataDog/datadog-actions) to monitor builds/deployments

## Branch Protection
- Go to Settings → Branches → Add rule for `main` (require PR review, status checks, etc.) 