# Minecraft options.txt Sync

Sync Minecraft options files between instances. Useful with launchers like Prism and MultiMC.

![demo](/demo,png)

> [!IMPORTANT]
> Requires at least bash 4.0. MacOS ships with an ancient bash version, so you'll need to install it yourself.

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
$ ./optsync -g /path/to/global/options.txt -l /path/to/instance/options.txt -s /path/to/instance/sync.txt -m respect
```

Before a sync is performed, LOCAL and SYNC are backed up to a file ending with `.bak`.

## Tips for generating sync.txt
**Compare options in GLOBAL and LOCAL**:
```sh
$ comm -i3 <(sort $GLOBAL) <(sort $LOCAL) | paste - - | column -t
```

**Get all lines from GLOBAL are DIFFERENT than LOCAL**:
```sh
$ comm -i23 <(sort $GLOBAL) <(sort $LOCAL)
```

**Get all options that are the SAME in GLOBAL and LOCAL**:
```sh
$ comm -i12 <(sort $GLOBAL) <(sort $LOCAL)
```

**Get all lines from GLOBAL except those that have been changed in LOCAL**:
```sh
$ comm -i2 <(sort $GLOBAL) <(sort $LOCAL) | awk '{$1=$1};1'
```

## Prism Launcher Pre-Launch Command
To automatically sync settings when you launch the game, add the following script to your Prism Launcher Pre-Launch command (instance settings > settings > Custom Commands > pre-launch):

```sh
/opt/homebrew/bin/bash /path/to/optsync -g "/path/to/global/options.txt" -l "$INST_MC_DIR/options.txt" -s "$INST_MC_DIR/sync.txt" -y sync
```

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
