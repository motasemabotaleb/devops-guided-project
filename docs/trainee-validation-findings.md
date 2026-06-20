# Trainee Validation Findings

This file captures issues found while walking the project end to end like a fresh trainee.

Use it as the improvement backlog for the next polish pass.

## 2026-06-20 Dry Run

### Confirmed Strengths

- the guided README sequence is much clearer than a flat project README
- the milestone validation scripts create a good sense of progress
- the VM setup script correctly fixes Docker access after a new SSH session
- the app and Nginx logs now show a useful request cycle for teaching
- the VM deployment script succeeds cleanly with a pulled image

### Issues Found

#### 1. Linux Docker access can fail even when Docker is installed

- symptom: `docker ps` failed for `azureuser` even though Docker was running
- root cause: the user was not yet in the `docker` group
- impact: a trainee can believe Docker is broken when the fix is just group membership plus a new shell
- follow-up: keep the Linux `docker` group reminder in prerequisites and troubleshooting

#### 2. VM Node installation is heavier than the docs suggest

- symptom: installing `nodejs` and `npm` on Ubuntu pulled a very large package set
- impact: the setup feels slower and noisier than a beginner expects
- follow-up: consider a tighter guided install path for Node.js on Linux or explain that local laptop testing is the main Node use case

#### 3. Public VM HTTP validation can be distorted by the trainee network

- symptom: external HTTP to the VM returned a middlebox redirect instead of the app response
- impact: a trainee may blame the project when the issue is the local network path
- follow-up: keep SSH tunnel validation steps visible and mention cloud firewall plus ISP or captive-network interception in troubleshooting

#### 4. Observability validator originally disagreed with the intended architecture

- symptom: `scripts/validate-observability.sh` failed because it expected `/metrics` through Nginx
- root cause: `/metrics` is intentionally private, so Prometheus should be queried instead
- impact: the script taught the wrong expectation
- follow-up: keep the validator aligned with the private-metrics design

#### 5. Nginx error log emptiness is not a failure by itself

- symptom: the validator failed when `logs/nginx/error.log` existed but was empty
- impact: a healthy deployment looked partially broken
- follow-up: validate that the file exists, and only expect entries after an error scenario

### GitHub Journey Blocker

- local GitHub CLI authentication was expired during this dry run
- impact: the full GitHub repository creation, Actions secret setup, and workflow trigger path could not be completed from this workstation without re-authentication
- follow-up: once GitHub auth is restored, complete:
  - create the `devops-guided-project` repository
  - add VM, registry, and Azure secrets
  - push the repo
  - run `ci-build-push.yml`
  - run `deploy-vm.yml`

## 2026-06-20 GitHub Actions Follow-Up

### Confirmed Working

- the GitHub repository was created successfully at `iabouemira95/devops-guided-project`
- `CI Build Push` succeeded on `main`
- the workflow ran tests, built the image, and published:
  - `ghcr.io/iabouemira95/devops-guided-project/devops-mini-app:latest`
  - `ghcr.io/iabouemira95/devops-guided-project/devops-mini-app:sha-22f8b59`

### Remaining Gaps

#### 6. `deploy-vm.yml` had a workflow parser defect

- symptom: workflow dispatch failed with `Unrecognized named-value: 'secrets'`
- root cause: step `if:` expressions referenced `secrets` directly
- status: fixed in the workflow

#### 7. GitHub-hosted VM deploy is still blocked by SSH authentication

- symptom: the deploy workflow now dispatches and reaches the SSH step, but the GitHub-hosted runner receives `Permission denied (publickey)`
- evidence: local SSH from the instructor machine works with the same VM key, while the GitHub workflow runner does not
- impact: the full trainee GitHub deploy path is not yet complete
- likely next checks:
  - verify the exact private key format expected by the VM against the stored GitHub secret
  - verify whether the VM or cloud environment applies any SSH restrictions that differ for GitHub-hosted runners
  - test with a dedicated deployment key generated specifically for GitHub Actions

#### 8. The registry story is still split between GHCR and ACR during validation

- symptom: CI publishes to GHCR, while the manually validated VM runtime in this dry run used ACR credentials and an ACR image path
- impact: the core student story should converge on one default registry path
- follow-up: decide one default deploy registry for the guided path, then align:
  - `.env` and `deploy/example.env`
  - GitHub deploy secrets
  - LAB-06 and LAB-07 wording
  - validation examples

## Next Review Focus

- complete the GitHub Actions path with valid GitHub authentication
- verify the app from a browser on a network path that does not rewrite plain HTTP
- re-run all milestone validators after the GitHub path is in place
