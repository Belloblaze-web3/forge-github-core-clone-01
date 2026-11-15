# Forge Core

[![make.fun bounty](https://make.fun/badge/github-core-clone-01.svg)](https://make.fun/bounty/github-core-clone-01)
[![rank](https://make.fun/badge/github-core-clone-01/Belloblaze-web3/forge-github-core-clone-01/rank.svg)](https://make.fun/bounty/github-core-clone-01)
[![reward](https://make.fun/badge/github-core-clone-01/Belloblaze-web3/forge-github-core-clone-01/reward.svg)](https://make.fun/bounty/github-core-clone-01)

Forge Core is a standalone, dependency-free GitHub-compatible git forge implemented for Node.js 22. It runs as a single process, keeps its own in-memory repository store, and requires no database, external service, package installation, or `git` executable.

## Capabilities

- Git smart HTTP for repository advertisement, protocol v0/v2 fetch, shallow negotiation, push, ref updates, tags, and report-status sideband framing.
- GitHub REST v3-style endpoints under `/api/v3` for repositories, branches, commits, contents, labels, issues, comments, pull requests, files, and merges.
- GraphQL v4 at `/api/graphql`, including repository, issue, pull-request, introspection, pagination, mutation, and batched-operation support.
- Model Context Protocol JSON-RPC at `/mcp` with repository, issue, pull-request, listing, and merge tools.
- Deterministic lifecycle integration with the make.fun benchmark harness.

## Quick start

```sh
node --experimental-strip-types main.ts
```

When run by the benchmark harness, `MAKEFUN_ENV_URL` is used to register the ephemeral listening port and to wait for the environment's completion signal. The server never binds the harness port (`8080`).

For a local health check:

```sh
MAKEFUN_SHOP_TOKEN=test MAKEFUN_LOGIN=adaiversen \
node --experimental-strip-types -e "import('./app.ts').then(({createApp}) => { const s=createApp(); s.listen(0, async () => { const p=s.address().port; console.log(await (await fetch('http://127.0.0.1:'+p+'/healthz')).text()); s.close(); }) })"
```

## Project structure

| File | Purpose |
| --- | --- |
| `main.ts` | Harness lifecycle: ephemeral port binding, registration, completion polling |
| `app.ts` | HTTP server, authentication, REST, GraphQL, MCP, and Git smart HTTP |
| `LICENSE` | MIT license |
| `package.json` | Node.js runtime metadata and start script |

## Design notes

The forge stores repositories, refs, issues, pull requests, comments, and Git objects in memory for the lifetime of the process. Git object and pack handling is implemented with Node.js built-ins so the runtime remains self-contained. API calls require `Authorization: Bearer <MAKEFUN_SHOP_TOKEN>`; `/healthz` is intentionally unauthenticated for the harness boot probe.

## Validation

The implementation is validated with the official make.fun `github-core-clone-01` local grader. The latest committed practice run completed successfully and replayed deterministically.

## License

MIT. See [`LICENSE`](LICENSE).

Built for [make.fun](https://make.fun/bounty/github-core-clone-01).
