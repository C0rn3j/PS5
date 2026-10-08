# Project aggregator

https://pedrohti.github.io/PS5LinkCentalizer/

# Patches for games

PS5 patches (closed source site I suppose :/) - https://www.prosperopatches.com/PPSA02433

# JB (13.60 or earlier)

Disable automatic update download AND automatic update installation in Settings.  
I repeat, disable automatic updates, do that first.

Set DNS server to 45.56.67.85 - this blocks SONY domains and replaces User Guide with jailbreak page.

Jailbreak for 13.60 - https://github.com/ntfargo/Relapse-Exploit - opens privileged ELF loader on port 9021 which can accept other payloads - you can drop them like so: `nc -q0 192.168.100.13 9021 < pkg-manager_v1.4.1.elf`

Drop in payload manager and add this repo to it, then from payload manager install:
  * kstuff-lite - let's you fake license checks (?) - add to autoloader as first
  * ShadowMountPlus - lets you mount apps from USB or internal storage - add to autoloader as second
    * SMP sometimes breaks scanning new files (presumably because it finds a partial one and doesn't rescan the full one later?) - restarting it in Payload Manager fixes it
  * PKG Manager - hosts a web server you can stream .pkg files to over network
  * pegasus-dl - effectively an app store
  * ftpsrv-drakmor - FTP server, manage files
  * klogsrv - let's you read logs over netcat - i.e. `nc 192.168.100.13 3232`
  * WebKit Autoloader Installer - offline way to fancy jailbreak but it seems even less stable than the normal User Guide way
  * PS5SX2 - PS2 emulator, its Helper ELF needs to be autoloaded for it to work, Installer ELF for updates and initial install

# Game formats

Folder - just a directory with eboot.bin and other needed app files - seems to work poorly for some apps, for example it breaks audio for PS5 version of Crash Bandicoot 4 (except the videos)

ExFAT - .exfat suffixed file that's an exfat file - `mkexfat.sh` in this repo can create them from the Folder format - not compressed (?)

FPKG - Fancy compressed package - good for file size - unsupported on 13.60 at the time of writing, can be extracted to Folder format via https://github.com/thanhsondev/PSVIETHOA-FPKG-Builder - which seems heavily vibe coded but works nicely

# PLDMGR_JSON

Custom payload repository for PS5 Payload Manager - forked off https://github.com/RDX-Sci01/PLDMGR_JSON

Payload sources are automatically refreshed every 2 hours using GitHub Actions. The generated `payloads.json` contains validated payload download URLs and metadata.

## Adding this source

Open the Payload Manager dashboard on your PS5:

1. Go to **Settings → Manage Sources**
2. Click **Add Source**
3. Paste **one** of the following URLs:

### GitHub Raw

```text
https://raw.githubusercontent.com/C0rn3j/PLDMGR_JSON/main/payloads.json
```

## Making your own fork

Simply replace the username in the example links with the one used in your own fork.

You will also need to enable GitHub Actions in the Settings of the fork otherwise the plugins won't be generated/updated.

You can edit the payload list by editing [`links.txt`](links.txt).
