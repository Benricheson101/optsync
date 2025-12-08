# Minecraft options.txt Sync

Sync Minecraft options files between instances. Useful with launchers like Prism and MultiMC.

## How it works
### Files:
**GLOBAL**: global options.txt file that should be mirrored to other instances
**LOCAL**: a Minecraft instance's options.txt file
**SYNC**: a file containing settings to be synced, as well as their most recent synced value

### Modes:
**respect**: only sync options that have not been changed in LOCAL. this means changing a synced setting locally will not be overwritten
**overwrite**: overwrite all options denoted in SYNC regardless of whether they've been changed locally or not

When a setting is changed in GLOBAL, it is propagated to LOCAL if
- respect: LOCAL == SYNCED and SYNCED != GLOBAL
- overwrite: always

## Usage
Create a sync file associated with your Minecraft instance (e.g. sync.txt). Copy the option lines __from the instance's `options.txt`__ that you would like to sync into the sync file.

To sync:
```sh
./optsync -g /path/to/global/options.txt -l /path/to/instance/options.txt -s /path/to/instance/sync.txt -m respect
```

Before a sync is performed, LOCAL and SYNC are backed up to a file ending with `.bak`.

---

```sh
Minecraft options.txt sync

Usage: optsync [options] <sync|help>

Options:
    -g <file>                 global options.txt file ()
    -l <file>                 local options.txt file ()
    -s <file>                 sync file ()
    -m <respect|overwrite>    sync mode (respect)
    -y                        bypass confirmation (0)
    -n                        dry run (0)
```
