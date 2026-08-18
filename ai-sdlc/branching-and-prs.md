# AI-SDLC Branching and Pull Request Policy

## Branch model

Default branch: `main`.

Agents must not implement directly on `main`. Create short-lived branches using:

- `feature/<work-item-id>-<short-name>`
- `fix/<work-item-id>-<short-name>`
- `refactor/<work-item-id>-<short-name>`
- `docs/<work-item-id>-<short-name>`
- `test/<work-item-id>-<short-name>`

Cross-repository work uses the same work-item ID across repositories to preserve traceability.

## Pull request requirements

Every implementation PR must include:

- work-item ID;
- parent requirement/objective IDs;
- problem/outcome summary;
- acceptance criteria addressed;
- affected contracts/data/configuration;
- tests added/updated and evidence;
- dependencies/order relative to other PRs;
- migration/rollback considerations where relevant;
- security/NFR considerations;
- known residual risks;
- explicit note if another repository must merge/release first.

## Draft-first workflow

Agents should open a draft PR once the change has enough structure to make scope visible. The PR becomes ready for review only after developer validation passes and the implementation agent records evidence.

## Cross-repository ordering

When a change spans repositories:

1. identify compatibility strategy before implementation;
2. prefer backwards-compatible shared/API changes first;
3. update consumers next;
4. remove deprecated behaviour only after consumers have migrated;
5. use linked PRs with explicit merge/release order.

A parent work item remains incomplete until the integrated behaviour passes.

## Shared packages

### `primeqa-common`
Treat package version/contract compatibility as a release boundary. Do not merge a change that knowingly breaks consuming services without an approved migration sequence.

### `fog-ui`
Reusable component changes must preserve or deliberately version public props/behaviour and automation interfaces. Product-specific hacks should remain in `primeQA-UI`, not be hidden inside shared components.

## PR review separation

The final code-review context must be independent from the implementation context. The reviewer may request changes, approve, or escalate architecture/product conflicts. The implementer may resolve findings but cannot close a blocking finding without evidence.

## Merge rules

Merge only when:

- branch is current enough to validate against target;
- required CI passes;
- blocking review findings are resolved;
- QA/security/NFR gates applicable to the change pass;
- dependency PR ordering is satisfied;
- no unresolved human-decision gate remains.

Prefer squash merge for normal work unless repository history or a specific migration requires preserving commits.
