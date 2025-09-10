# Starbound Mod Manager (SteamCMD Wrapper)

A shell script to **download, install, clean, and back up** Starbound mods from the Steam Workshop using `steamcmd`.

Instead of hard-coding Workshop IDs into the script, it uses a simple database file (`starbound_workshop_ids.txt`) where you can add/remove mods and optionally assign names.
Mods are installed into Starbound’s `mods/` directory with clean filenames (`Name.pak` or `ID.pak` if no name is given).

---

## Features

* **Download & update** Starbound + mods via `steamcmd`.
* **Named mods** — specify a friendly name in the ID file; otherwise the Workshop ID is used.
* **Automatic index** — generates `mods_index.tsv` mapping IDs → filenames.
* **Clean mode** — removes `.pak` files in `mods/` that are not listed in your ID file.
* **Backup & restore** — tarball your `mods/` directory and restore later.
* **Force mode** — delete existing Starbound and Workshop dirs before reinstall.
* **Cosmetics** — removes the default `mods_go_here` placeholder file.
* **Colored output** — errors in red, successes in green, info in cyan, warnings in yellow.

---

## Requirements

* Linux or WSL
* `steamcmd` installed and available in `$PATH`
* `awk`, `tar`, `find` (usually already installed)

---

## Installation

1. Save the script (e.g. as `starbound_mods.sh`).

2. Make it executable:

   ```sh
   chmod +x starbound_mods.sh
   ```

3. Run it:

   ```sh
   ./starbound_mods.sh
   ```

The first run will create `~/starbound_workshop_ids.txt` with some starter IDs.

---

## Usage

### Basic run

```sh
./starbound_mods.sh
```

Prompts for your Steam username and password, then downloads Starbound + all mods listed in `starbound_workshop_ids.txt`.

---

### Flags

* `--force`
  Delete Starbound and Workshop directories before downloading fresh.

* `--clean`
  Remove `.pak` files in `mods/` not listed in your ID file.

* `--backup <file.tgz>`
  Create a tar.gz archive of your current `mods/` folder.

* `--restore <file.tgz>`
  Restore a `mods/` folder from a tar.gz archive (skips downloading).

* `--help`
  Show usage info.

---

### Mod database format

`~/starbound_workshop_ids.txt`

```
# Workshop IDs for Starbound (211820)
# Optional name after ID, otherwise the .pak will be named by its ID
729480149 FrackinUniverse
729492703
1264107917 BetterGraphics
```

* First column = Workshop ID
* Second column (optional) = Friendly name (used as `.pak` filename)
* Lines starting with `#` or blank lines are ignored

Example results in `mods/`:

```
FrackinUniverse.pak
729492703.pak
BetterGraphics.pak
```

---

### Index file

After each run, an index file is generated:

`Starbound/mods/mods_index.tsv`

```
seq   workshop_id   name              installed_path
1     729480149     FrackinUniverse   /.../Starbound/mods/FrackinUniverse.pak
2     729492703     729492703         /.../Starbound/mods/729492703.pak
3     1264107917    BetterGraphics    /.../Starbound/mods/BetterGraphics.pak
```

---

## Examples

Force refresh + clean up unused mods:

```sh
./starbound_mods.sh --force --clean
```

Backup current mods:

```sh
./starbound_mods.sh --backup ~/starbound-mods-$(date +%F).tgz
```

Restore from backup:

```sh
./starbound_mods.sh --restore ~/starbound-mods-2025-09-10.tgz
```

---

## Notes

* Your Steam credentials are **only passed to `steamcmd` locally**; they are not stored by the script.
* If you use Steam Guard, you may be prompted for the code when logging in.
* `mods_go_here` (the useless placeholder) is automatically removed.
* This script is meant for **server installs or personal setups** — you still need to own Starbound on Steam.

---

## License

MIT — do whatever, just don’t blame me if you nuke your mods with `--force`.
