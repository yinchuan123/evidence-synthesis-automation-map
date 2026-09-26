# Contributing

Thanks for helping keep this map accurate. Two things get contributed here: **tool rows** in
`AUTOMATION_MAP.md`, and **corrections** to anything already written.

## The evidence rule

Every row in the map carries a source and a check date. That is the whole quality bar.

- **Source** means the tool's own page — its website, repository, CRAN page, documentation, or the
  developers' own paper. A blog post, a listicle, a search-result snippet or a vendor's marketing
  email is not a source for what a tool does.
- **Check date** means the date *you* opened that page and saw the thing you are claiming. Write it
  as `checked YYYY-MM`. Prices and feature lists go stale fast, so an undated claim cannot go in.
- **If you could not verify it, say so.** Write `unverified / 未核实` rather than guessing. A row
  that honestly says "the pricing page was behind a login on 2026-09-20" is more useful than a
  confident number nobody checked.
- **Say what you could not reach.** If a page returned 403, needed an institutional login, or
  showed a bot check, note that. An absence that is written down is evidence; an absence that is
  silent looks like nothing.

## Proposing a tool addition

One tool per issue or pull request. Use the
[Add or fix a tool](../../issues/new?template=add-or-fix-a-tool.yml) issue form, or open a pull
request adding a single row with these fields:

| Field | What to put |
|---|---|
| Tool name | As the tool calls itself. |
| Link | The tool's own page. |
| Step of the map | Which workflow step it serves. Pick one; if it spans two, pick the main one and say so. |
| Cost | Free, free tier plus paid tiers, or paid — with the actual figure and currency you saw. |
| What it does | One sentence, in the tool's own terms, not an endorsement. |
| What is left to the human | The specific thing the tool does not do. This column is why the map exists. |
| Verified | What you opened and when. |

Please do not submit a row for a tool that has a landing page but no working product. Note it in
the issue instead, and say what you tried.

## Proposing a correction

Open an issue with the same form and describe what is wrong, what it should say, and what you
opened to establish that. Corrections to prices, to "left to the human" columns, and to the
benchmark's scoring are all in scope.

**Corrections about your own tool are welcome and are not treated as a conflict of interest.** If
you built a tool listed here and a row is out of date, wrong, or unfair, say so and point at the
page that shows it. Please mention that it is your tool, so the row can note who reported the
change. We will not remove an accurate limitation on request, but we will correct an inaccurate
one, and we will happily record a limitation that has since been fixed.

## What does not belong here

- Claims about a named paper, author, journal article or trial having data problems, impossible
  numbers, registration discrepancies or duplicate publication. This repository does not host
  allegations about specific work. Published retractions, corrections and expressions of concern
  are already public and may be cited neutrally as illustrations; nothing beyond that.
- **The benchmark's task texts and answer keys, as written.** They quote real articles and name
  which of them carry a numeric or registration discrepancy, which is exactly what the rule above
  forbids. Nothing is published under `benchmark/` until article identity has been removed from it.
  `.gitignore` carries patterns for the shapes these working files arrive in, as a backstop; the
  real check is reading the diff. If you want to verify a score, ask for the material by email
  instead.
- Benchmark results that cannot be reproduced from the task, the answer key and the scoring rule
  as published.
- Rankings, scores or "best tool" verdicts. The map reports what each tool does, and readers judge.

## Style

Plain, short sentences. Say the limitation out loud. No "first", "only" or "revolutionary". English
and Simplified Chinese content should say the same things; if you can only write one language,
submit it anyway and say which, and it will be paired later.

## Licence for contributions

By contributing you agree that your contribution is published under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), the same licence as the rest of the
repository.
