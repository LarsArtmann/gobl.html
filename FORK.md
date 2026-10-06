# Fork of invopop/gobl.html — v0.124.0 + gobl v0.507 compatibility

Private LarsArtmann fork. Upstream tag: `v0.124.0` (commit 6e48718).
Exists because upstream has not released a gobl.html version compatible
with gobl v0.507.0 (no gobl.html release imports the externally moved
`pl-favat-v3` addon, while gobl.ksef/gobl.ubl releases >= v0.45/v0.80
hard-require gobl v0.507.0). Tracked upstream: PR invopop/gobl.html#156
carries the same migration but pins unreleased gobl main.

## Deltas vs upstream v0.124.0 (18 files)

- `components/regimes/pl/{ksef,title}{,.templ,_templ.go}`:
  `github.com/invopop/gobl/addons/pl/favat` (removed in gobl v0.507.0)
  -> `favat "github.com/invopop/gobl.pl.ksef/addon"` (identical package
  name and identifiers). Ported from upstream PR #156.
- `components/t/units.go` + `units_test.go`: `org.Unit` type removed in
  gobl v0.506.0; port to `cbc.Key` + `cbc.GetKeyDefinition` +
  `org.UnitMetaKeySymbol`. Ported from upstream PR #156.
- `components/regimes/ar/ar.go`: `pay.DueDate.Amount` became
  `*num.Amount` in gobl v0.505.0; nil-safe minimal variant (upstream PR
  #156 removes the whole tourism-refund path instead, but that depends
  on the unreleased gobl.ar.arca module).
- `examples/`: fixture ports from PR #156 (sg UEN codes, fr, mx, pt) +
  regenerated `mx-sat-invoice-food-voucher.html` golden (gobl v0.507
  unit-normalization drops the unit column). The PR's ar-arca and
  food-voucher fixture changes that encode unreleased-gobl behavior
  were intentionally NOT taken.
- `go.mod`/`go.sum`: gobl v0.507.0, gobl.dev v0.507.1 (bundle registers
  the full v0.507 external addon set incl. pl.ksef), addon module bumps
  (gobl.mx.cfdi v0.65.0, gobl.pt.saft v0.0.8, gobl.fr.ctc v0.0.8,
  gobl.sa.zatca v0.0.4, gobl.br.nfe v0.0.4, gobl.br.nfse v0.0.2).

Full test suite green against gobl v0.507.0 (build, vet, all packages
incl. the examples golden suite).

## When to drop this fork

When invopop/gobl.html releases a version whose `go.mod` requires gobl
>= v0.507.0 (or when PR #156 merges and releases). Then: remove the
`replace` directive in consumers, bump to the upstream tag, delete this
repo.

## Tagging

`v0.124.0-lars1` = upstream v0.124.0 + the deltas above.
