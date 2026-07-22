# DockyardManifest

The Dockyard catalog: `manifest.json` is served raw from this repo and consumed by the Dockyard app at runtime.

## Files

- `dockyard.config.json` — the authoring config: the list of app repos in the catalog (plus optional per-app overrides). This is the file you edit.
- `manifest.json` — **generated**; never edit by hand. Built by `dockyard-manifest-tool` from the Dockyard repo (`Packages/DockyardManifestTool`).
- `editorial.json` — editorial content for the store front.

## How releases reach the catalog

The scheduled workflow (`.github/workflows/update-manifest.yml`) rebuilds `manifest.json` daily and pushes it when something changed — cutting a GitHub release in an app repo is all it takes. Trigger it manually from the Actions tab for an immediate update, or publish locally:

```
cd <Dockyard repo>/Packages/DockyardManifestTool
./publish.sh --dry-run   # preview
./publish.sh             # build + commit + push
```

## Adding an app

```
cd <Dockyard repo>/Packages/DockyardManifestTool
swift run dockyard-manifest-tool add owner/repo --config <this repo>/dockyard.config.json
```

The app repo should carry its own metadata in `.dockyard/` (dockyard.json, AppIcon.png, about.md, screenshots/) — the `add` command scaffolds `dockyard.json` for you when it's missing. See the tool's README for the full format.
