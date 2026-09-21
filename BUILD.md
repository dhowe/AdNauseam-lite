
## Building

### Release build (GitHub Actions)

Releases are produced by the [_AdNauseam MV3 release_](.github/workflows/create_release.yml) workflow, which can be triggered manually:

```sh
git submodule update --remote AdNauseam
git add AdNauseam CHANGELOG.md   # update CHANGELOG.md with the release notes
git commit -m "point at <commit>"
git push
```

The release notes come from the top of `CHANGELOG.md`, down to the first `----------` separator.

Then trigger the build, either from the GitHub UI — _Actions_ → _AdNauseam MV3 release_ → _Run workflow_ → pick the branch → _Run workflow_ — or with the [`gh`](https://cli.github.com/) CLI:

```sh
gh workflow run create_release.yml --repo dhowe/AdNauseam-lite --ref main
gh run watch --repo dhowe/AdNauseam-lite
```

The workflow picks a time-based version (`YYYY.MMDD.HHMM`), builds the Chromium package, and publishes it as a **pre-release** with `AdnauseamLite_<version>.chromium.zip` attached.

### Local build

From a checkout of the [AdNauseam](https://github.com/mneunomne/AdNauseam) repository (the `AdNauseam/` submodule directory works too). Requires `bash`, `node`/`npm`, `jq` and `zip`.

Build the CodeMirror bundle once (it is not committed):

```sh
cd platform/mv3/extension/lib/codemirror/codemirror-ubol
npm ci && npm run build
```

Then, from the repository root:

```sh
tools/make-mv3.sh chromium              # unpacked, for development
tools/make-mv3.sh chromium 2026.920.2003  # + publishable zip, as the workflow does
```

The unpacked extension lands in `dist/build/ADNLite.chromium/` — load it with _Load unpacked_ in `chrome://extensions` (_Developer mode_ enabled). With a version argument the packaged zip is written to `dist/build/`.

Other platforms (`firefox`, `edge`, `safari`) are accepted by the script but are not currently released from this repository.