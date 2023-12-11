
<!-- README.md is generated from README.Rmd. Please edit that file -->

# mkLRD

<!-- badges: start -->
<!-- badges: end -->

- The goal of mkLRD is to create or update a cumulative tracking sheet for Large Result (Score) Difference cases (since April 1st 2023). Specifically, the program automate the following major steps:
1)	Extract existing exam information from EPRs of LRD cases (e.g., FName, LName, EPRID, CustID, Attempt, Theta, Time, NItem).
2)	Create several difference variables based on existing information (e.g., date difference, NItem difference, theta difference, and Time difference)
3)	Clean cumulative LRD records and produce an organized cumulative tracking sheet.

- The tracking sheet is in long format. Each row represents an exam and can be uniquely identified by the column "EPRID". 

- The program will generate or update a cumulative tracking sheet named "NGN Cumulative LRDs.xlsx" in the specified result directory.

- Should you find any issues, please contact

<font size="2"> `Mingqin Zhang` Email: <mzhang@ncsbn.org> </font>


## Main steps

1. Load the relevant packages:

``` r
library(dplyr)
library(tibble)
library(writexl)
library(readxl)
library(tidyverse)
library(openxlsx)
library(stringr)
library(pdftools)
```

2. Select your working directory and manually create:
* "EPR" folder to save NCLEX format EPRs.
* "NGN EPR" folder to save NGN format EPRs. 
For new LRD cases, download their EPRs and save to the folder based on format. There's no requirement for EPR file name. Specify the working directory you selected in the program.

``` r
# Specify working directory
wk_folder <- "/NGN Score Hold/NGN LRD/Training/" 
NCLEX_EPR_path <- paste0(getwd(), wk_folder, "EPR/")
NGN_EPR_path <- paste0(getwd(), wk_folder, "NGN EPR/")
```

Note: After running the program, the EPRs will be automatically removed to the "Processed" subfolders inside the "EPR" and "NGN EPR" folders so that these cases won't be added to the tracking sheet again.

3. Specify the result directory to save the generated cumulative LRD tracking sheet. 

For example, the working directory and result directory are set to be the same here. 

``` r
# Specify result directory
result_path <- paste0(getwd(), wk_folder)
```

Note: If there's no existing "NGN Cumulative LRDs.xlsx" file in the result directory, the program will generate a new "NGN Cumulative LRDs.xlsx" file after running the program; if there's already an existing "NGN Cumulative LRDs.xlsx" file in the result directory, the program will append the new cases to the existing list and update the "NGN Cumulative LRDs.xlsx" file.

4. The generated tracking sheet is sorted by "CustID" and "EPRID". You can specify how exams are sort within each candidate. The default sorting is 'ascending', which sort the tracking sheet by `arrange(CustID, EPRID)`. If you use "descending" sorting, the tracking sheet is sorted by `arrange(CustID, desc(EPRID))`.

``` r
# How to sort exams of each candidate
sort = 'ascending'
```

5. Specify whether you want to save a separate daily copy of cumulative tracking sheet for records. The cumulative tracking sheet "NGN Cumulative LRDs.xlsx" in the result directory will be overwritten when running the program. If you choose to save a daily copy, the program will automatically create a folder "Copies" and save the daily copy in the folder.

``` r
# Whether to create a daily copy of cumulative tracking sheet
saveDailyCopy <- T
```

6. Run the function `create_LRD()` to create or update cumulative LRD tracking sheet:

``` r
#### Create LRD records ####
LRD_results <- create_LRD(NCLEX_EPR_path, NGN_EPR_path, result_path, sort = sort)
```

7. Run the function `write_LRD()` to write out the cumulative LRD tracking sheet:

``` r
#### Write out LRD records ####
write_LRD(LRD_results$combined_LRDs, result_path, saveDailyCopy)
```
