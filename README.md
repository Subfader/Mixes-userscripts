# MixesDB userscripts

MixesDB userscripts change the look and behaviour of certain DJ culture related websites to help contributing to MixesDB, e.g. by adding copy-paste-ready tracklists in wiki syntax.

See https://www.mixesdb.com/w/Help:MixesDB_userscripts for installation and general help.

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

## Moved from mixesdb/userscripts

These three scripts used to be "SoundCloud (by MixesDB)", "TrackId.net (by MixesDB)" and
"hearthis.at (by MixesDB)" in [mixesdb/userscripts](https://github.com/mixesdb/userscripts).
Your userscript manager treats the renamed ones as new scripts, so after installing them from
here, uninstall the old "(by MixesDB)" copies – otherwise both run on the same page.

The other scripts (Mixcloud, YouTube, RA, 1001 Tracklists, the tracklist box scripts,
MixesDB Userscripts Helper, Tracklist Cue Switcher, ...) are still in the old repo. The Page
Creator and the tracklist importer need the
[MixesDB Userscripts Helper](https://github.com/mixesdb/userscripts/tree/main/MixesDB_Userscripts_Helper/)
installed from there.

## Issues

There probably won't be any issues listed here. Discussion takes place on Discord:
https://discord.com/channels/1258107262833262603/1261652338314055742
