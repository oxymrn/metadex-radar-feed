# metaDEX Radar — public feed

`public.json` is a machine-readable snapshot of measured metaDEX vote-escrow
inventory: locked supply and its USD value, permanent (sale-only) share, position
counts, average position size, and venue volume/fee figures, per protocol.

**Generated, not authored.** A workflow in the producing repository writes this
file and pushes it here. Nothing in this repo is edited by hand, and a pull
request against `public.json` cannot be merged — the next refresh would overwrite
it. Corrections belong upstream.

## Reading it

- `https://raw.githubusercontent.com/oxymrn/metadex-radar-feed/main/public.json`
- `schema` is `metadex-radar/public/N`. Check the prefix before trusting fields:
  a consumer that renders a renamed or reordered schema puts wrong numbers on a
  page, which is worse than rendering nothing.
- `generated_at` / `as_of_date` are the measurement time. Render the age rather
  than implying the figures are live.
- `formulas` states how each derived field is computed, so a reader can check the
  arithmetic instead of taking it on trust.
- `notice` states what is refreshed weekly and what moves continuously. Structural
  metrics (locked supply, permanent share, remaining term, position counts) change
  slowly; token prices do not, and are only as of `generated_at`.
- `validation` carries the producing run's own warnings. A field can be absent
  because it was not measurable — absence is a normal state, not an error, and
  should read as "unknown" rather than zero.

## Cadence

Thursdays, 02:00 UTC. Solidly-lineage epochs turn over at Thursday 00:00 UTC, so a
Thursday sample covers exactly the epoch that just closed; the two-hour delay lets
the upstream day-bucket finalise. `fees_window.epoch_aligned` reports whether a
given snapshot actually landed on that boundary, so an off-schedule run labels
itself honestly.

## What is deliberately not here

This feed carries measurements only. The producing repository's judgment layer —
tiering, dependency and custody assessments, wrapper and incumbent-venue notes —
is withheld, and the publishing job refuses to push a file containing any of those
fields. That refusal is asserted at the boundary on purpose: a mistake here would
be public and irreversible.
