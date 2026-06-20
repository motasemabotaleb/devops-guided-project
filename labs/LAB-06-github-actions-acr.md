# LAB-06 GitHub Actions ACR

## Goal

See how the app becomes a reusable image in Azure Container Registry.

## Problem Scenario

Running locally is not enough. Teams need a repeatable image build and push flow.

## Files Used

- `.github/workflows/ci-build-push.yml`
- `docker/app.Dockerfile`

## Commands to Run

```bash
git status
```

Then perform one of these guided checks:

1. Open the Actions tab and inspect the latest `CI Build Push` run.
2. If you have registry access, inspect the `devops-mini-app` image tags in ACR.

## GUI Actions to Click

None required.

## Expected Output

- PR runs tests only
- push to `main` builds and pushes image
- image tags include `latest` and `sha-<short-sha>`
- students can point to one run that only validated and one run that published
- students can identify the exact SHA tag that would be used for a recovery deploy

## Checkpoint Questions

- Why do PRs test but not push?
- Why is the SHA tag useful for recovery?
- Where in the workflow logs do you confirm the final pushed image names?
- Where in GitHub do you confirm that the package was actually published?

## Common Issues

- ACR credentials missing
- workflow not running on the expected branch
- students read the YAML but never verify the real workflow outcome

## Team Task Split

- Student 1 reads workflow triggers
- Student 2 reads build steps
- Student 3 verifies the exact published image names from the workflow run
- Student 4 verifies the matching image tags in ACR and explains which secrets make the push path work

## Instructor Checkpoint

Have teams explain the difference between validation and publishing, then show:

- one workflow run that tested only
- one workflow run that published
- one SHA tag they could redeploy later

## Next Step

Read [Registries](../docs/registries.md), then [Azure Key Vault and Secrets Flow](../docs/secrets-and-azure-key-vault.md), then continue to [LAB-07 Deploy to VM](LAB-07-deploy-to-vm.md).
