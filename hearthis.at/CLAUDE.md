# hearthis.at – working notes

## Page markup (since the redesign seen 2026-11-18)
- hearthis.at switched to Tailwind utility classes. The old semantic hooks are gone, most
  importantly `section.track-detail-header`, which the toolkit used to be placed after. Losing it
  made the toolkit (and the Page Creator row, which waits for the toolkit) silently vanish: the
  only trace was `toolkitTarget.length: 0` in the console.
- The track header is now `section` with the Tailwind group name `group/hero` in its class list,
  holding a `ul` of `li` items with `group/track`. Match both as class SUBSTRINGS
  (`[class*="group/hero"]`): the slash would need escaping in a class selector, and the other
  utility classes change with every styling tweak.
- Profile pages have a `group/hero` too (artist banner) with no `group/track` inside. That is why
  `getHearthisToolkitTarget()` requires exactly one track item in the hero.
- The `<meta>` tags the script reads (`og:url`, `og:title`, `hearthis:embed:id`) survived the
  redesign. `og:type` is `website` on every page, so it cannot tell track pages apart.
- If the toolkit disappears again, check `toolkitTarget.length` in the console log first.

## Light and dark theme
- hearthis.at has its own theme setting: `data-color-scheme` on `<html>` (`system` by default,
  or light/dark set by the reader). It is NOT tied to the OS: a dark Mac can show the light
  site. A `prefers-color-scheme` media query therefore cannot tell which theme the toolkit sits
  in - that is why the old "OS lightmode fixes" block in `shared/global.css` was removed.
- The track page's backdrop is a dark, artwork-tinted gradient in both themes; only the cards
  (hero, "Mehr von ...", sidebar) and the text colour change. So the toolkit stays a dark box
  (`#222`) and sets its OWN text colour (`.mdb-toolkit.hearthis-at` in `shared/global.css`).
  Never let it inherit `color` from the site: with the light theme that is near-black, and
  every label in the box (everything that is not a link) disappears.
- `.content-layer-content` still exists and wraps the whole track page. Do not restyle it -
  it also holds hearthis' own white cards.
- The toolkit and the Page Creator row have no side margin: they line up with the hero card
  and the cards below it. The old 20px inset matched the old header's padding.
- Testing a theme without changing the site setting: set `color` on `.content-layer-content`
  by hand in the console - that is what the toolkit would inherit.

## Images
- hearthis.at's stylesheet sets `img { font-size: 0 }` (hides alt text). Any `em` size on one of
  our images is relative to the IMAGE's own font size, so it becomes 0px there - the TrackId.net
  icon rendered as a 2x0 box. `shared/global.css` gives `img.tidSubmit-icon` and
  `img.mdb-domainIcon` `font-size: inherit`. A new em-sized icon class needs to go into that
  rule too, or be sized in px.

## Reading the track: hero item, not the metas
- hearthis.at does NOT update its `<meta>` tags on an in-site click. After profile -> track,
  `hearthis:embed:id` is gone and `og:url` / `og:title` still describe the profile. Only a fresh
  document load has them right, which is why the old code worked when tested by reload.
- `getHearthisHeroTrack()` reads the header's `group/track` li instead, which is re-rendered
  with the page: `data-trackid` (numeric ID), `data-detail` (`/<user>/<permalink>/`),
  `data-playlist-title`. The metas are only the fallback for the old layout.
- `runHearthisPage()` validates both before anything is requested: ID digits only, URL
  `/<user>/<permalink>/` or the numeric short form. See the root CLAUDE.md rule on missing
  values in URLs.
- To test SPA behaviour, open a profile and click a track - never only reload the track page.
