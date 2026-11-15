# GitHub-Compatible Git Forge

A standalone, dependency-free GitHub-compatible forge for Node.js 22. It runs as one process with an in-memory store and exposes:

- Git smart HTTP endpoints for repository refs, upload-pack, and receive-pack
- GitHub REST v3-style APIs under `/api/v3`
- GraphQL at `/api/graphql`
- Model Context Protocol JSON-RPC at `/mcp`

This implementation is designed for the `github-core-clone-01` make.fun benchmark and uses only Node.js built-ins at runtime.

[![make.fun bounty](https://make.fun/badge/github-core-clone-01.svg)](https://make.fun/bounty/github-core-clone-01)
[![rank](https://make.fun/badge/github-core-clone-01/Belloblaze-web3/forge-github-core-clone-01/rank.svg)](https://make.fun/bounty/github-core-clone-01)
[![reward](https://make.fun/badge/github-core-clone-01/Belloblaze-web3/forge-github-core-clone-01/reward.svg)](https://make.fun/bounty/github-core-clone-01)

## Run

```sh
node --experimental-strip-types main.ts
```

The process expects `MAKEFUN_ENV_URL` when run under the benchmark harness. It binds an ephemeral port, registers with the environment, serves requests, and exits after the environment reports completion.

For a local HTTP smoke test:

```sh
node --experimental-strip-types -e "import('./app.ts').then(({createApp}) => { const s=createApp(); s.listen(0, () => console.log(s.address())) })"
```

## Repository layout

- `main.ts` — benchmark lifecycle and ephemeral-port registration
- `app.ts` — forge HTTP server, REST, GraphQL, MCP, and smart HTTP implementation
- `LICENSE` — MIT license

## Notes

The server is intentionally self-contained and does not invoke the `git` executable or require a package installation. Data lives in the process store for the duration of a run.

Built for [make.fun](https://make.fun/bounty/github-core-clone-01).
