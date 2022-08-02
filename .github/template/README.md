# Trigger Doc Pull from Repo

CaraML connsists of several components such as Model, Experimentation and Data which are stored in separate repository. However, we rely on consolidating documentations from these different repository in the CaraML Docs repository in order to build the Documentation Gitbook.

To ensure that our Gitbook is always up to date, we created a workflow called `Pull Docs CI` that will pull the latest documentations from the respective component repository automatically. This workflow is triggered by the respective repository whenever there is a merge to the main branch, a new tag created or whenever there are new commits to a branch in pull request.

However, this requires us to set up a triggering workflow in the respective component repository, say R. In order to do that, follow the steps below:

1. Copy the file `trigger-caraml-doc-sync.yml` into the workflow directory of R. i.e. .github/workflows/
2. Modify the `inputs` of the workflow dispatch function. Namely,
    * module: the name of the module. for example model, data or experiment
    * git_https: the .git value to clone the repository via https
    * doc_folder: the folder where all the documentations are stored in the repository
    * branch: the branch from which to copy the documentations from
3. Notice in the same file that a secret key (Personal Access Token) called `CARAML_SYNC` is being used. This is required to trigger the `Pull Docs CI` workflow. Ask the administrator for this token and add it to R as a `Repository secrets` under settings > Secrets > Actions. You may need appropriate access rights to view/edit this.