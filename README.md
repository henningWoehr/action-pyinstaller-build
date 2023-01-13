# action-pyinstaller-build
GitHub Action for python projects using poetry

## Description
With this action you can easily build your poetry-project for linux, windows and mac using pyinstaller. The only thing that needs to be provided is a spec file, which can be created using [this](https://pyinstaller.org/en/stable/spec-files.html) information. The path is then passed into the action (See the example below). After building, the project gets uploaded to an artifact. To download it see the example or the [github help](https://docs.github.com/en/actions/managing-workflow-runs/downloading-workflow-artifacts).

## Action Inputs
| Input name | Description | Required | Default Value |
| --- | --- | --- | --- |
| spec-path | Path of the spec file containing the information for pyinstaller | true | - |
| zip-name | Name of the zipped folder containing the built project (To differentiate between linux, etc.) | true | - |
| artifact-name | Name of the artifact, where the built project gets uploaded | true | - |
| python-version | Version of python to install | false | "3.10" |
| poetry-version | Version of poetry to install | false | "1.1.14" |

## Example

```yaml
name: Build with pyinstaller

on: 
  push:
    branches:
    - main

env:
  SPEC_PATH: project_name.spec
  ARTIFACT_NAME: artifact_name

jobs:

  build_linux:
    runs-on: ubuntu-20.04
    
    steps:
    - name: build executable
      uses: henningwoehr/action-pyinstaller-build@v1
      with:
        spec-path: ${{ env.SPEC_PATH }}
        artifact-name: ${{ env.ARTIFACT_NAME }}
        zip-name: "linux"
        python-version: "3.10"
        poetry-version: "1.1.14"

  
  build_win:
    runs-on: windows-2022
    
    steps:
    - name: build executable
      uses: henningwoehr/action-pyinstaller-build@v1
      with:
        spec-path: ${{ env.SPEC_PATH }}
        artifact-name: ${{ env.ARTIFACT_NAME }}
        zip-name: "windows"
        python-version: "3.10"
        poetry-version: "1.1.14"

  download_files:
    runs-on: ubuntu-latest
    needs: [build_linux, build_win]

    steps:
      - name: Download files
        uses: actions/download-artifact@v3
        with:
          name: ${{ env.ARTIFACT_NAME }}
          path: download

      - name: List files
        run: ls download
```