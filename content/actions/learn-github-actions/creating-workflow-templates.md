---
title: Creating starter workflows
shortTitle: Creating starter workflows
intro: Learn how you can create starter workflows to help people in your team add new workflows more easily.
redirect_from:
  - /actions/configuring-and-managing-workflows/sharing-workflow-templates-within-your-organization
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: tutorial
topics:
  - Workflows
  - CI
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}

## Overview

{% data reusables.actions.workflow-organization-templates %}

## Creating a Starter Workflow

Starter Workflows can be created by users with write access to the organization's `.github` repository. These can then be used by organization members who have permission to create workflows.

{% ifversion fpt %}
These workflows can be created in public repositories only. Organizations using {% data variables.product.prodname_ghe_cloud %} can also create these workflows in private repositories. For more information, see the [{% data variables.product.prodname_ghe_cloud %} documentation](/enterprise-cloud@latest/actions/learn-github-actions/creating-workflow-templates).
{% endif %}

{% ifversion fpt or ghes > 3.3 or ghae-issue-4757 or ghec %}
{% note %}

**Note:** To avoid duplication among starter workflows you can call reusable workflows from within a workflow. This can help make your workflows easier to maintain. For more information, see "[Reusing workflows](/actions/learn-github-actions/reusing-workflows)."

{% endnote %}
{% endif %}

This procedure demonstrates how to create a workflow and metadata file. The metadata file describes how these starter workflows will be presented to users when they are creating a new workflow.

1. If it doesn't already exist, create a new public repository named `.github` in your organization.
2. Create a directory named `workflow-templates`.
3. Create your new workflow file inside the `workflow-templates` directory.

   If you need to refer to a repository's default branch, you can use the `$default-branch` placeholder. When a workflow is created the placeholder will be automatically replaced with the name of the repository's default branch.

   For example, this file named `octo-organization-ci.yml` demonstrates a basic workflow.

   ```yaml
   name: Octo Organization CI

   on:
     push:
       branches: [ $default-branch ]
     pull_request:
       branches: [ $default-branch ]

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
         - uses: actions/checkout@v2

         - name: Run a one-line script
           run: echo Hello from Octo Organization
   ```
4. Create a metadata file inside the `workflow-templates` directory. The metadata file must have the same name as the workflow file, but instead of the `.yml` extension, it must be appended with `.properties.json`. For example, this file named `octo-organization-ci.properties.json` contains the metadata for a workflow file named `octo-organization-ci.yml`:
   ```yaml
   {
       "name": "Octo Organization Workflow",
       "description": "Octo Organization CI workflow template.",
       "iconName": "example-icon",
       "categories": [
           "Go"
       ],
       "filePatterns": [
           "package.json$",
           "^Dockerfile",
           ".*\\.md$"
       ]
   }
   ```
   * `name` - **Required.** The name of the workflow. This is displayed in the list of available workflows.
   * `description` - **Required.** The description of the workflow. This is displayed in the list of available workflows.
   * `iconName` - **Optional.** Defines an icon for the workflow's entry among the list of workflows. The `iconName` must be an SVG icon of the same name, and must be stored in the `workflow-templates` directory. For example, a SVG file named `example-icon.svg` is referenced as `example-icon`.
   * `categories` - **Optional.** Defines the language category of the workflow. When a user views the available workflows, those workflows that match the same language will feature more prominently. For information on the available language categories, see https://github.com/github/linguist/blob/master/lib/linguist/languages.yml.
   * `filePatterns` - **Optional.** Allows the workflow to be used if the user's repository has a file in its root directory that matches a defined regular expression.

To add another workflow  add your files to the same `workflow-templates` directory. For example:

![Workflow files](/assets/images/help/images/workflow-template-files.png)

## Next steps

To continue learning about {% data variables.product.prodname_actions %}, see "[Using workflow ](/actions/learn-github-actions/using-workflow-templates)."
