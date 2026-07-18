# BeerDeer website source

Jekyll site. Source lives here; the built site is mirrored into a separate
GitHub Pages repo at `../beerdeerab.github.io`.

## Deploy

**The one command that does everything** — refresh product catalogue, build,
commit/push both repos:

```powershell
.\_scripts\build-site.ps1 -Deploy
```

Useful variants:

| Command | What it does |
| --- | --- |
| `.\_scripts\build-site.ps1 -Deploy -DryRun` | Shows what would be committed/deleted, writes nothing |
| `.\_scripts\build-site.ps1 -Deploy -SkipProducts` | Deploy without re-scraping Systembolaget (fast) |
| `.\_scripts\build-site.ps1` | Build only, no publish |
| `.\_scripts\build-site.ps1 -Serve` | Local preview (blocks; can't combine with `-Deploy`) |

Deploy steps, in order: refresh `_data/products.json` → `bundle exec jekyll
build` → commit+push `website_source` → mirror `_site/*` into the deploy repo
(deleting everything except `.git` and `CNAME`) → commit+push that repo.
Safety: it refuses to mirror if the target has no `.git` or no `CNAME`.

## Product catalogue

`_scripts/build-products.ps1` writes `_data/products.json`. It walks
Systembolaget's beer sitemap, keeps only URLs matching a hand-maintained
`$BreweryTokens` list, fetches each page, and keeps products where
`supplierName -eq "BeerDeer AB"` and none of the discontinued /
out-of-stock flags are set.

- **It is slow by design** — 3–6s sleep between fetches to avoid throttling.
  Expect a long run. Don't "optimise" the delays away.
- **New brewery not showing up?** Add its URL slug token to `$BreweryTokens`.
- **It aborts rather than write bad data** if the product count drops below
  75% of the previous run, or if too many fetches fail (both signal
  Systembolaget throttling). Re-run later; use `-Force` only when you're
  certain the drop is real (e.g. products genuinely delisted).
- Use `Invoke-WebRequest .Content`, never `Invoke-RestMethod` — the latter
  auto-parses pages into an XmlDocument and silently breaks field extraction.
