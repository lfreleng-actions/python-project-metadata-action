<!--
# SPDX-License-Identifier: Apache-2.0
# SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🛠️ Python Project Metadata

> [!WARNING]
> Deprecated: this action will receive no further changes or
> releases. We will archive this repository; workflows that reference
> its existing tags or commits keep working after that.
> `lfreleng-actions/build-metadata-action` replaces it: see
> [Migrating to build-metadata-action](#migrating-to-build-metadata-action).

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
| python_project_version | The version of the Python project                        |
| python_package_name    | The name of the Python package                           |
| project_match_package  | Set true when the project and package name match         |
| project_match_repo     | Set true when the project name and repository name match |
| build_python_version   | Most recent Python version supported by project          |
| matrix_json            | All Python versions supported by project as JSON string  |

<!-- markdownlint-enable MD013 -->

Python package names can be different from project names.

Note: dashes in the project name typically get replaced with underscores

## Migrating to build-metadata-action

Replace the step from the usage example above with a call to
`lfreleng-actions/build-metadata-action`. Pin it to the commit SHA of
the release you choose. Set `path_prefix` when the project does not
sit at the repository root.

<!-- markdownlint-disable MD013 -->

```yaml
  - name: 'Get Python project metadata'
    uses: lfreleng-actions/build-metadata-action@0000000000000000000000000000000000000000  # REPLACE: vX.Y.Z
    with:
      project_type: python
      # Needed when the project is not at the repository root
      path_prefix: 'path/to/project'
```

<!-- markdownlint-enable MD013 -->

### Output mapping

<!-- markdownlint-disable MD013 -->

| This action            | build-metadata-action          |
| ---------------------- | ------------------------------ |
| python_project_file    | python_metadata_source         |
| python_project_name    | project_name                   |
| versioning_type        | python_versioning_type         |
| python_project_version | project_version                |
| python_package_name    | python_package_name            |
| project_match_package  | python_project_match_package   |
| project_match_repo     | project_match_repo             |
| matrix_json            | python_matrix_json             |
| build_python_version   | python_build_version           |

<!-- markdownlint-enable MD013 -->

`matrix_json` and `python_matrix_json` share the same shape:
`{"python-version": [...]}`.

### Differences

1. **Dynamic versions.** For a dynamically versioned project, this
   action leaves `python_project_version` empty.
   `build-metadata-action` reports a version it resolves from
   `version.properties` or the latest git tag, unless you set its
   `use_version_extract` input to `false`. Use
   `python_versioning_type` to tell the two cases apart. Releases up
   to v0.13.4 can report a wrong value in `project_version`
   (lfreleng-actions/build-metadata-action#228).
2. **Package name.** This action replaces dashes in the project name
   with underscores. The `python_package_name` output of
   `build-metadata-action` reports the name as the project declares
   it. Its `python_project_match_package` output makes the same
   replacement before comparing, and agrees with
   `project_match_package`.
3. **No project name.** This action fails when the directory declares
   no project name. `build-metadata-action` succeeds and leaves
   `project_name` empty; a caller that needs a name must check for one.
