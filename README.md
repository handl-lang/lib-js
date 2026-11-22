# lib-js

JavaScript interop surface — `JS` grade, typed call/invoke/await arrows for sdkgen-generated adapters.

## Layout

```text
src/        the .hnl sources
specs/      committed WebIDL specs (dom.idl, html.idl — w3c/webref pinned)
scripts/    hnl-gate.sh — regenerates handl-packages/generated/ via
            `hnl sdkgen webidl`, then typecheck/lint/test
pkg.hnl     package manifest — modules, deps, description, license
```

DOM/HTML bindings are generated into `handl-packages/generated/`
(`dom-webidl-browser/`, `html-webidl-browser/`) — gitignored build
artifacts consumed via dep/path imports, same pattern as `lib-fetch-js`.

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.
