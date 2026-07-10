# leafwax-data

Versioned posterior draws backing the [`leafwax`](https://github.com/bradleylab/leafwax)
R package. This repo holds the heavy data (~10 MB across 14 models)
that is excluded from the CRAN tarball; the package downloads from
here on first use.

Citable via the Zenodo DOI minted on every tagged release; see
`CITATION.cff` for the current concept DOI.

## Contents

Fourteen `<model>_posterior.rds` files, one per hierarchical Bayesian
calibration model from Bradley (2026), *Communications Earth and
Environment*. Each file is a `posterior::draws_df` with 1000 stratified
draws across 8 chains, columns named to match the frozen-run Stan model
(calibration run `c2_run_20260626`, n = 1128 observations).

| Model | Spatial GP | Elevation | Vegetation | Interactions |
|---|---|---|---|---|
| `baseline` | – | – | – | – |
| `baseline_sp` | yes | – | – | – |
| `baseline_env` | – | yes | – | – |
| `baseline_env_sp` | yes | yes | – | – |
| `baseline_veg` | – | – | C4 + PFT | – |
| `baseline_veg_sp` | yes | – | C4 + PFT | – |
| `c4_only_sp` | yes | – | C4 only | – |
| `elevation_only_sp` | yes | yes | – | – |
| `elevation_c4_sp` | yes | yes | C4 only | – |
| `elevation_c4_interact_sp` | yes | yes | C4 only | yes |
| `full` | – | yes | C4 + PFT | – |
| `full_sp` | yes | yes | C4 + PFT | – |
| `full_interact` | – | yes | C4 + PFT | yes |
| `full_interact_sp` | yes | yes | C4 + PFT | yes |

Spatial models carry per-knot latent values
(`z_intercept_spatial[1..125]`, `z_slope_spatial[1..125]`) on a
125-point Fibonacci sphere lattice plus the GP hyperparameters
(`ls_intercept_km`, `ls_slope_km`, `sigma_intercept_spatial`,
`sigma_slope_spatial`).

`manifest.json` carries sha256 checksums, byte sizes, and the
manuscript reference; the leafwax package uses it to verify file
integrity after download.

## Use

The intended consumer is the leafwax R package's
`download_model_data()` function, which fetches the relevant file
into the user's cache on first inversion. For direct use:

```r
url <- "https://raw.githubusercontent.com/bradleylab/leafwax-data/v2.0.0/baseline_sp_posterior.rds"
draws <- readRDS(url(url))
```

## Versioning

Each release is a refit or revision of the calibration models; older
versions remain accessible at their version DOIs and at their git
tags. The model schema may change between major versions; check
`manifest.json`'s `version` field.

| Tag | Models | Draws/model | Stan version | Notes |
|---|---|---|---|---|
| `v1.0.0` | 14 | 1000 | 2.34 | Initial deposit; metadata placeholder DOIs (do not cite). |
| `v1.0.1` | 14 | 1000 | 2.34 | Same posteriors as v1.0.0; metadata corrected (real DOI, CFF-spec compliance, fixed URL example). v10 / n=1129 lineage. |
| `v2.0.0` | 14 | 1000 | 2.34 | Re-fit on frozen run `c2_run_20260626`, n=1128 (Africa 142); supersedes v1.x v10/GCA lineage. Manuscript: *Communications Earth and Environment*. Version DOI: TBD (minted on release). |

## Citation

If you use these posteriors directly (rather than via the leafwax
package), please cite:

> Bradley, A. (2026). leafwax model posteriors. Zenodo.
> https://doi.org/10.5281/zenodo.20085465

The leafwax R package itself is cited separately. See its DESCRIPTION
file for the package citation.

## License

CC-BY 4.0. Reuse freely with attribution.
