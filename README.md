## Check updated repo path

This example demonstrates how to find out if files under a specific folder are updated in a commit, comparing with it parent.

#### Checkout

Azure Pipeline will check out its default repository `self` if no `checkout` step is specified in the pipeline. The [default checkout](https://dev.azure.com/zuer0720/pipeline_project/_build/results?buildId=258&view=logs&j=12f1170f-54f2-53f3-20dd-22fc7dff55f9&t=af08acd5-c28a-5b03-f5a9-06f9a40627bb&l=33) is with depth 1.

```bash
git ... fetch ... --depth=1
```

As we are comparing at least two commits, e.g., `HEAD` and `HEAD~`, checkout of depth 1 wouldn't be enough. To resolve this issue, we need to add a checkout step in the pipeline:

```yaml
- checkout: self
  fetchDepth: 2
```

#### Find the updates

We use `git diff --name-only <commit1> <commit2>` to list all the updates in between two commits. The result will be similar as below:

```bash
..check-repo-path [main ≡ +0 ~2 -0 !]> git diff --name-only HEAD HEAD~3
azure-pipeline.yml
env/.gitkeep
env/file2.yml
src/file2.cs
```

Once the result is captured, and processed by a Powershell script, a variable is set to indicate if update happens on a given path. This variable then is used as condition for next step.


