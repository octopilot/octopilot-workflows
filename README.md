# sample-custom-workflows

Sample reusable or copy-paste GitHub Actions workflows for the Octopilot ecosystem.

**Pattern**: Repos like `sam-activity-service` call shared workflows from `sam-custom-workflows` via `uses: org/sam-custom-workflows/.github/workflows/<name>@branch`. This repo holds **sample** workflows you can copy into your repo or adapt for your own custom-workflows-style repo.

## Workflows

| Workflow | Reusable | Description |
|----------|----------|-------------|
| [octopilot-bootstrap.yml](.github/workflows/octopilot-bootstrap.yml) | Yes (`workflow_call`) | Check that `.github/octopilot.yaml` exists. Used by Octopilot Probot in target repos via `uses: owner/sample-custom-workflows/.github/workflows/octopilot-bootstrap.yml@main`. |
| [release.yml](.github/workflows/release.yml) | Yes (`workflow_call`) + `workflow_dispatch` | Manual release: bump version (Cargo.toml via `rerp`), generate release notes (OpenAI/Anthropic), commit, tag, push, create GitHub Release. Target repos get a thin caller that `uses:` this workflow. Source: [microscaler/rerp](https://github.com/microscaler/rerp). |

### release.yml

- **Trigger**: `workflow_dispatch` (manual) with inputs: `bump` (patch|minor|major|rc|release), `branch`, `provider` (openai|anthropic).
- **Requires**: Repo with `tooling/rerp` (pip-installable). Secrets: `REPO_PAT`; optionally `OPENAI_API_KEY` / `ANTHROPIC_API_KEY` for AI-generated notes.
- **Use**: Copy into your repo’s `.github/workflows/` and configure secrets; or reference from a custom-workflows repo if you make it a `workflow_call` and run in the caller repo.

## Adding workflows

Add new YAML under `.github/workflows/`. Prefer reusable workflows (`workflow_call`) if other repos should call them; otherwise use `workflow_dispatch` or `push`/`pull_request` as needed. Document each in this README and note the source repo if adapted from elsewhere.
