# Workflow to Generate a "Tag" and a "Release" Version Using Semantic Versioning

This workflow is responsible for automated semantic versioning of applications and dependencies (workflows, actions, and so on) in accordance with **semantic versioning** standards in commit messages (**[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)**). Since this is a **Reusable Workflow**, it can be used across multiple repositories. So this will help to avoid duplication as we can reuse it in existing workflows. To learn more about this, click **[here](https://docs.github.com/en/actions/using-workflows/reusing-workflows)**. Also, if you have any questions about **Semantic Versioning**, you can access the **[Semantic Versioning 2.0.0](https://semver.org/lang/pt-BR/)** article. In short, after the run of this workflow, will be generated a **"Tag"** and a **"Release"** version for that repository that is calling this function.

<br>

Furthermore, a template using this workflow has already been created. So, if you want to use automated versioning in your repository using semantic versioning based on conventional commits, you can choose **[template-semantic-versioning-automation](https://github.com/BancoArbi/template-semantic-versioning-automation)** when creating a new repository.

<br>

## Event That Trigger This Workflow

To trigger this **workflow**, we will use the following command once this is a **"called"** workflow:

<br>

```yaml
on:
  workflow_call:
```

<br>

## Relevant Job Settings

This workflow is running in **GitHub-Hosted Runner**, which in this case is **`ubuntu-latest`**:

<br>

```yaml
jobs:
  Workflow-versioning:
    runs-on: ubuntu-latest
```

<br>

## Job Steps

The steps that this workflow runs in the **"Workflow-versioning"** job are:

<br>

### 1) Workflow version

In this step will be used the **[github-semantic-versioning-actions](https://github.com/BancoArbi/github-semantic-versioning-actions)** action that is responsible for generating an automated semantic version from a Git repository commit history. To understand more about this action, click **[here](https://github.com/BancoArbi/github-semantic-versioning-actions#readme)** and read the documentation in **README.md**:

<br>

```yaml
- name: Workflow version
  id: versioning
  uses: BancoArbi/github-semantic-versioning-actions@v1.0.0
```

<br>

### 2) Create Release

This action simplifies the **GitHub** release process by automatically uploading assets, generating change logs, processing pre-releases, and so on. Also, this action will only be performed if **`github.ref`** does not represent a tag reference, but a branch. Thus, at the end of the execution of this action, a release will be created in the **Release** tab of **GitHub**, which will represent the generation of a Release package. This Release will have the name formatted as stipulated in the step: **"Workflow version"**. As this version does not represent a pre-release version, it is necessary to put the value **"false"** in the **`prerelease`** key:

<br>

```yaml
- name: Create Release
  if: ${{ !startsWith(github.ref, 'refs/tags/') }}
  id: version
  uses: "marvinpinto/action-automatic-releases@latest"
  with:
    repo_token: "${{ secrets.GITHUB_TOKEN }}"
    title: ${{ steps.versioning.outputs.version }}
    automatic_release_tag: ${{ steps.versioning.outputs.version }}
    prerelease: false
```

<br>

## Dependencies and Requirements

It was necessary to make this repository accessible from other repositories in the organization through **Settings > Actions > General > Access > Accessible from repositories in the organization 'Name_Organization'**.
