# Framecut build runners

Public GitHub-hosted runner definitions for building and validating Framecut's
private GPU worker images. The application source remains in the private
`SakethSripada/VideoEditor` repository and is checked out at an explicit commit
with a read-only deploy key.

## Security boundary

- Every workflow is manual-only. Pushes and pull requests cannot execute code.
- Private source is checked out at an exact 40-character commit SHA.
- The deploy key can only read `VideoEditor` and checkout credentials are not
  persisted.
- Third-party actions are pinned to full commit SHAs.
- Source, Docker contexts, and model layers are never uploaded as public Actions
  artifacts or stored in the public Actions cache.
- Worker images and BuildKit caches are pushed only to private GHCR packages.
- Workflow inputs are constrained choices, booleans, or validated identifiers;
  no input is evaluated as a shell command.

## Manual run order

1. Run **Public runner canary**.
2. Run **Build private GPU workers** for the exact private source SHA.
3. Copy the immutable image digests from the job summary into the RunPod
   provisioning manifest.
4. Reconcile zero-minimum-worker RunPod endpoints.
5. Run image smoke tests, provider primitives, then full assistant acceptance.

The build workflow requires these encrypted repository secrets:

- `VIDEOEDITOR_DEPLOY_KEY`: dedicated read-only deploy key.
- `HF_TOKEN`: Hugging Face token passed only as a BuildKit secret.
- `GHCR_TOKEN`: classic PAT limited to `write:packages` and `read:packages`.

`RUNPOD_API_KEY`, `OPENAI_API_KEY`, and `TWELVELABS_API_KEY` are reserved for
the validation stage and are not exposed to image-build jobs.
