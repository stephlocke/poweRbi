# poweRbi
An R-based, httr-style interface for the Power BI REST API. Inspired by the work of libraries like twitteR and httr in R, 
this library aims to provide a useful set of tools for navigating the (fairly limited) Power BI REST API for their web services.

You will need to register an app before you can use this library: https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-overview-of-power-bi-rest-api/

#### Before installing:
This is still in development and fairly experimental. Much of the functionality is ready to use, so if there are any significant improvements
that can be made during this process then feel free to make changes and contribute as you please.

    devtools::install_github("olfrost/poweRbi")
    
### Examples

See the demo directory for the full code for the following examples:

    library(poweRbi)

#### Authenticate your session.
    options(httr_oauth_cache = FALSE)
    pbiAuthenticate(appName, clientId, clientSecret) ## fix readRDS

#### Get metadata about objects in your workspace

    # Datasets
    pbiListAllDatasets()
    pbiListAllDatasets(toDf = FALSE)
    
    # Dashboards
    pbiListAllDashboards()
    
    # Imported files
    pbiListAllImportedFiles()
    
    # Reports
    pbiListAllReports()
    
    # Tables
    pbiListAllTables(allDatasets = TRUE)
    pbiListAllTables(id = "cfcf6cde-45b7-459d-a8a3-a09644f5cbbe")
    
    # Tiles
    pbiListAllTiles(allDashboards = TRUE)
    pbiListAllTiles(id = "cfbd4b32-3160-437b-8076-94e6cab9aa4a")

    # Data sources
    pbiListAllDataSources()
    
    # User groups
    pbiListAllUserGroups()

#### Create datasets from data frames
    pbiCreateDatasetFromDataFrame(
      name = "Iris Dataset",
      df = iris,
      tableName = "Iris Data Frame",
      FIFO = "basicFIFO"
    )

#### Manage existing datasets

    # Use populated (or empty) data frames to update tables in Power BI.
    pbiUpdateTableSchema(
      iris3,
      guid = "abcdefg",
      tableName = "Iris Data Frame"
    )

    # Truncate tables.
    pbiTruncateDataset(
      guid = "cfcf6cde-45b7-459d-a8a3-a09644f5cbbe",
      tableName = "Triangle"
    )

    # Delete entire datasets.
    pbiDeleteDataset(guid = "cfcf6cde-45b7-459d-a8a3-a09644f5cbbe")

    # Create new rows.
    pbiAddRowsToTable(
      df = cats,
      guid = "abcdef",
      tableName = "Iris Data Frame"
    )

#### Utilities

    # What data types does Power BI implement?
    pbiDataTypes(simple = TRUE, verbose = FALSE)
    pbiDatasetTypes()

    # Infer data types from a data frame.
    sapply(MASS::cats, ._pbiGetDataType)

    # Generate JSON objects or lists from data frames to be passed to Power BI.
    pbiGenerateTableSchema(
      MASS::cats,
      tableName = "Many Cats"
    )

    pbiGenerateTableSchema(
      MASS::cats,
      debug = TRUE,
      tableName = "Meowsers in my Trousers"
    )

    # Automatic token refreshing.
    ._pbiRefresh()
    ._pbiTokenExpiresIn()

