<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🛠️ Python Project Metadata

Extracts Python project metadata from a repository.

## python-project-metadata-action

## Usage Example

<!-- markdownlint-disable MD013 -->

```yaml
  - name: 'Get Python project metadata'
    uses: lfreleng-actions/python-project-metadata-action@main
```

<!-- markdownlint-enable MD013 -->

##  Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name       | Required | Description                           |
| ------------------- | -------- | ------------------------------------- |
| path_prefix         | False    | Path/directory to Python project code |

## Outputs

<!-- markdownlint-disable MD013 -->

| Variable Name          | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| python_project_file    | File used to define/describe the project                 |
| python_project_name    | The name of the Python project                           |
| versioning_type        | Can be either static or dynamic (determined by tags)     |
| python_project_version | The name of the Python project                           |
| python_package_name    | The name of the Python package                           |
| project_match_package  | Set true when the project and package name match         |
| project_match_repo     | Set true when the project name and repository name match |
| build_python           | Most recent Python version supported by project          |
| matrix_json            | All Python versions supported by project as JSON string  |

<!-- markdownlint-enable MD013 -->

Python package names can be different from project names.

Note: dashes in the project name typically get replaced with underscores
