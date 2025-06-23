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

## Release Versioning and Tracking

To track and display release versions for this project:

1. **VERSION File**
   - The current version is stored in a file named `VERSION` in the project root (e.g., `0.1.0`).
   - Update this file before each release.

2. **Tagging Releases**
   - After updating the `VERSION` file, create a git tag that matches the version (e.g., `v0.2.0`).
   - Example commands:
     ```sh
     echo 0.2.0 > VERSION
     git add VERSION
     git commit -m "Bump version to 0.2.0"
     git tag v0.2.0
     git push origin main --tags
     ```

3. **GitHub Actions Integration**
   - The CI/CD workflow will display the version in the workflow logs.
   - When you push a tag like `v0.2.0`, the workflow will automatically create a GitHub Release with the version and release notes.

4. **Viewing Releases**
   - Go to the **Releases** tab in your GitHub repository to see all tagged releases and their version numbers.

**Summary:**
- Always update the `VERSION` file before a release.
- Tag your release with the version (e.g., `v0.2.0`).
- The workflow will handle the rest and create a visible release in GitHub.

## Where is the VERSION file and how do I edit it?

- The `VERSION` file is located in the **root folder** of this project (`sample-python-api/VERSION`).
- It contains the current version number (e.g., `0.1.0`).
- To update the version, open the `VERSION` file in any text editor (VSCode, Notepad, etc.), change the version number, and save the file.
- You can also update it from the command line:
  ```sh
  echo 0.2.0 > VERSION
  ```
- After editing, commit the change:
  ```sh
  git add VERSION
  git commit -m "Bump version to 0.2.0"
  ```
- Then follow the release process as described above.

## Versioning and Release Tracking Across Multiple Environments

For projects with multiple environments (e.g., dev, uat, prod1, prod2), it's important to track which version is deployed to each environment and to document the promotion of code through these stages.

### **Recommended Approach**

1. **Consistent Versioning**
   - Use the `VERSION` file to define the current version of your codebase.
   - Update the version number whenever you make a release candidate for promotion to the next environment.

2. **Tagging Releases for Each Environment**
   - Use git tags to mark releases for each environment. For example:
     - `v0.2.0-dev` for a release to dev
     - `v0.2.0-uat` for a release to uat
     - `v0.2.0-prod1` for a release to prod1
     - `v0.2.0-prod2` for a release to prod2
   - This makes it easy to see which version is in each environment.

3. **Promotion Workflow**
   - When promoting code from one environment to the next:
     1. Update the `VERSION` file if needed.
     2. Commit any changes.
     3. Tag the commit for the target environment:
        ```sh
        git tag v0.2.0-uat
        git push origin v0.2.0-uat
        ```
     4. Deploy the tagged version to the corresponding environment.

4. **Documenting Releases**
   - Use the **Releases** tab in GitHub to add notes about what's included in each environment's release.
   - Optionally, maintain a `CHANGELOG.md` to summarize changes for each version and environment.

5. **Example Tagging Flow**
   - Release to dev:
     ```sh
     echo 0.2.0 > VERSION
     git add VERSION
     git commit -m "Prepare 0.2.0 for dev"
     git tag v0.2.0-dev
     git push origin main --tags
     ```
   - Promote to uat:
     ```sh
     git tag v0.2.0-uat
     git push origin v0.2.0-uat
     ```
   - Promote to prod1:
     ```sh
     git tag v0.2.0-prod1
     git push origin v0.2.0-prod1
     ```
   - Promote to prod2:
     ```sh
     git tag v0.2.0-prod2
     git push origin v0.2.0-prod2
     ```

6. **Tracking in GitHub**
   - Each tag will appear in the **Releases** tab, so you can see which version is in each environment.
   - You can filter tags/releases by environment suffix (e.g., `-uat`, `-prod1`).

### **Best Practices**
- Always update the `VERSION` file before tagging a new release.
- Use clear, consistent tag names for each environment.
- Document changes and environment promotions in release notes or a changelog.
- Automate deployment from tags using GitHub Actions for each environment if possible.

**This approach ensures you always know what code is running in each environment and can easily roll back or audit releases.**

## Automating Versioning and Release Tagging (Production-Grade)

For production-grade applications, you can use manual version management and release tagging, or explore advanced tools as your workflow matures.

### **Manual Versioning (Recommended for Simplicity)**
- Update the `VERSION` file manually before each release.
- Tag releases for each environment as described above (e.g., `v0.2.0-dev`, `v0.2.0-uat`).
- Document changes in a `CHANGELOG.md` or in GitHub Releases.

### **Advanced Automation (Optional)**
- Tools like `bump2version` or `python-semantic-release` can automate versioning, but require additional setup and careful configuration.
- If you want to explore these, see their documentation:
  - [bump2version](https://github.com/c4urself/bump2version)
  - [python-semantic-release](https://python-semantic-release.readthedocs.io/en/latest/)
- For most teams, manual versioning with clear tagging and documentation is sufficient and reliable.

### **Summary**
- Update the `VERSION` file and create tags manually for each release.
- Use GitHub Releases and changelogs for tracking.
- Consider automation only if your workflow requires it and your team is comfortable with the tools. 