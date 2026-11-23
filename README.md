# lib-js

JavaScript interop surface — `JS` grade, typed call/invoke/await arrows for codegen-generated adapters.

## Layout

```text
src/        the .hnl sources
specs/      committed WebIDL specs (dom.idl, html.idl — w3c/webref pinned)
scripts/    hnl-gate.sh — regenerates handl-packages/generated/ via
            `hnl codegen webidl`, then typecheck/lint/test
pkg.hnl     package manifest — modules, deps, description, license
```

DOM/HTML bindings are generated into `handl-packages/generated/`
(`dom-webidl-browser/`, `html-webidl-browser/`) — gitignored build
artifacts consumed via dep/path imports, same pattern as `lib-fetch-js`.

## Modules

- `js` (`src/index.hnl`) — the `JS`-grade contract: `call`, `invoke`,
  `construct`, `on`/`off` (the JS→handl event trampoline), `error`,
  `await`.
- `worker` (`src/worker/index.hnl`) — Web Worker bindings over those
  ops: `spawn`/`spawn_code`, `post_message`/`terminate`,
  `on_message`/`on_message_self`, `self_post_message`/`self_close`.
  Import as `from "js/worker" import …` under the usual
  `js = { path = "…/lib-js/src" }` dep (or a `worker` path dep); it is
  intentionally not in `pkg.hnl` `modules` — see the comment there for
  why (the N-module layout would relocate `js` and break path deps).
  The codegen WebIDL pipeline now also emits raw Worker bindings
  (`handl-packages/generated/html*/Worker*`, `DedicatedWorkerGlobalScope*`,
  `SharedWorker*`, `MessagePort*`, `BroadcastChannel*`) using
  `js.construct`/`js.invoke`/`js.on` with `Result<_, JsError>` returns —
  broader WebIDL coverage but a different API shape, so `js/worker`
  remains the ergonomic surface (see the module header for the exact
  gap list).

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.
