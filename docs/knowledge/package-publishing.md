--- 
title: Package Publishing
layout: default
parent: Knowledge WIP
---

# Automated Package publishing


ESLint and CDKTF look way too complex for our simple causes.
Swagger uses a release library that looks promising.

## ESLint 

The publish process for ESLint is handled in the `makefile.js` and relies mainly on the publishRelease function.
The publish API is accessible via a npm script and probably is triggered manually. CI is handled in an own workflow.

**Publish the release package:**
The release operations are delegated to ReleaseOps.publishRelease(), which handles publishing the npm package and probably also the GitHub release. This function comes from the eslint-release tool specialized for ESLint's release process. 

**Read release metadata:**
It reads `.eslint-release-info.json`, a JSON file created during release generation that contains metadata like the release version and package tag.

**Push Git branch with tags:**
It determines the target branch for the docs site based on the package tag (e.g. latest, next, or a maintenance branch like v8.x) and pushes the current Git HEAD to that branch forcefully.

**Publish the website:**
Calls publishSite() which changes directory to the adjacent eslint.org site repo, pushes all commits and tags to GitHub.

**Update changelog and version files on main branch:**
If the current Git branch is not the main branch, it switches to main forcibly, updates the changelog file and the `versions.json` file containing the list of documentation versions with the new release info, then commits and pushes those changes to the remote.

## CDKTF

CDKTF is a monorepo with multiple packages. 
Each package build step gets handled by `lerna`. 
Releases are managed in `sentry`.

Versioning for the packages is handled inside of the `.github\workflow\release.yml`:

```YML
version=$(node -p "require('./package.json').version")
echo "version=${version}" >> $GITHUB_OUTPUT
```

It uses their own library [publib](https://github.com/cdklabs/publib) to publish the artifacts. `npx -p publib publib-npm`

### Release-Flow

**prepare-release:**
Checks out the repo, installs dependencies, and builds/releases the project.
Uses Sentry CLI to manage release creation and uploads source maps.
Uploads build artifacts for downstream jobs.

**Testing Jobs:**
Integration testing, provider integration testing, example testing, linting, and unit tests are run but only if the release is "unreleased" (meaning it is a new release).
Unit tests run in a matrix across various packages and Terraform versions.

**Release Jobs:**
Multiple jobs release the built package to different package managers and platforms, including npm.

## Publib

TLDR: Its doing versioning by using [projen](https://github.com/projen/projen).

**release Job**
- Runs on an Ubuntu runner.
- Checks out the full repo history.
- Sets git identity to "github-actions" for automated commits or tagging.
- Sets up Node.js (using latest LTS).
- Installs dependencies via yarn install.
- Runs `npx projen release` - this runs the `projen release` command which typically generates project files, bumps version, creates changelog, and tags the release commit.
- Checks if the release tag already exists on the remote repo (to avoid duplicate releases).
- Gets the latest commit hash for the current branch.
- Backs up artifact permissions and uploads the dist folder as an artifact if the latest commit matches the pushed SHA.

**release_npm Job**
- Depends on release.
- Also gated to only run if the release tag doesn’t already exist and commit matches.
- Downloads the same dist artifact and restores permissions.
- Runs `npx -p publib@latest publib-npm` to publish the npm package:
- Uses environment variables for npm registry, tag, and authentication token.
- Publishes the contents of the dist directory to the npm registry.

**release_github Job**
- Depends on the release job and the release_npm job.
- Runs only if the release tag does not already exist and the latest commit matches.
- Downloads the dist artifact.
- Restores file permissions.
- Uses the GitHub CLI `gh release` create to create a GitHub release:
- The release tag and title are read from `dist/releasetag.txt`.
- The release notes come from `dist/changelog.md`.
- The release targets the released commit SHA.

## Swagger

[Swagger](https://github.com/swagger-api/swagger-ui/blob/master/.github/workflows/release-swagger-ui.yml) uses [semantic-release](https://github.com/semantic-release/semantic-release) to handle versioning and so on.

{: .note}
It seems rly good. 

## 