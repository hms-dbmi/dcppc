
# Using sandboxR, R package to streamline and sort clinical data

## Introduction
### Load the package
```R
install.packages("devtools", repos = "https://cran.rstudio.com/")
devtools::install_github("hms-dbmi/sandboxR", force = TRUE)
assignInNamespace("cedta.override", c(data.table:::cedta.override,"sandboxR"), "data.table")
library(sandboxR)```
### Download the latest NHANES dataset
For the purpose of this tutorial, we are using NHANES, a dataset publicly available (https://www.cdc.gov/nchs/nhanes/index.htm). The first function we created dowloads the various nhanes tables, and join the questionnaires across different years. By default, it will place the csv files in a directory called "NHANES_tables_per_questionnaire" folder on your working directory. You can add a "destination" argument if you want to change the destination of this folder
dl.nhanes()
Now, let's use sandboxR in order to clean our variables, and to gather them into a tree that will be easier to use for researchers.

## 1. Extract and Build your sandboxR tree
The goal of the following function is to create a folder with a "0_map.csv" file who will map all your variables, and a subfolder "study_tree" containing one .csv file per variable in your study, gathered by datatables.
study_name <- "NHANES"
files <- "NHANES_tables_per_questionnaire"
destination <- "path/were/your/tree/will/be/located"
table.expand("nhanes", "/Users/bidou/Desktop/NHANES_tables_per_questionnaire")
## 2. Transform your tree and build your sandboxes
### 2.1 Modify your tree
Once your first tree has been created,  you can easily modify the arrangement of your variables by creating new subdirectories, and by dragging and dropping your variable files. You can also change the name of your directories and variables. Be careful not to delete a variable file in this process.

Then, use the function TreeToMap() in order to reflect your modifications in your "0_map.csv" file
path <- "Pathway/to/the/folder/where/the/map/and/the/tree/are/located"
TreeToMap(path)
### 2.2 Modify your map
Similarly, you can modify the name of your files directly in the "0_map.csv" files. Modify the 5th column "data_label" to change the name of your variables. Use then the MapToTree() function in order to reflect your modifications in your tree.
MapToTree(path)
##### Be careful to apply your changes with MapToTree or TreeToMap BEFORE switching from 4.1 to 4.2 and from 4.2 to 4.1
## 3. Fixing and repairing issues
Each time you will use TreeToMap(), the old map will be saved with a time stamp in a hidden folder called ".oldmaps". Use the function list.oldmaps() in order to list your previous maps. Use look.oldmaps() in order to view one of these maps as a data.frame. Use recover.map() to change your tree and your "0_map.csv" according to one of your previous maps.
list.oldmaps(path)
look.oldmap(path, "olmap YYYY-MM-DD HHMM10 AM")
recover.map(path, "olmap YYYY-MM-DD HHMM AM")