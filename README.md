# doki Core — releases

Public catalog and jar assets for **doki Core**, the in-game client behind
*doki's Collection* in the doki Client launcher. Source is private and lives
elsewhere.

This repo is public because the launcher reads it with **no credentials**.

## How the launcher reads this

* **Catalog** — `catalog.json` on `main`, fetched over `raw.githubusercontent.com`.
* **Jars** — GitHub *Release* assets, linked by absolute URL from `catalog.json`.

Nothing here is read through `api.github.com`, so the launcher never spends the
60-request/hour unauthenticated API budget.

## catalog.json

A *family* is a Minecraft line rendered as one card; its *builds* are the point
versions offered in that card's dropdown.

```jsonc
{
  "version": 1,                     // schema version; the launcher refuses anything else
  "families": [{
    "id": "1.21",                   // stable key
    "label": "doki 1.21",         // card title
    "art": null,                    // cover image URL, or null for the generated cover
    "blurb": "…",
    "builds": [{
      "mc": "1.21.8",               // Minecraft version
      "loader": "fabric",
      "loaderVersion": null,        // null = latest stable Fabric loader
      "jarUrl": null,               // null = listed but NOT installable ("Coming soon")
      "sha1": null,                 // verified on download when present
      "sizeBytes": null,
      "extraJars": []               // companions dropped into mods/ alongside doki Core
    }]
  }]
}
```

`jarUrl: null` is the honest default: the version shows in the launcher, is
clearly marked *Coming soon*, and refuses to install rather than failing halfway.

## Publishing a build

1. Build the jar from the source repo.
2. Attach it to a GitHub Release here.
3. Point that build's `jarUrl` at the asset and fill in `sha1` / `sizeBytes`.

Adding a version — or a whole new family — is an edit to `catalog.json`. It
never requires a launcher release.

## Versions

doki Core targets **Fabric on 1.21.1 → 26.2**. Legacy lines (1.8, 1.12, 1.16,
1.17) are not a rename away — they are a port, and are not offered here.
