# Mixes userscripts

Userscripts for mix related websites. They change the look and behaviour of sites like SoundCloud, hearthis.at and TrackId.net, e.g. by adding copy-paste-ready tracklists in wiki syntax for MixesDB.

To install a script, add a userscript manager like Tampermonkey to your browser, then click
the **Install** link in the script's README.

## The scripts

Each folder holds one userscript and a README describing its features.

| Script | Runs on | What it does |
| --- | --- | --- |
| [SoundCloud](SoundCloud/) | soundcloud.com | Page Creator, tracklist from the description, toolkit, artwork, list filters |
| [TrackId.net](TrackId.net/) | trackid.net, mixesdb.com | Tracklist in wiki syntax, Page Creator, player embed, integration marker, TID links under MixesDB players |
| [hearthis.at](hearthis.at/) | hearthis.at | Page Creator, tracklist from the description, toolkit for both player URL variants |

Shared across the site scripts, in [shared/](shared/): the [toolkit](shared/toolkit/), the
[tracklist box](shared/tracklist_editor/), the [Page Creator](shared/page_creator/), the
[tracklist importer](shared/tracklist_importer/) and the [MixesDB modal](shared/mixesdb_modal/).

[Tracklist_Cue_Switcher/](Tracklist_Cue_Switcher/) only holds `script.funcs.js`, which the
TrackId.net script loads. The Tracklist Cue Switcher userscript itself is not part of this repo.

## Origin

This repo is not a fork of the official MixesDB userscripts in
[mixesdb/userscripts](https://github.com/mixesdb/userscripts) and is developed independently.
It started from three scripts copied from there: "SoundCloud (by MixesDB)",
"TrackId.net (by MixesDB)" and "hearthis.at (by MixesDB)". Your userscript manager treats the
ones from here as different scripts, so if you have the "(by MixesDB)" copies installed,
uninstall them – otherwise both run on the same page.

The Page Creator and the tracklist importer need the
[MixesDB Userscripts Helper](https://github.com/mixesdb/userscripts/tree/main/MixesDB_Userscripts_Helper/)
on mixesdb.com, which is installed from the official repo.

## Issues

There probably won't be any issues listed here. Discussion takes place on Discord:
https://discord.com/channels/1258107262833262603/1261652338314055742
