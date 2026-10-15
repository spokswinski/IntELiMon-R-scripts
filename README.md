# IntELiMon R scripts

[![Version](https://img.shields.io/badge/version-1.2.0-blue)](CHANGELOG.md)
[![License: CC0-1.0](https://img.shields.io/badge/license-CC0--1.0-lightgrey)](LICENSE)
[![DOI](https://img.shields.io/badge/DOI-10.1093%2Fforestry%2Fcpag083-informational)](https://doi.org/10.1093/forestry/cpag083)

IntELiMon is an automated framework for standardized
processing of terrestrial lidar scans (TLS) for multi-disciplinary ecosystem monitoring. This
repository contains the R processing script that converts single-scan `.ptx` files into
standardized plot-level fuels, vegetation structure, occlusion, and tree/shrub inventory metrics.

> **Disclaimer:**
> Ecological monitoring utilizing terrestrial based lidar is a new and quickly evolving
> technology. The IntELiMon source code represents the current state of this technology in fuels
> and ecological monitoring. The Science Advisory Panel for the IntELiMon viewer welcomes
> discussion/input on peer reviewed methodologies that could be utilized to enhance the system.
> The New Mexico Consortium, The USDA Forest Service, US Department of Interior, or any person
> associated with IntELiMon, is not responsible for your use of the information contained in or
> linked to these scripts.

## Citation

If you use these scripts, please cite the following paper:

Scott Pokswinski, Kurtis Nelson, Aaron Maxwell, Emily Link, Hannah E Redford, Jeff W Atkins,
Andrew Sanchez Meador, Isaac Nealey, Daniel Crawl, Melissa Floca, Ilkay Altintas,
E Louise Loudermilk, Chad Anderson, Julia Oliveto, Rodman R Linn, Jon Wallace, J Kevin Hiers,
IntELiMon: an automated framework for standardized terrestrial lidar processing in
multi-disciplinary ecosystem monitoring, *Forestry: An International Journal of Forest
Research*, Volume 99, Issue 5, December 2026, cpag083,
<https://doi.org/10.1093/forestry/cpag083>

Please also report the script version used (for example, `IntELiMon_1.2.0`). The version is
written to the `h_script_name` column of every metrics output file. GitHub's
**"Cite this repository"** button (from [`CITATION.cff`](CITATION.cff)) provides APA and BibTeX
formats.

## Requirements

- R (4.x)
- CRAN packages: `nabor`, `dplyr`, `data.table`, `terra`, `raster`, `rlas`, `lidR`, `e1071`,
  `geometry`, `sf`, `Morpho`
- `TreeLS`, which has been archived on CRAN and must be installed from GitHub

```r
install.packages(c("nabor", "dplyr", "data.table", "terra", "raster", "rlas", "lidR",
                   "e1071", "geometry", "sf", "Morpho", "remotes"))
remotes::install_github("tiagodc/TreeLS")
```

## Usage

1. Place the `.ptx` scan files to be processed in one input folder.
2. Open [`IntELiMon.R`](IntELiMon.R) and set the input and output folders at the top of the
   script:

   ```r
   inDir <-  "path/to/input"   # Location of input files
   outDir <- "path/to/output"  # Location of output folders
   ```

3. Run the whole script. Every `.ptx` file in `inDir` is processed in turn.

The scan name used for all output files is the first `characters` (default 21) characters of
the input file name.

### Processing parameters

All parameters are set in the first block of the script.

| Parameter | Default | Description |
|---|---|---|
| `clipRadius` | 15 | Plot radius (m) the point cloud is clipped to |
| `b1`–`b4`, `bMax` | 0.5, 1, 1.5, 2, 30 | Height strata breaks (m) for point cloud metrics |
| `h1`–`h4`, `binMax` | 0.5, 1, 1.5, 2, 40 | Height strata breaks (m) for spherical voxel (occlusion) metrics |
| `binRes` | 1 | Radial bin width (m) for spherical voxels |
| `spacing` | 0.5 | Cartesian grid spacing (m) for spherical-to-Cartesian conversion |
| `agg_factor` | 10 | Aggregation factor for the scan-angle raster grid |
| `length`, `vsize` | — | Voxel edge lengths and volumes used for box-count fractal dimension |
| `characters` | 21 | Number of file name characters used as the scan name |
| `script_name` | `IntELiMon_1.2.0` | Version label written to the metrics output |

## Outputs

The script creates these folders in `outDir`:

| Folder | Contents |
|---|---|
| `las/` | Converted and clipped point cloud (`<scan>.las`) |
| `dtm/` | Digital terrain model (`<scan>_dtm.tif`) |
| `chm/` | Canopy height model (`<scan>_chm.tif`) |
| `metrics/` | Plot-level metrics table (`<scan>_metrics.csv`) |
| `Inventory/` | Tree inventory with location, height, DBH and basal area (`<scan>_inv.csv`) |
| `Shrubs/` | Shrub inventory (`<scan>.csv`) |
| `Fuels/` | Shrub height raster (`<scan>.tif`) |

Descriptions of every metric in the metrics table are in
[`docs/metrics_descriptions.csv`](docs/metrics_descriptions.csv), with a condensed summary in
[`docs/output_readme.csv`](docs/output_readme.csv).

## Versioning

This project uses [Semantic Versioning](https://semver.org/), starting with version **1.2.0**.

- **MAJOR**: changes to the metrics output schema (columns renamed or removed)
- **MINOR**: new metrics, or method changes that change metric values
- **PATCH**: bug fixes and documentation that do not change the methodology

Release history is in [`CHANGELOG.md`](CHANGELOG.md). Each release is tagged (`v1.2.0`, ...)
and published as a GitHub Release.

### Releasing a new version

1. Update the version in [`VERSION`](VERSION).
2. Update `script_name` near the top of [`IntELiMon.R`](IntELiMon.R) (for example,
   `'IntELiMon_1.3.0'`).
3. Update `version` and `date-released` in [`CITATION.cff`](CITATION.cff).
4. Add an entry to [`CHANGELOG.md`](CHANGELOG.md).
5. Commit, then tag and publish the release:

   ```bash
   git tag -a v1.3.0 -m "IntELiMon 1.3.0"
   git push origin main --tags
   gh release create v1.3.0 --title "IntELiMon 1.3.0" --notes-file <release-notes>
   ```

## Contributing

Questions, bug reports and suggestions of peer-reviewed methods are welcome through
[GitHub Issues](../../issues). Proposed method changes are reviewed by the IntELiMon Science
Advisory Panel.

## Acknowledgments

IntELiMon was developed by the New Mexico Consortium in collaboration with the USDA Forest
Service, the US Department of the Interior and the co-authors listed above. The PTX header
reader is adapted from code by Jeff W. Atkins, and the leaf area density functions are adapted
from the [leafR](https://github.com/DRAAlmeida/leafR) package.

## License

Released under [CC0 1.0 Universal](LICENSE).
