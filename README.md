# JB (13.60 or earlier)

You need to be on firmware 13.60 or less. Note down the version you're on somewhere, in case it'll be helpful later.

Some specific firmware versions are unsupported by the [Relapse exploit](https://github.com/ntfargo/Relapse-Exploit) which this doc uses, in which case look up which other exploit to use for your version.

Disable automatic update download AND automatic update installation in Settings.  
I repeat, disable automatic updates, do that first.

Edit your Networking settings to be manual, and set DNS server to 45.56.67.85 - this blocks SONY domains and replaces User Guide with jailbreak page, network connectivity test uses SONY domains, so that will fail, which is expected and desired. 

To jailbreak, simply open the User Guide from Settings, and select click the jailbreak button.

Note that the jailbreak can fail to apply, breaking the current boot of the console, requiring you to reboot it - which almost always will require you to attempt to power off via the menu then hold the physical power button until the console beeps to force actual poweroff, since it'll fail to do so.

Note that the jailbreak can catastrophically break the SONY OS to the point where its own recovery will kick in - this means you lose all the data on the console, including the blu-ray pairing for the SONY optical drive if you have one - this means your only avenue to use optical media would be to get a non-SONY optical drive which does not have DRM to be used - or to reverse engineer the needed pairing activation yourself.  
In addition, the recovery system will attempt to download the LATEST FIRMWARE RECOVERY IMAGE - do not let it do that, look up how to manually get the version you're already on, and let it use that instead, else you lose the ability to jailbreak.

The jailbreak, when it succeeds, opens a privileged ELF loader on port 9021 which can accept other payloads - you can drop them via netcat like so: `nc -q0 192.168.100.13 9021 < pkg-manager_v1.4.1.elf`

The same User Guide website also allows you to install payloads over the website, which is arguably more user friendly and you should stick with that, unless you know better.

Drop in payload manager and add this repo to it(see below), then from payload manager install:
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

* Folder - just a directory with eboot.bin and other needed app files - seems to work poorly for some apps, for example it breaks audio for PS5 version of Crash Bandicoot 4 (except the videos)
* ExFAT - .exfat suffixed file that's an exfat file - `mkexfat.sh` in this repo can create them from the Folder format - not compressed (?)
* FPKG - .pkg fancy compressed package in the console's native format - should be best for file size, performance and compatibility - unsupported on 13.60 at the time of writing, can be extracted to Folder format via https://github.com/thanhsondev/PSVIETHOA-FPKG-Builder - which seems heavily vibe coded but works nicely
* FFPKG - TODO

# Project aggregator

This project tries to aggregate all the interesting PS5 projects that you may want to check out -  https://pedrohti.github.io/PS5LinkCentalizer/

# Patches for games

PS5 patches (closed source site I suppose :/) - https://www.prosperopatches.com/PPSA02433

# Running Linux

You can make the SONY OS gut itself and be replaced with a Linux distribution of your choice (do I hear Arch Linux?) for the current boot.

This means you won't suffer any restrictions on what you want to do by the SONY OS, and you can play regular games made for Windows/Linux natively.

To do that, you cannot use the internal storage, you have to use an internal M.2 slot or an external drive over USB.

The tooling is WIP, but it should hopefully be shortly available for firmware version 13.60 and under.

# Custom payload repository

This git repository is also a custom payload repository for PS5 Payload Manager - forked off https://github.com/RDX-Sci01/PLDMGR_JSON

Payload sources are automatically refreshed every 2 hours using GitHub Actions. The generated `payloads.json` contains validated payload download URLs and metadata.

## Adding this repository as a payload source

Open the Payload Manager dashboard on your PS5:

1. Go to **Settings → Manage Sources**
2. Click **Add Source**
3. Paste the following URL: `https://raw.githubusercontent.com/C0rn3j/PLDMGR_JSON/main/payloads.json`

## Making your own payload repository fork

Simply replace the username in the example links with the one used in your own fork.

You will also need to enable GitHub Actions in the Settings of the fork otherwise the plugins won't be generated/updated.

# Backing up your own games

TODO, look it up.

To back up games from PSN, you need to have them installed and activated already, meaning that you're going to have a hard time dumping new releases on a jailbreakable console, as there is no known way of getting them installed, since PSN will throw you off on a firmware that's too old.

# Downloading games that aren't your own

Unless you're talking about free(ware) games, figure it out, this doc is not meant to enable piracy.

You can edit the payload list by editing [`links.txt`](links.txt).
