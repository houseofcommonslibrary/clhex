# clhex

A collection of hexmap tools for statistical researchers. This package is principally designed for use by researchers in the House of Commons Library but may be useful to anyone using R for working with hexmap data.

## Installation

Install from GitHub using remotes.

``` r
install.packages("remotes")
remotes::install_github("houseofcommonslibrary/clhex")
```

## HexJSON

These functions are used to convert data from tabular formats to hexjson. The `convert_hexjson` functions can be used to convert tabular data that already contains hexjson coordinates, while the `create_hexjson` functions can be used to produce initial hexjson coordinates data for a hexmap of geographic areas. The hexjson data generated from these functions can then be edited in a [hexjson editor].

These functions take tabular data for a collection of geographic areas and convert it to a set of hexes represented as hexjson. If you do not already have hexjson coordinates for these areas, the `create_hexjson` functions will assign unique grid coordinates to each hex. Data may be provided in the form of a dataframe, tibble or csv. 

The tabular data may contain codes, names, and categorical or numerical values for each area to be mapped. However, it is assumed that the first column of the table contains data that uniquely idenitfies each area, and which functions as the key for each area within the hexjson object. The remaining columns may contain any values that can be validly represented as json; this data will be included in the output hexjson as properties of each hex. Tabular data passed to the `convert_hexjson` functions should contain columns named `q` and `r` containing column and row hexjson coordinates.

A [hexjson editor] can be used on the output of these functions to rearrange the hexes and alter the design of the resulting hexmap, which can then be exported as either hexjson or geojson.

[hexjson editor]: <https://olihawkins.com/project/hexjson-editor/>

### Convert hexjson


`convert_hexjson` converts a dataframe of codes, names, hexjson coordinates, and other data to a hexjson string. The values in the first column are used as the key for each hex in the hexjson and therefore must be unique. The hexjson coordinates should be supplied in columns named `q` and `r`.

``` r
data <- tibble::tibble(
    name = letters[1:25],
    group = rep(letters[1:5], rep(5, 5)),
    value = 1:25,
    q = rep(1:5, 5),
    r = rep(1:5, rep(5, 5)))

# Convert hexjson with the default layout
hexjson <- convert_hexjson(data)

# Convert hexjson with the specified layout
hexjson <- convert_hexjson(data, "odd-q")
```

`convert_and_save_hexjson` is identical to `convert_hexjson` but also saves the hexjson string to a file.

``` r
# Convert and save hexjson with the default layout
convert_and_save_hexjson(data, "output.hexjson")

# Convert and save hexjson with the specifed layout
convert_and_save_hexjson(data, "output.hexjson", "odd-q")
```

`convert_hexjson_from_csv` is identical to `convert_and_save_hexjson` but reads the data in from the given csv.

``` r
# Convert hexjson from a csv with the default layout
convert_and_save_hexjson("input.csv", "output.hexjson")

# Convert hexjson from a csv with the specified layout
convert_and_save_hexjson("input.csv", "output.hexjson", "odd-q")
```

### Create hexjson

`create_hexjson` converts a dataframe of codes, names, and other data to a hexjson string, adding unique column and row coordinates for each hex. The values in the first column are used as the key for each hex in the hexjson and therefore must be unique.

``` r
data <- tibble::tibble(
    name = letters[1:25],
    group = rep(letters[1:5], rep(5, 5)),
    value = 1:25)

# Create hexjson with the default layout
hexjson <- create_hexjson(data)

# Create hexjson with the specified layout
hexjson <- create_hexjson(data, "odd-q")
```

`create_and_save_hexjson` is identical to `create_hexjson` but also saves the hexjson string to a file.

``` r
# Create and save hexjson with the default layout
create_and_save_hexjson(data, "output.hexjson")

# Create and save hexjson with the specifed layout
create_and_save_hexjson(data, "output.hexjson", "odd-q")
```

`create_hexjson_from_csv` is identical to `create_and_save_hexjson` but reads the data in from the given csv.

``` r
# Create hexjson from a csv with the default layout
create_and_save_hexjson("input.csv", "output.hexjson")

# Create hexjson from a csv with the specified layout
create_and_save_hexjson("input.csv", "output.hexjson", "odd-q")
```
