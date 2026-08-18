# AI-SDLC Activation Checklist

The automation files are installed. Complete these GitHub-side steps before placing real work into `ai:ready`.

## 1. Organization/repository secrets

Prefer organization-level Actions secrets scoped only to the Prime QA repositories.

### `OPENAI_API_KEY`

Required by `openai/codex-action@v1` in Developer, Reviewer, QA and Objective Auditor jobs.

Security requirements:

- keep it in GitHub Actions secrets only;
- never expose it as repository configuration or source code;
- scope/rotate it according to the provider security policy;
- do not make it available to untrusted fork workflows.

### `PRIMEQA_AUTOMATION_TOKEN`

Required for cross-repository orchestration. Use a fine-grained GitHub PAT or GitHub App token with the minimum organization/repository access needed to:

- read `primeqa-project` requirements/issues;
- send `repository_dispatch` events to Prime QA repositories;
- read/write branches in Prime QA repositories;
- create/update pull requests;
- read checks/PR metadata;
- comment/update/close AI-SDLC issues in `primeqa-project`.

Do not use a personal full-scope classic PAT if a fine-grained token/GitHub App can satisfy these permissions.

## 2. Run `AI-SDLC Bootstrap`

From `primeqa-project` → Actions, run **AI-SDLC Bootstrap** once. It creates/ensures the executable state/agent/control labels defined in `ai-sdlc/github-project-model.md`.

The workflow also runs on changes to its own/bootstrap model, but a manual first run is recommended so activation is explicit.

## 3. GitHub Projects v2 board (recommended, not required for execution)

Create an organization/repository Project named **Prime QA AI-SDLC** and mirror the field model in `ai-sdlc/github-project-model.md`.

The executable automation uses issue labels as its source of state. The Project is the portfolio/visualization layer until a Projects-v2-capable GitHub App/PAT sync is configured.

## 4. Repository Actions policy

Ensure Actions are enabled in all Prime QA repositories and that reusable workflows from `prime-QA-TCMS/primeqa-project` are permitted.

The worker/review wrappers are installed in:

- `primeQA-UI`
- `primeqa-common`
- `fog-ui`
- `user-service`
- `project-service`
- `testcase-service`
- `results-service`
- `configuration-service`
- `primeqa-project`
- `primeqa-qa-ui-engine`
- `primeqa-qa-api-engine`
- `primeqa-qa-test-automation`

New product/service repositories must receive the same thin worker/review wrappers before the orchestrator targets them.

## 5. Branch protection

Recommended on `main` for every code repository:

- require PRs;
- require existing repository CI checks;
- prohibit force-push/deletion;
- prevent direct AI commits to `main`;
- keep human override available for emergency administration.

The AI worker creates draft `ai/*` branches/PRs. Low/medium-risk work may be squash-merged by the verification workflow only after independent review + QA + objective verification. High/critical/shared-package changes stop for human approval.

## 6. Environment-dependent test configuration

The AI-SDLC does not invent a runtime test environment. Configure the QA automation repository with environment-specific endpoints/credentials using GitHub Environments/Secrets before expecting live API/UI/cross-layer suites to execute.

Where a QA gate cannot obtain required environment evidence it must escalate rather than claim success.

## 7. First controlled test

Use a low-risk documentation or isolated-test work item first.

1. Create it from the `AI-SDLC Work Item` issue form.
2. Give it exactly one target repository.
3. Ensure it has `ai:managed` and `state:intake`.
4. Review the requirement/acceptance criteria.
5. Add `ai:ready`.
6. Observe: implementation dispatch → draft PR → independent review → QA → verification → merge/Done.

Do not use the first activation run for auth, tenancy, retention, destructive data, shared-package breaking changes or deployment infrastructure.

## 8. Human approval behavior

Automation stops at `state:human-decision` + `human:required` when:

- product/architecture/security decisions are unresolved;
- Objective Auditor requests human evidence;
- item risk is High/Critical at final verification;
- `shared-package` is present at final verification;
- GitHub repository/branch policy refuses automated merge;
- an explicitly human-gated operation is required.

To resume, resolve the decision/evidence, remove `human:required`, place the item into the correct lifecycle state, and add `ai:ready` or `ai:retry` as appropriate.
