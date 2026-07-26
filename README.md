# Kharvalen — public manifest

This repo hosts one file: **`version.json`**, read by the game at startup to tell
players when a newer build is available.

While Kharvalen is distributed through Discord (not yet on Steam), the game has no
auto-update. On the main menu it fetches this manifest and, if the running build is
older than `latest`, shows a notice with the Discord link so the player can grab the
new build.

## `version.json`

```json
{
  "latest": "0.6.170",
  "min": "0.0.0",
  "url": "https://discord.gg/du6PZekvnz",
  "notes": ""
}
```

| Field | Meaning |
|---|---|
| `latest` | Newest published build. A player on an older version gets a dismissible notice. |
| `min` | Oldest build still allowed to play. A player below it gets a notice with **no** "continue" button. Keep it at `0.0.0` unless you really need to cut off old builds. |
| `url` | Where to download. Lives here — not baked into the game — so the Discord invite can change without shipping a new build. |
| `notes` | Optional one-line changelog shown under the message. Leave empty to hide it. Not localized: whatever you write is shown as-is in every language. |

Versions are `major.minor.patch` and must match Unity's `bundleVersion` in
`ProjectSettings.asset`.

## Releasing a new build

1. Bump `bundleVersion` in the game and build it.
2. Upload the build to the Discord `beta-test` channel.
3. Edit `latest` here to the version you just published.

That last step is what makes everyone else's game start telling them to update.
The game caches nothing, but the raw.githubusercontent CDN serves the file with a
short TTL, so the change reaches players within a few minutes.

## Failure behaviour

The check is **fail-open** by design: no internet, GitHub down, malformed JSON or a
slow response all result in *no notice at all* and a normal launch. It can never
block someone from playing because of a network problem.
