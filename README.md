# Azure DevOps Templates

Collection of templates to use in Azure DevOps.

## Usage

Currently Azure DevOps does not support referencing templates from public GitHub repositories without
specifiying a valid service connection. This would be too cumborsome to deal with for multiple organizations.
Instead this repo is best used by creating a mirror of it, and using a pipeline to synchronize the content from upstream.

### Importing the repository to Azure DevOps

Begin by [importing the repository](https://docs.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops) into a global project in your organization.

- Azure DevOps > Project > Repos > Import Repository
  - Repository type: Git
  - Clone URL: https://github.com/XenitAB/azure-devops-templates.git
  - [ ] Requires Authentication
  - Name: azure-devops-templates
- Press Import

### Configuring Build Service permissions

Configure Build Service to have permission to push changes to the template repository.

- Azure DevOps > Project > Project settings > Repositories > azure-devops-templates > Permissions
- [Project Name] Build Service ([org name]) > Contribute: `Allow`

### Adding Azure Pipelines for repository syncronization

Then create a pipeline from the definition located in `./ci/pipeline.yaml`, this pipeline will sync with Github.
Any time you want to get the latest changes you need to run this pipeline.

- Azure DevOps > Project > Repos > Set up build
- Choose Azisting Azure Pipelines YAML file
  - Branch: master
  - Path: /.ci/pipeline.yaml
- Press Continue > Press Run

### Adding reference to the synchronized repository

You should be able to use the templates when the mirroring is complete by referencing the git repository.

```yaml
resources:
  repositories:
    - repository: templates
      type: git
      name: <project>/azure-devops-templates
      ref: refs/tags/<version>

stages:
  - template: gitops/deploy/pipeline.yaml@templates
```

# Versioning

Versions follow the [CalVer](https://calver.org/) standard. This simplifies detecting usage of very old versions.
The versions should use the following pattern `YYYY.0M.MICRO`. The micro value starts at 0 and increments by one
for each release during the same month.

# License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
