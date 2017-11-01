
<!-- README.md is generated from README.Rmd. Please edit that file -->
<a rel="Exploration" href="https://github.com/BCDevExchange/docs/blob/master/discussion/projectstates.md"><img alt="Being designed and built, but in the lab. May change, disappear, or be buggy." style="border-width:0" src="https://assets.bcdevexchange.org/images/badges/exploration.svg" title="Being designed and built, but in the lab. May change, disappear, or be buggy." /></a>

Analysis of land designations that contribute to conservation
=============================================================

This repository contains R code that calculates summaries of the amount of land designated in B.C. that contributes to conservation. It supports this [Environmental Reporting BC indicator]().

Usage
-----

### R packages required:

Other than `bcmaps` and `envreportutils`, all required packages are available from CRAN

-   bcmaps (install with `devtools::install_github("bcgov/bcmaps")`)
-   envreportutils (install with `devtools::install_github("bcgov/envreportutils")`)
-   geojsonio
-   rmapshaper
-   feather
-   readr
-   dplyr
-   tidyr
-   sf
-   ggplot2
-   ggpolypath
-   magrittr
-   ggthemes
-   svglite

### Data

Input data is obtained by running the python script [here](https://github.com/bcgov/designatedlands) using the following commands:

    ## Download the Biogeoclimatic Zone layer from here: 
    ## https://catalogue.data.gov.bc.ca/dataset/biogeoclimatic-ecosystem-classification-bec-map
    ## and store it in your working directory

    ## Download the Ecosection layer from here: 
    ## https://catalogue.data.gov.bc.ca/dataset/ecosections-ecoregion-ecosystem-classification-of-british-columbia
    ## and store it in your working directory

    python designatedlands.py create_db

    python designatedlands.py load

    python designatedlands.py process

    python designatedlands.py dump --out_file=designatedlands.gpkg

    python designatedlands.py overlay BEC_BIOGEOCLIMATIC_POLY.gdb --in_layer=WHSE_FOREST_VEGETATION_BEC_BIOGEOCLIMATIC_POLY_polygon --new_layer_name=bec

    python designatedlands.py overlay ERC_ECOSECTIONS_SP.gdb --in_layer=WHSE_TERRESTRIAL_ECOLOGY_ERC_ECOSECTIONS_SP_polygon --new_layer_name=eco

    python designatedlands.py dump --out_table=bec_overlay --out_file=lands_bec.gpkg --aggregate_fields="bc_boundary,designation,category,zone,subzone,variant,phase,map_label"

    python designatedlands.py dump --out_table=eco_overlay --out_file=lands_eco.gpkg --aggregate_fields="bc_boundary,designation,category,parent_ecoregion_code,ecosection_code,ecosection_name"

The outputs from that script should be:

-   `designatedlands.gpkg` - the land designations layer
-   `lands_bec.gpkg` - the land designations layer intersected with BEC (using the `overlay` command in the conservationlands tool)
-   `lands_eco.gpkg`- the land designations layer intersected with ecosections (using the `overlay` command in the conservationlands tool)

Place these in the `data` folder of this repository. You will also require `BEC_BIOGEOCLIMATIC_POLY.gdb` from [here](https://catalogue.data.gov.bc.ca/dataset/biogeoclimatic-ecosystem-classification-bec-map) and `ERC_ECOSECTIONS_SP.gdb` from [here](https://catalogue.data.gov.bc.ca/dataset/ecosections-ecoregion-ecosystem-classification-of-british-columbia)

The [`data-raw`](data-raw) folder contains a file [`sources.csv`](data-raw/sources.csv) that lists all of the source land designation layers used in the analysis.

### Running the analysis

-   Run the `01_clean.R` file to prepare the data. Note that this will take a long time (several hours)
-   Run the `02_analysis.R` file to calculate the summaries.
-   Summary csv files are copied to the `out` directory, and summary tabular objects for use in the [land designations Shiny app](https://github.com/bcgov/land-designations-shinyapp) are copied to the `out-shiny` directory.
-   Run the `03_output_static.R` script to create outputs (png maps and graphs etc).
-   Run the `04_output_shiny.R` scrip to generate the objects required for the shiny app. This will also copy the files from `out-shiny` to the `../land-designations-shinyapp/app/data` folder, assuming the folder exists in same parent directory as this repository

Pathway to Open Source
----------------------

Once the data are available, this repo will be moved to the bcgov org and developed in the open.

Getting Help or Reporting an Issue
----------------------------------

To report bugs/issues/feature requests, please file an [issue](https://github.com/bcgov-c/land-designations-indicator/issues/).

How to Contribute
-----------------

If you would like to contribute, please see our [CONTRIBUTING](CONTRIBUTING.md) guidelines.

Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

License
-------

    Copyright 2016 Province of British Columbia

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at 

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

This repository is maintained by [Environmental Reporting BC](http://www2.gov.bc.ca/gov/content?id=FF80E0B985F245CEA62808414D78C41B). Click [here](https://github.com/bcgov/EnvReportBC-RepoList) for a complete list of our repositories on GitHub.
