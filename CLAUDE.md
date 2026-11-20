# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Repository Purpose
Userscripts that help contributors of the mixesdb.com (MixesDB) Wiki.

## Abbreviations
- `MDB`: MixesDB
- `TLE`/ `TLE API`: MixesDB's Tracklist Editor API called in `shared/tracklist_editor/funcs.js` –> `function apiTracklist()`

## Origin of this repo
- Independent repo, NOT a fork of `mixesdb/userscripts` (the official MixesDB userscripts).
  Docs must not present it as the official MixesDB scripts or as a mirror of that repo.
- Ported on 2026-11-18 from `mixesdb/userscripts` (GitHub; local checkout was
  `~/Documents/GitHub/userscripts`). Only hearthis.at, SoundCloud and TrackId.net came over,
  plus everything they load. The other scripts (Mixcloud, YouTube, RA, 1001 Tracklists, the
  Tracklist box scripts, MixesDB Userscripts Helper, Tracklist Cue Switcher, ...) stay there.
- On the port every `@name` lost its ` (by MixesDB)` suffix, `@author` became `Subfader`, and every URL that loads code or CSS
  was moved from `raw.githubusercontent.com/mixesdb/userscripts/` to
  `raw.githubusercontent.com/Subfader/Mixes-userscripts/`. That base is hard-coded in three
  places besides the headers: `githubPath_raw` in `shared/global.js`, and the absolute CSS URLs
  in `shared/tracklist_editor/funcs.js` and `shared/mixesdb_modal/funcs.js`. A repo rename or
  move has to touch all of them, or the scripts silently load old code and CSS.
- Tampermonkey identifies a script without `@namespace` by its `@name`. The rename therefore
  installs these as NEW scripts next to the old "(by MixesDB)" ones; the old copies keep
  updating from the old repo. Uninstall the old ones by hand, or both run on the same page.
- READMEs here still mention scripts that were not ported (e.g. "Runs on" lists in the shared
  READMEs). Those links point to `https://github.com/mixesdb/userscripts/tree/main/...` on
  purpose, so they keep working without the local folders.
- The Page Creator and the TrackId.net merge mode still need the MixesDB Userscripts Helper on
  mixesdb.com. It was not ported; it is installed from the old repo.

## Structure
- `<script-name-with-underscores>/` - one .user.js file per userscript, named `script.user.js`
- `Tracklist_Cue_Switcher/` holds only `script.funcs.js`, which TrackId.net `@require`s. The Cue
  Switcher userscript itself lives in the old repo; keep the two copies in mind when one changes.
- No shared build system; each script must work standalone when copy-pasted into a userscript manager
- Each script can has own script.css files in the same folder
- Each script can `@require` shared code from `/shared/`, e.g. `/shared/global.js`.
- Shared code that is a whole feature rather than a helper gets its own subfolder under
  `/shared/` holding a `funcs.js`, its own CSS and its own README - see `/shared/toolkit/`,
  `/shared/tracklist_editor/`, `/shared/mixesdb_modal/`, `/shared/tracklist_importer/` and
  `/shared/page_creator/`, which also has its own CLAUDE.md
- Loose files directly in `/shared/` are the helpers that are not a feature of their own
  (`global.js`, `global.css`, `youtube_funcs.js`) plus the vendored copies
  (`jquery-3.7.1.min.js`, `waitForKeyElements.js`). `trustedTypes.js` was not ported because
  none of the three scripts loads it.
- If a shared script changes, update the version number in its @require URL of each userscript (and the @version of each userscript)

## Feature Docs
- Every userscript folder has a `README.md` describing its user-facing features, and so do the
  shared ones (`/shared/`, `/shared/toolkit/`, `/shared/tracklist_editor/`,
  `/shared/page_creator/`). GitHub renders a folder's README right
  under its file listing, which is why they are not called FEATURES.md.
- They are the source for https://www.mixesdb.com/w/Help:MixesDB_userscripts, which links to them
  instead of duplicating the text. Write for a MixesDB contributor, not for a developer: what the
  feature does and where it shows up on the page, not which selector it hangs off.
- **Any change that adds, removes or alters user-visible behaviour must update the README.md of
  every affected script** - a change under `/shared/` usually means several of them, plus the
  shared README that owns the feature. Purely internal changes (refactors, logging, selector
  fixes that only restore existing behaviour) need no README.md edit.
- Keep the structure identical across all of them: the intro sentence, the
  `Runs on` / `Install` / `Shared features` list, `## Features` with one `###` per feature, and
  `## Known limitations`. The wiki deep-links to headings, so renaming one silently breaks those
  links - rename only when the feature itself changed.
- **Never use a userscript's name as a `##`/`###` headline** - not in its own README, not in
  another script's, not in a shared feature's. The name here means the title with blanks instead
  of underscores: `SoundCloud`, `TrackId.net`, `hearthis.at` here, and the old repo's names
  (`Mixcloud`, `1001 Tracklists`, `MixesDB Userscripts Helper`, `Tracklist Cue Switcher`, ...)
  without their ` (by MixesDB)` suffix.
  https://www.mixesdb.com/w/Help:MixesDB_userscripts already carries one headline per userscript;
  a second headline of the same name collides with that anchor and breaks every link pointing at
  it. Name the FEATURE instead: `### Toolkit on podcast episodes`, never `### RA`.
  The `#` title at the top of a README is the one exception - that line IS the script's name.
- A feature that lives in `/shared/` is described once there and linked from the site scripts,
  not copied into each of them.
- `Shared features:` lists only the shared features the script ITSELF puts on the page. It is not
  a list of everything the script touches or sits next to:
  - a script that ADDS something to a shared feature (Tracklist Cue Switcher -> the Tracklist
    box) has none - name the surface it is applied to in `Runs on` instead
  - a script that READS a shared feature another script put there has none (the retired
    Tracklist Merger read TrackId.net's tracklist box without listing it)
  - a script that only `@require`s a shared file without rendering its feature (Internet Archive
    requires `toolkit.js` but never calls `getToolkit()`) has none

  Check the code before listing one: the `@require` list is not the answer, the call is.
- Root `README.md` holds the table of all scripts; a new script needs a row there.
  Its `Update` column shows each script's `@version` as the link text, pointing at the raw
  `script.user.js` (same URL as `@updateURL`). **Every `@version` bump must also update that
  number**, or the table advertises an old version.

## Userscript Header Conventions
Every script must start with a complete `==UserScript==` metadata block:
- `@name` - short, descriptive, in English
- `@namespace` - use `https://github.com/<user>/<repo>`
- `@version` - YYYY.MM.DD.N, bump on every functional change. `N` starts at `1` on each new calendar date (not a running total across dates) and increments for further changes that same date - so the first change on a new day is `.1`, not a continuation of the previous day's last `N`. At the end of a task, always tell me which version you bumped to.
- `@description` - one line, English
- `@match` / `@include` - as narrow as possible, never `*://*/*` unless truly required
- `@require` - Update version paramters in other script URLs of they got changes
- `@grant` - list only what's actually used, default to `none` if no GM_* API is needed
- `@author` - `Subfader`, the same in every script (the old repo used `User:Martin@MixesDB (Subfader@GitHub)`)

## Links We Add
- Open in the **same tab by default** - leave the new-tab decision to the user (cmd/ctrl/middle-click). Don't reach for `target="_blank"` just because a link leads off-site.
- Always set an **explicit `target`** on generated links, and default it to `_top`, not `_self`/nothing. Some pages render our HTML inside a nested browsing context (e.g. SoundCloud's `iframe#__WEBI_IFRAME_PRELOADED__` since the ~Aug 2026 redesign), where a target-less link replaces the frame instead of the page: the address bar keeps the old URL, the page can't be bookmarked or shared, and once that frame goes cross-origin the userscript loses access to it. `_top` leaves the frame while staying in the same tab, and is identical to `_self` on unframed pages, so it is always safe.
- `target="_blank"` only where the user is expected to keep working on the source page while the link is open (e.g. the toolkit's EDIT/HIST links).

## Code Style
- Indentation: 4 spaces
- Comments: English, explain *why* not just *what*
- Never write an em dash (U+2014). Use an en dash (`–`) instead - in code comments, in commit
  messages and in every Markdown file. The only em dashes allowed in this repo are the ones that
  are DATA: character classes and separator patterns that have to match an em dash in a scraped
  title (`title_builder.js`, `title_definitions.js`, `tracklist_detector.js`), and the test
  fixtures in `tracklist_examples.js`. Never "fix" those.
  A grep for U+2014 across `*.md` must stay empty.
- Prefer plain JS, but do not change existing jQuery code
- Wrap script body in an IIFE: `(function() { ... })();` right after `// ==/UserScript==`, closed at end of file. No `'use strict'` – too risky to audit every implicit global across this much legacy jQuery code; a silent bug would become a hard `ReferenceError`.
- If the script declares `scriptName`, add `window.scriptName = scriptName;` right after – `shared/toolkit/funcs.js` reads `scriptName` as a plain (non-`typeof`-guarded) global, so it must survive the IIFE.
  `window.cacheVersion = cacheVersion;` goes on the next line for the same reason: shared files
  that load their own CSS (`shared/tracklist_editor/funcs.js`) build the `?v-<script>_<n>`
  cache param from it, so a CSS change ships with the version bump instead of arriving on its own.
- The `Load @ressource files with variables` block (`var cacheVersion = N, scriptName = "...";`
  plus `window.scriptName` and the `loadRawCss()` calls) is the FIRST thing inside the IIFE –
  see TrackId.net. Never let it sink into a constants section further down: `cacheVersion` is
  bumped on most shared-file changes and must be findable without scrolling. Where something
  genuinely has to run even earlier (YouTube's dependency-free startup diagnostics, SoundCloud's
  frame opt-out that no CSS may precede), only the `loadRawCss()` calls stay down there – the
  `cacheVersion`/`scriptName` declarations still go on top.
- Use the log()/logVar()/logFunc() helpers from global.js and a `DEBUG` flag if logging is needed during development

## Single-page apps (all the sites we run on)
A click on these sites swaps the page content and rewrites the address bar without ever
loading a document. `onUrlChange( runPage )` in `shared/global.js` is the shared answer -
it waits for the new DOM, clears the previous page out, then re-runs what was registered.
It replaced `redirectOnUrlChange()`, which forced a full reload on every URL change.

Three rules make it work; breaking any of them produces the same symptom - the first page is
fine and every page after it is stale or empty:
- **Never capture URL-derived state at load time.** `const isSetPage = urlPath(2) == "sets"`
  right after the IIFE's opening brace answers for the first page forever. Make it a function,
  or re-read it in the `onUrlChange` callback.
- **Register `waitForKeyElements` handlers once, at the top level, and test the URL INSIDE the
  handler.** waitForKeyElements keeps one polling interval per selector and that interval holds
  the callback it was created with, so registering the same selector again later does not
  replace it - the old closure keeps winning. Return `true` from a handler that is not
  interested in the current page, so the node stays on the watch list.
  For the same reason, do not pass its `waitOnce` argument for anything that has to work on
  more than the first page.
- **Name what you put on the page so the cleanup can find it:**
  - elements we CREATE: class `mdb-element`, or an id starting with `mdb`/`mixesdb` - these are
    removed on every navigation
  - "already handled" markers on the SITE's nodes: a class containing `processed` and starting
    with `mdb` (e.g. `mdb-processed-toolkit`) - these are cleared on every navigation
  - never a bare `processed`: it collides with the sites' own classes and the cleanup cannot
    tell it apart from theirs

## Testing / Verification
- No automated test suite, with one exception: the MixesDB Page Creator's title suggestion has
  `shared/page_creator/title_examples.js` (reported titles + what they should produce) and a
  deno runner. See `shared/page_creator/CLAUDE.md`. Use deno, not node - node is not
  installed here.
- Manual verification steps:
  1. Load script in Tampermonkey (dev mode / local file URL)
  2. Confirm `@match` triggers only on intended pages
  3. Check browser console for errors on page load

## Common Tasks
- Adding a new script: create `<Script_Name>/script.user.js`, follow header template above, write its `README.md` (see Feature Docs) and add a row to the root README.md table
- Porting another script from the old repo: copy its folder plus every `/shared/` file it `@require`s or `loadRawCss()`es that is not here yet, rewrite `mixesdb/userscripts` to `Subfader/Mixes-userscripts` in its header, drop ` (by MixesDB)` from `@name`, and turn the old-repo README links that now point at it back into relative links
- Updating a script: bump `@version`, note change in a `## Changelog` comment block at bottom of file if non-trivial
- Adding new features to a userscript or shared script: Log debugging diagnostics for crucial steps from the beginning not just when bugs occur
- Renaming/moving a script: update `@namespace`/`@updateURL`/`@downloadURL` if present, since users' managers track updates by these

## Things to Avoid
- **Never send a URL built from a missing value to any API** - no `/undefined/`, `/null`,
  `=NaN`, empty ID. String concatenation turns an unread value into a real-looking URL, and the
  MixesDB search's `insource:` fallback then matches every page whose text holds the word
  (hearthis.at, 2026-11-18: `https://hearthis.at/undefined/` listed 5 unrelated pages as the
  player's). Validate every value read off a page (an ID is digits, a URL has the expected
  shape) BEFORE building a URL from it, and skip the request with a log line when it fails.
  `toolkit_isBrokenUrl()` in `shared/toolkit/funcs.js` guards `getToolkit()` as the net under
  all site scripts; new API callers need the same check.
- Don't bundle multiple unrelated scripts into one file
- Don't use broad `@match` patterns that could break other sites
- Don't add remote `@require`/`@resource` from untrusted or unpinned sources
- Don't commit personal API keys or tokens into script files
- When solving problems on a certain userscript: When you change non-functional things like logging in shared scripts like global.js: Avoid version bumps for other userscripts (it has no effect on them when they still load the old global.js version from cache).

## Discord Summary
- Don't write one unprompted. I decide when a session is done and then type `/discord`, which returns the paste-ready summary of the whole session's fix (no headline). The format rules live in that skill, not here.