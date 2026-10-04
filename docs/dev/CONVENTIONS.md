# Conventions

What is shared with the sibling CLIs is written once, in max-cli's
[`CONVENTIONS.md`](https://github.com/leemour/max-cli/blob/main/docs/dev/CONVENTIONS.md): Biome decides
formatting, strict TypeScript with no `any`, comments only for *why*, the environment as arguments,
correct documents in place. Command and MCP names follow cli-messaging's
[`STANDARD.md`](https://github.com/leemour/cli-messaging/blob/main/docs/dev/STANDARD.md). This page
adds only what differs.

## Code

**`src/` is the package.** Every `.ts` file under it is published. A new entry point is a new
`exports` entry in `package.json` and a line in [`scripts/smoke.ts`](../../scripts/smoke.ts).

**The clock and the id are arguments.** `createTaskService` takes `now` and `newId`; the real ones
are only defaults, so a test never waits and never guesses an id.

## Documents

English. `README.md` is the user page: current facts only, no correction marks, no backlog or
decision ids. `docs/dev/` is for whoever works on the code; a claim there that turns out wrong is
corrected in place and marked.

## The changelog

`CHANGELOG.md`, newest first. The top section is `## Unreleased` while work is merged; a release
dates it as `## <version> — DD.MM.YYYY`. Headings, each at most once per version: `Added`,
`Changed — may break callers`, `Fixed`, `Security`, `Removed`. Every entry says what changed as a
caller sees it, why, and what to watch for. `pnpm docs:check` checks the shape.
