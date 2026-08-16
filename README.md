# Zombie Foundry — plugin catalogue

The list of plugins [Zombie Foundry](https://github.com/SmallSoftwareHouse/zombie-foundry)
offers for installation, one file per game.

Installed copies of the tool read these files directly, so **editing a file here
reaches every user without publishing a new version of the tool**. That is the
point of keeping the catalogue outside the program: plugin sources move, forks
go quiet and download links change on their own schedule, not on the tool's
release schedule.

## What this is not

The catalogue holds **links only**. No plugin is hosted, mirrored or
redistributed here: the tool downloads each file from the author's own source at
install time, exactly as a browser would.

## Files

| file | game |
|---|---|
| `plugins-l4d2.json` | Left 4 Dead 2 |

## Format

```jsonc
{
  "Plugins": [
    {
      "Id": "l4dtoolz",                    // stable identifier, never reused
      "Name": "L4DToolZ",
      "Desc": {                            // one line, for the list column:
        "it": "…", "en": "…"               // text per language, NOT an i18n key —
      },                                   // a tool already installed has no key
                                           // for a plugin added later
      "Long": {                            // optional: the detail box under the
        "it": "…", "en": "…"               // list, where there is room to say
      },                                   // what the plugin actually does
      "Hint": {                            // optional: OUR line, kept apart from
        "it": "…", "en": "…"               // the author's — what we think of it
      },                                   // for a server like this one
      "Version": "2.2.0",                  // optional, declared by hand: update
                                           // it whenever the source is updated
      "Author": "Accelerator74",
      "Page": "https://…",                 // where the user can read about it
      "Kind": "metamod",                   // "sourcemod" (default) or "metamod"
      "NeedsConfig": true,                 // needs cvars set after installing
      "Steps": [                           // applied in order; first failure stops
        {
          "Type": "github_release",        // github_release | url | asset
          "Repo": "accelerator74/l4dtoolz",
          "AssetMatch": { "windows": "l4d2-win", "linux": "l4d2-linux" },
          "Extract": "game"                // unpack into the game folder
        }
      ]
    }
  ]
}
```

`en` is the fallback language: an entry without the user's language shows
English, so a new plugin is never blank for anyone. `Long` falls back to `Desc`
when missing — an entry written before that field existed still describes
itself. `Hint` does not fall back beyond English: advice in a language the
reader does not speak is not advice they can act on, and showing nothing is
honest where guessing is not.

`Version` is the one field here that can go stale: it is written by hand, so a
plugin whose author publishes a new release keeps showing the old number until
this file is updated. The tool always downloads whatever the source currently
serves, never the version named here.

## Editing

Every change is checked by [a workflow](.github/workflows/validate.yml) that
refuses invalid JSON, duplicate ids and unknown step types. This matters because
a broken catalogue **fails quietly**: the tool falls back to its cached or
built-in copy, so the update simply never arrives and nothing looks wrong.
