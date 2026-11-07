# CURL AI Bridge CI

Public GitHub Actions runner controller for the private
[`suniltnngl-gm/curl-ai-bridge`](https://github.com/suniltnngl-gm/curl-ai-bridge)
repository.

This repository contains CI orchestration only. It does **not** mirror or publish
the private application source. A manually triggered workflow checks out a
selected source ref using a narrowly scoped read-only token, then runs the
source repository's validation and test suite on GitHub-hosted runners.

## One-time setup

1. In GitHub, create a **fine-grained personal access token**.
2. Set the resource owner to `suniltnngl-gm`.
3. Restrict repository access to **only** `curl-ai-bridge`.
4. Grant **Contents: Read-only**. Do not grant write, Actions, administration,
   or other permissions.
5. Add the token to this repository under **Settings → Secrets and variables
   → Actions → New repository secret**:
   - Name: `MAINTAINER_REPO_READ_TOKEN`
   - Secret: the fine-grained token created above.
6. Give the token an expiry and rotate it when needed.

Do not put the token in source files, workflow inputs, issues, or logs.

## Run CI

1. Open the **Actions** tab.
2. Select **Private source CI**.
3. Select **Run workflow**.
4. Enter the source branch, tag, or commit SHA in `source_ref`.
5. Run the workflow and inspect all Node 18, 20, and 22 jobs.

For the current Codespaces PR #43 snapshot, use:

```text
codespaces-node22-devcontainer
```

For reproducible verification, prefer the exact source commit SHA:

```text
09a6a2c217187db466826521b3cefb63a5f87d3e
```

## Security boundary

- The workflow is `workflow_dispatch` only; it does not run for public pull
  requests or arbitrary pushes.
- The private-source token is used only by the checkout step and checkout
  credential persistence is disabled.
- Workflow permissions are read-only.
- No deployment, release, write-back, or automatic merge is performed.
- The source remains private; only CI orchestration and its logs are public.
- Anyone able to change this public workflow could alter how its secret is
  used. Keep write access restricted and review workflow changes carefully.

## Provenance

Every run records the requested source ref and the resolved source commit SHA
in the GitHub Actions job summary. Treat the run result as verification evidence
for that exact commit, not as approval to merge it into the private repository.
