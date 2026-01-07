# Azure DevOps YAML Pipeline Artefacts

This folder contains `*.buildDefinition.json` artefacts for creating Azure DevOps YAML pipelines.

## File Naming Patterns

### Inclusion

In order to be considered by the ECP deployment, the file pattern must be `*.buildDefinition.json`.

### Exclusion

To override adding a library file from being added, the module's individual configuration may contain a folder `yaml-pipeline` with a file following the pattern `*.buildDefinition.exclude.json`.

### Disabling

To disable files in this library completely, it is suggested to follow the name pattern `*.buildDefinition.disabled.json`. This should happen with examples.

## JSON Format

The JSON format is based on the REST schema of [Azure DevOps Build Definitions API](https://learn.microsoft.com/en-us/rest/api/azure/devops/build/definitions/create?view=azure-devops-rest-7.1) with only the following, mandatory properties added:

- `artefactName` for the artefactName
- `nameElement` becoming `displayName`

## Supported Properties

Artefact deployment will use the `azuredevops_build_definition` terraform resource and honor only the following properties:

- **name** (nameElement)
- **path** (defaults to `\`)
- **queue**
  - **name** (defaults to `Azure Pipelines`)
- **skipFirstRun** (defaults to `true`)
- **queueStatus** (defaults to `enabled`)
- **jobAuthorizationScope** (defaults to `projectCollection`)
- **repository**
  - **type** (defaults to `TfsGit`)
  - **defaultBranch** (defaults to `refs/heads/main`)
- **process**
  - **yamlFilename**
- **repository**
  - **properties:**
    - **reportBuildStatus** (defaults to `true`)
- **variableGroups** (defaults to `[]`)
- **variables** (defaults to `{}` - key becomes name)
  - **value**
  - **isSecret**
  - **allowOverride**

Properties or objects not mentioned above are ignored.

## Retrieving Existing Pipeline Configurations

Output of already configured YAML pipelines (e.g., for adding additional artefacts) can be retrieved by:

```bash
az rest --method GET --url "https://dev.azure.com/<DevOpsORG>/<DevOpsProject>/_apis/build/definitions?api-version=7.1" --resource 499b84ac-1321-427f-aa17-267ca6975798
```

## Terraform Variable Replacement

The deployment supports replacement of the following patterns with their terraform variables throughout the entire JSON file:

- `<TERRAFORM_VARIABLE:ecp_environment_name>`
- `<TERRAFORM_VARIABLE:ecp_azure_devops_project_name>`
- `<TERRAFORM_VARIABLE:ecp_azure_devops_repository_name>`
- `<TERRAFORM_VARIABLE:ecp_azure_devops_pool_name>`

## Best Practices for Successful Implementation

- Do not explicitly set the agent queue name. Let the referenced YAML file do this, preferably via variable groups.
- Variables and variable groups can also be added in the referenced YAML file.
- Default value of `skipFirstRun` prevents pipeline from running upon creation.
