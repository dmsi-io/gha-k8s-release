# GHA Kubernetes Release

[![actions-workflow-main][actions-workflow-main-badge]][actions-workflow-main]
[![release][release-badge]][release]

This GitHub Action is a composite action that handles the pulling and population of Kubernetes configuration files into a `release/*` branch. This action will also tag the new commit with a provided `semver_tag`.

## Inputs

| NAME                 | DESCRIPTION                                                                                       | TYPE     | REQUIRED  | DEFAULT                     |
| -------------------- | ------------------------------------------------------------------------------------------------- | -------- | --------- | --------------------------- |
| `GITHUB_TOKEN`       | GitHub Action Token or PAT.                                                                       | `string` | `true`    | `''`                        |
| `semver_tag`         | Semver Override Tag. This value will always be used if provided.                                  | `string` | `false`   | `null`                      |
| `branch`             | Branch override value.                                                                            | `string` | `false`   |                             |
| `force_tag`          | Flag to force tag.                                                                                | `bool`   | `false`   | `true`                      |
| `skip_tag`           | Flag to skip tagging.                                                                             | `bool`   | `false`   | `false`                     |
| `new_branch`         | Name of new branch to commit changes to. (used more so to test this repo)                         | `string` | `false`   |                             |
| `author_name`        | The author name to use for the back merge.                                                        | `string` | `false`   | `github-actions`            |
| `author_email`       | The email to use for the back merge.                                                              | `string` | `false`   | `github-actions@github.com` |
| `k8s_directory`      | Location of k8s config files.                                                                     | `string` | `false`   | `k8s`                       |
| `k8s_config_repo`    | Name of repo that may contain additional default Kubernetes config files.                         | `string` | `false`   |                             |
| `k8s_repo_directory` | Location of repo k8s config files.                                                                | `string` | `false`   | `k8s`                       |
| `GCP_PROJECT_ID`     | ProjectID of the GCP project to deploy to. (only required to populate Docker image tag correctly) | `string` | `false`\* |                             |
| `REGISTRY_HOSTNAME`  | Hostname of Container Registry.                                                                   | `string` | `false`   | `gcr.io`                    |
| `SERVICE_NAME`       | Allows to override the desired SERVICE_NAME variable.                                             | `string` | `false`   | `github.repository`         |

## Example

```yaml
name: Kubernetes Release

on:
  push:
    branches:
      - release/*

jobs:
  test-gha-k8s-release:
    runs-on: ubuntu-latest
    name: Release Kubernetes
    steps:
      - name: Release
        id: release
        uses: dmsi-io/gha-k8s-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.PUBLIC_GHA_ACCESS_TOKEN }}
          author_name: ${{ secrets.PUBLIC_GHA_ACCESS_USER }}
          author_email: ${{ secrets.PUBLIC_GHA_ACCESS_EMAIL }}
          k8s_config_repo: dmsi-io/gha-go-deploy
          GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
```

For a further practical example, see [.github/workflows/release.yml](.github/workflows/main.yml).

<!-- badge links -->

[actions-workflow-main]: https://github.com/dmsi-io/gha-k8s-release/actions/query?workflow%3ATest%20gha-k8s-release
[actions-workflow-main-badge]: https://img.shields.io/github/workflow/status/dmsi-io/gha-k8s-release/Test%20gha-k8s-release?label=Test%20gha-k8s-release&style=for-the-badge&logo=github
[release]: https://github.com/dmsi-io/gha-k8s-release/releases
[release-badge]: https://img.shields.io/github/v/release/dmsi-io/gha-k8s-release?style=for-the-badge&logo=github
