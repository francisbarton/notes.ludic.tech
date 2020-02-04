---
title: "Ashfield project - first steps"
tags:
  - notes
  - #twirl

---

When I first started getting boundaries I would use local shapefiles and subset them for the area I wanted:

```{r}
# Get LA boundaries
# LA boundaries shapefile
uk_area <- read_sf("C:/Users/Francis/...../OutputAreas2011/OA_2011_EW_BFE_V2.shp")

# This process of subsetting the shapefile is very slow, so once done, we
# save the results as an Rdata (.Rda) file - the subset doesn't need to
# change - then comment out those lines and just have a single line
# loading in the data for future runs

# Epping Forest
# epping_oas <- subset(uk_area, LAD11CD == "E07000072")
# saveRDS(epping_oas, "epping_oas.Rda")
epping_oas <- readRDS("epping_oas.Rda")

```

Then I would use the Open Geography API and I was being quite clever.
I would do something like this for every data set:

**version 2**

```{r}
# gets boundary file json from ONS Open Geography site
# Local Authority boundaries (BGC) April 2019
# https://geoportal.statistics.gov.uk/datasets/local-authority-districts-april-2019-boundaries-uk-bgc

get_la_boundary <- st_read(
  "https://services1.arcgis.com/ESMARspQHYMw9BZ9/arcgis/rest/services/LAD_APR_2019_UK_BGC/FeatureServer/0/query?where=LAD19NM%20like%20%27%25ASHFIELD%25%27&outFields=*&outSR=4326&f=json")
```

...using the URL direct from the ONS page.
Or I might, for presentation's sake, do:

**version 3**
```{r}
get_la_boundary <- st_read(
      paste0(
        "https://services1.arcgis.com/ESMARspQHYMw9BZ9/arcgis/rest/services/",
        "LAD_APR_2019_UK_BGC",
        "/FeatureServer/0/query?where=LAD19NM%20like%20%27%25",
        "ASHFIELD",
        "%25%27&outFields=*&outSR=4326&f=json"
    )
  )
```

My latest iteration started like this, though, taking it to the next level by writing a one-time function (but with a view to more easily reusing it in the next project)...

Here I wrote function around `x` which could then be set as "Ashfield" in the line below.

**version 4**
```{r}
# gets boundary file json from ONS Open Geography site
# Local Authority boundaries (BGC) April 2019
# https://geoportal.statistics.gov.uk/datasets/local-authority-districts-april-2019-boundaries-uk-bgc

get_la_boundary <- function (x) {
  x <- toupper(x)
  st_read(
    paste0(
    "https://services1.arcgis.com/ESMARspQHYMw9BZ9/arcgis/rest/services/",
    "LAD_APR_2019_UK_BGC",
    "/FeatureServer/0/query?where=LAD19NM%20like%20%27%25",
    x,
    "%25%27&outFields=*&outSR=4326&f=json"
    )
  )
}
ashf_dist_boundary <- get_la_boundary("Ashfield")
```

After putting about three of these in my script I realised that all the parts of that long unwieldy weird URL were basically the same, and I realised that I could generalise the function, and even put it in a separate file (in order to declutter my main analysis file).
As well as the parameter `x` for the area of interest, which I re-wrote as `area` below, I realised I could change the hardcoded dataset code like `"LAD_APR_2019_UK_BGC"` into a function parameter as well, enabling the function to simply called in a single pass.

The one thing remaining was the difference between calling for boundary data with `st_read` and calling for other data such as lookups with plain old `fromJSON`.
I could either pass these as a third parameter, or write a two separate but similar functions.

For now I think I will do the latter.
Or maybe - I don't know yet if this works - just always use `fromJSON` but then for spatial sets like boundaries try calling `st_read` on the data post-retrieval.

```{r}
## $PROJECT_HOME/code/get_open_geodata.R

# hardcoding "Ashfield" in here as `area` as I am working just on Ashfield
# district but trivial to change this if importing into another project -
# or of course can be changed as the function is called

get_opengeo_data <- function(dataset, area = "Ashfield") {
  area <- toupper(area)
  fromJSON(
    paste0("https://services1.arcgis.com/ESMARspQHYMw9BZ9/arcgis/rest/services/",
           dataset,
           "/FeatureServer/0/query?where=LAD19NM%20like%20'%25",
           area,
           "%25'&outFields=*&outSR=4326&f=json"
    )
  )
}
```

This file is then called in my main script with

```{r}
source(here("code", "get_opengeo_data.R"))
```

which then puts the function into my working environment.

This will seem simple to coding veterans, but to me as someone quite new to R and to coding at all, this kind of thing feels like quite an achievement and something when I started would have seemed incredibly arcane and quite frankly unnecessary.

But then I reached a point halfway through last year, my first year of coding in R, where I could see the advantages of doing this kind of thing, but still could not yet work out how to make it work.

My understanding needed to grow, and my skill and sense of how to manipulate and refactor code both in "literary" terms (actually moving the code around and getting a feel for _syntax_) as well as in abstract terms (thinking more clearly about code objects and the flow of variables and data through functions: relationships) needed time to develop.

I needed to understand which bits of a script make sense to put into functions, which bits can be put into separate snippets and how to source these in.

### Problems with working with the downloaded JSON data

Trying to write a function to get boundaries from ONS Open Geography site.
The problem here is making my brain ache.

_note: I need to sort out a reprex to investigate this stuff._


```{r}

# this works
ashfield_lsoas2 <- ashfield_lsoas[["features"]] %>% 
  map_df(bind_rows)
  
# this doesn't
ashfield_lsoas2 <- ashfield_lsoas %>%
  map("features") %>% 
  map_df(bind_rows)

# this also doesn't
ashfield_lsoas2 <- ashfield_lsoas %>%
  extract("features") %>% 
  map_df(bind_rows)
```

The problem here is that the first example, the one that works, can't be easily put into a pipe system because it requires me to manually write in the `[["features"]]` bit as a new instruction in the code. Whereas the latter, which, to my mystification, don't work (despite Jenny Bryan's instructions [here](https://jennybc.github.io/purrr-tutorial/ls01_map-name-position-shortcuts.html#name_and_position_shortcuts) explicitly saying that `df %>% map("features")` is equivalent to `df[["features"]]`), are pipeable.