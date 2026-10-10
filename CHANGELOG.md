# Changelog

Notable changes to `@wirecat/cli-tasks` (`@leemour/cli-tasks` up to 0.1.0), one section per version, newest
first. Versions follow [semantic versioning](https://semver.org/); before `1.0.0` a minor release may change
the API.

Every entry says what changed as a caller sees it, why, and what to watch for — the rules are
[`docs/dev/CONVENTIONS.md`](docs/dev/CONVENTIONS.md#the-changelog).

## 0.3.0 — 10.10.2026

### Added

- `TASK_VERDICTS` (`useful`, `not_useful`) and the `TaskVerdict` type: the owner's judgement of a task a rule or
  an agent raised, so a host and its callers share one list.

### Changed — may break callers

- **`TaskStore` gains `answer(id, { resolution, by? })` and `judge(id, verdict | null)`.** The shared store keeps
  a question's answer and the owner's verdict; the port now says so, matching cli-messaging's `StoreTaskStore`.
  A host that implements `TaskStore` must add both; `memoryTaskStore` has them. `Task` is unchanged: neither the
  answer nor the verdict is read back through the port yet.

### Security

- Fast secret checks remain on PRs; source, production dependency and workflow security checks run before publication. Automatic Socket checks are disabled.

## 0.2.1 — 10.10.2026

### Changed — may break callers

- **The project is now licensed under Apache License 2.0.** See `LICENSE` for the terms.

## 0.2.0 — 10.10.2026

### Changed — may break callers

- **The package is now `@wirecat/cli-tasks`, and the repository is `WireCatLabs/cli-tasks`.** Install
  `@wirecat/cli-tasks` and change imports from `@leemour/cli-tasks`. It depends on `@wirecat/cli-core`
  0.18.1 (was `@leemour/cli-core` 0.17.0). `@leemour/cli-tasks` gets no new versions.

## 0.1.0 — 05.10.2026

### Added

- **The task model and its rules.** A task points at its source by a locator and holds no text of
  it; it is `open`, then `done` or `dismissed`, and a closed task never opens again.
- **`createTaskService`** adds, closes, lists and counts tasks over a `TaskStore` the host
  implements. A rule that sees a source again gets the task already there, whatever its state, so
  a dismissed task stays dismissed; a person or an agent may add a task of another kind to the same
  source.
- **`@leemour/cli-tasks/testing`** has `memoryTaskStore`, an in-memory `TaskStore` for tests.
