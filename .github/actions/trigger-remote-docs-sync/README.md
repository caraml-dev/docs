# Tigger Remote Docs Sync Action

CaraML consists of several components such as Model, Experimentation and Data which are stored in separate repository. However, we rely on consolidating documentations from these different repository in the CaraML Docs repository in order to build the Documentation Gitbook.

To ensure that our Gitbook is always up to date, we created a workflow called `Remote Docs Sync` that will sync the latest documentations from the component repository automatically. This workflow can triggered by the component repository whenever required, using this `trigger remote docs sync` action.

This action allows us to trigger the `Remote Docs Sync` workflow from the component repository when run from there.

Example of how to set up this action in a workflow:

```example-workflow.yml
# This workflow triggers sync of docs to caraml-dev/docs

name: Trigger CaraML Docs Sync

on:
  push:
    tags:
    - v*

jobs:
  trigger-sync:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Remote Doc Sync Workflow
        uses: caraml-dev/docs/.github/actions/trigger-remote-docs-sync@main
        with:
          module: 'model'
          git_https: 'https://example.com/example.git'
          doc_folder: 'example_docs'
          ref_name: ${{ github.ref_name }}
          ref_type: ${{ github.ref_type }}
          credentials: ${{ secrets.CARAML_SYNC }}
```

In the example workflow yaml above, inputs to the action can be customized to needs:
* module: the name of the module. for example model, data or experiment
* git_https: the .git value to clone the repository via https
* doc_folder: the folder where all the documentations are stored in the repository
* ref_name: the branch or tag to copy from
* ref_type: the type of ref. Valid values are branch or tag
* credentials: this is the personal access token to caraml-dev/docs repository, stored as a repository secret in the component repository.

Note: Notice in the same file that a secret key (Personal Access Token) called `CARAML_SYNC` is being used. This is required to trigger the `Remote Docs Sync` workflow. Ask the administrator for this token and add it as a `Repository secrets` under settings > Secrets > Actions to the component repository. You may need appropriate access rights to view/edit this.