# ci-templates

Reusable GitHub Actions workflows called by each repository's thin `orchestrator-*.yml` callers.

| Template | Called for | Uploads |
|---|---|---|
| `build-docker.yml` | build stage | `build-manifest` (orchestrator-build-manifest/v1) |
| `scan-grype.yml` | security scan stage | `scan-results`: grype.json, grype.sarif, sbom.cdx.json, target.json |
| `test-node.yml` | unit test stage | `test-results`: summary.json, junit.xml |

Rules every template follows:

- Build or test exactly `inputs.sha`; scan and deploy exactly `inputs.target_ref` (pinned to `inputs.target_digest`).
- Report results as artifacts and **don't fail the run on findings or failing tests**. The orchestrator
  evaluates gates; a failed run means the pipeline itself is broken.
- No inputs that filter or suppress findings.

Callers reference a tag (`@v1`). Tag releases, and bump the tag in the orchestrator workflow config
(`customization.template.version`) when repositories should move to a new version.
