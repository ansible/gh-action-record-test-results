[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/ansible/gh-action-record-test-results/main.svg)](https://results.pre-commit.ci/latest/github/ansible/gh-action-record-test-results/main)

# `ansible/gh-action-record-test-results`

This is a GitHub Action for uploading JUnit test results to the
aggregated dashboard.

## Example use

```yaml
- name: >-
  Upload ${{
    matrix.tests.coverage-upload-name || 'awx'
  }} jUnit test reports to the unified dashboard
if: >-
  !cancelled()
  && steps.make-run.outputs.test-result-files != ''
  && github.event_name == 'push'
  && env.UPSTREAM_REPOSITORY_ID == github.repository_id
  && github.ref_name == github.event.repository.default_branch
uses: ansible/gh-action-record-test-results@cd5956ead39ec66351d0779470c8cff9638dd2b8
with:
  aggregation-server-url: ${{ vars.PDE_ORG_RESULTS_AGGREGATOR_UPLOAD_URL }}
  http-auth-password: >-
    ${{ secrets.PDE_ORG_RESULTS_UPLOAD_PASSWORD }}
  http-auth-username: >-
    ${{ vars.PDE_ORG_RESULTS_AGGREGATOR_UPLOAD_USER }}
  project-component-name: >-
    ${{ matrix.tests.coverage-upload-name || 'awx' }}
  test-result-files: >-
    ${{ steps.make-run.outputs.test-result-files }}
```
