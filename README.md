# BOIN-ETC-RShiny
BOIN-ETC Trial Design by Kakizume et al., 2024. Reference: Kakizume T, Takeda K, Taguri M, Morita S. BOIN-ETC: A Bayesian optimal interval design considering efficacy and toxicity to identify the optimal dose combinations. Stat Methods Med Res. 2024 Apr;33(4):716-727.

**BOINETC**
`BOINETC` is an R package assembled from the provided BOIN-ETC script (located at: http://github.com/kakizume/BOINETC). It preserves the original function names and adds package-friendly helpers for scenario selection, simulation, and summary output.

**Included core functions**
`BOINETC.subtrial()` — select the next dose in a BOIN-ETC subtrial.
`get.oc.BOINETC()` — calculate operating characteristics for BOIN-ETC1.
`get.oc.BOINETC_m2()` — calculate operating characteristics for BOIN-ETC2.
`get.oc.BOINETC_m3()` — calculate operating characteristics for BOIN-ETC3.
`CALC.SUM()` — summarize simulation data and identify ODCs.

**Added convenience helpers**
`get_boinetc_scenario(SC)` — retrieve one of the 10 included scenarios.
`run_boinetc_study()` — run one or more BOIN-ETC methods over selected scenarios.
`run_boinetc_workflow()` — run simulations and summaries in one call.
`write_boinetc_summary_workbook()` — write `Sum1` and `Sum2` summary tables to Excel.
The original uploaded scripts are preserved under `inst/extdata/original/`.

**Installation**
1. Download BOINETC_0.1.0.tar.gz to a local folder, e.g., C:/myfolder/BOINETC_0.1.0.tar.gz,
2. Install locally with the correct directory. install.packages("C:/myfolder/BOINETC_0.1.0.tar.gz", repos = NULL, type = "source").

**From the source tarball:**
```r
install.packages("BOINETC_0.1.0.tar.gz", repos = NULL, type = "source")
```
**From an unpacked local folder:**
```r
install.packages("path/to/BOINETC", repos = NULL, type = "source")
```
**Optional dependencies**
`CALC.SUM()` and `run_boinetc_workflow()` require a package that provides `biviso()` such as `UniIsoRegression` or `Iso`. **Excel output requires `openxlsx`.**
```r
install.packages(c("UniIsoRegression", "Iso", "openxlsx"))
```
**Quick example**
```r
library(BOINETC)

outdir <- tempfile("boinetc-")
dir.create(outdir)

sc <- get_boinetc_scenario(1)

set.seed(1234)
get.oc.BOINETC(
  ncohort = 2,
  cohortsize = 3,
  n.earlystop = 6,
  ntrial = 5,
  phi = 0.30,
  delta = 0.60,
  lambda1 = 0.14,
  lambda2 = 0.35,
  eta1 = 0.48,
  tdose = sc$tdose,
  pt.true.mat = sc$pt.true.mat,
  pe.true.mat = sc$pe.true.mat,
  filename = "demo_m1_sc1",
  outdir = outdir
)
```
**A full workflow, after installing optional dependencies:**

```r
library(BOINETC)

res <- run_boinetc_workflow(
  scenario_ids = 3:6,
  ncohort = c(16, 24),
  n.earlystop = c(6, 9),
  ntrial = 1000,
  outdir = "output",
  seed = 1234
)

res$out.sum1
res$out.sum2
```

**R Shiny App**

An R shiny app is developed to visualize the functions. 
1. Download BOINETC_Shiny_App.zip
2. Unzip to a local directory.
3. Open app.R in R studio to run the App.
