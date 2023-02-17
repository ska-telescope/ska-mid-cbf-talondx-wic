# ska-mid-cbf-talondx-wic

This repository is intended for uploading the Mid.CBF TALON-DX's WIC file in a raw package to the [Central Artefact Repository](https://artefact.skao.int/). 
It is based on the [ska-raw-skeleton](https://gitlab.com/ska-telescope/templates/ska-raw-skeleton) repository.

## Initialization

### Makefiles
Refer to https://gitlab.com/ska-telescope/sdi/ska-cicd-makefile

### Prerequisites
- Git
- [Git Large File Storage:](https://git-lfs.github.com/) 
    ```bash
    apt install git-lfs
    ```

Clone the repository locally, then set up the `.make` submodule and `git-lfs`.
```bash
git submodule init
git submodule update
git lfs install
```
*Note: `git lfs install` need only be run once per user.*

## Release WIC raw package
1. On branch `main`, add the wic file to `raw/ska-mid-cbf-talondx-wic/`. Each sub-directory of `raw/` prefixed with `ska-` will be packaged and uploaded to the CAR separately upon tag pipeline success, so delete any directories not intended for release; currently, only the `ska-mid-cbf-talondx-wic` package is used.

2. Update the `.release` file version; if simply incrementing the current 
semver, can use the following make rules: `make bump-patch-release`, 
`make bump-minor-release` or `make bump-major-release`.

The Artifact in CAR will have the version in the .release file appended to it.
For example if version in .release is "1.0.1", the final CAR package will be "ska-mid-cbf-talondx-wic-1.0.1.tar.gz"

    *Note: currently the `.make` submodule applies the same semver tag (from `.release`) to all packages created from the sub-directories of `raw/`.*

3. Commit changes and tag release. Ensure that LFS pointers are staged properly for commit with `git lfs status`;
```bash
git add .
git lfs status
```
Output should say `LFS` next to files in parentheses:
```bash
...
Git LFS objects to be committed:

        raw/ska-mid-cbf-talondx-wic/...wic(LFS: ...)
...
```
To commit, tag and push both in fewer commands:
```bash
git commit -m <message>
git tag <tag>
git push --atomic origin main <tag>
```

## How to use
Refer to https://confluence.skatelescope.org/display/SE/Updating+SD+Card for information on how to burn the .wic image to a sd-card.
