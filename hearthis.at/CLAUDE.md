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
