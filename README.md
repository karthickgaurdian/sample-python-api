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

## Test File Structure and Best Practices

For medium or enterprise projects, organize your tests for scalability and maintainability:

```
sample-python-api/
│
├── app.py
├── requirements.txt
└── tests/
    ├── __init__.py
    ├── test_api.py
    ├── test_utils.py
    └── submodule/
        ├── __init__.py
        └── test_subfeature.py
```

- Place all test files in a `tests/` directory.
- Name test files starting with `test_` (e.g., `test_api.py`).
- Use subfolders for different modules or features.
- Include `__init__.py` files in test folders for package structure.
- Separate unit, integration, and end-to-end tests (e.g., `tests/unit/`, `tests/integration/`).
- Use clear, descriptive names for test files and functions.
- `pytest` will automatically discover and run all tests in files matching `test_*.py` or `*_test.py`.

**To run all tests:**
```sh
pytest
```

**To run tests in a specific file or folder:**
```sh
pytest tests/test_api.py
pytest tests/submodule/
```

For more, see the [pytest documentation](https://docs.pytest.org/en/stable/). 