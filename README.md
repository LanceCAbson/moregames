# More Games — cross-promo catalog

This folder is the **live "More Games" catalog** shown inside all of my apps
(Sleep like a Baby, Sugar Cubes, AstroX, Ding Dong, Mystic, AI Adventure,
Interactive Fiction, Golf Caddie Pro …). Each app fetches `games.json` at
launch via the shared `MoreGamesPanel` (in the `unity-shared` submodule,
`CrossPromo/`). Editing this file changes the line-up in **every app with no
rebuild** — the next time each app launches it picks up the change.

## Where it must live

Drop this folder into the **`lancecabson.github.io`** Pages repo so it serves at:

```
https://lancecabson.github.io/moregames/games.json      <- the catalog
https://lancecabson.github.io/moregames/icons/astrox.png <- icons (optional)
```

That exact `games.json` URL is hard-coded as the default in
`MoreGamesCatalog.RemoteUrl`. If you change the path, update that constant too.

If `games.json` is ever unreachable, each app falls back to the copy bundled in
its build (`CrossPromo/Resources/MoreGamesCatalog.json`), so a 404 here never
breaks anything — it just means the line-up can't be updated remotely.

## File format

```jsonc
{
  "version": 2,                  // informational; bump when you edit
  "games": [
    {
      "id": "sugarcubes",        // stable slug (any unique string)
      "name": "Sugar Cubes",     // title shown on the tile
      "tagline": "Addictive block puzzle",
      "iconUrl": "https://…/512x512.jpg",   // square image, any public URL
      "androidUrl": "https://play.google.com/store/apps/details?id=…",
      "iosUrl": "https://apps.apple.com/app/id…",
      "url": "",                 // optional smartURL; if set, used on BOTH platforms
      "bundleIds": ["com.iphonegamesapps.sugarcubesblockpuzzle"]
    }
  ]
}
```

### Rules the apps apply at runtime
- **Self-exclusion:** an entry is hidden in the app whose own bundle id is in its
  `bundleIds` list (so Sugar Cubes never advertises Sugar Cubes). List every
  platform's id if they differ — e.g. Interactive Fiction is
  `["com.iphonegamesapps.ifinteractive", "com.iphonegamesapps.if"]`.
- **Platform hiding:** an entry with no store link for the running platform is
  hidden. On iOS the app uses `iosUrl`; on Android, `androidUrl`; `url` (a
  per-platform smartURL) overrides both.
- **Icons** can be any public image URL (Apple's CDN artwork is fine and used
  for most entries here). ~512×512 square looks best. Missing/blank icon → a
  coloured initial-letter tile is shown instead.

## Common edits

- **AstroX goes live on iOS:** set its `iosUrl` to
  `https://apps.apple.com/app/id<NEW_ID>` and it appears on iOS everywhere on
  next launch. (Its Android icon is already hosted here at
  `icons/astrox.png`; swap to Apple's artwork URL if you prefer once it's live.)
- **Add a new game:** copy a block, fill in `name`, `iconUrl`, the store URLs,
  and `bundleIds`.
- **Run a promo / reorder:** move blocks around — they display top-to-bottom in
  array order.
- **Pull a game:** delete its block (or blank its store URLs).

Validate before committing: `python3 -c "import json;json.load(open('games.json'))"`
