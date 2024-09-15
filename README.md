# Prediction Model - NYC

## Table of Contents
- [Overview](#Overview)
  - File Layout
- [Setting up Notebook](#Setting-up-Notebook)
  - Installing Python
  - Dependencies
- [The Data](#The-Data)
  - Property Sales
  - Cleaning Property Sales
  - Neighborhood Shapes
  - Matching Neighborhood Shapes and Property Sales
- [The Model](#The-Model)
- [Predictions compared to 2022](#Predictions-compared-to-2022)
  - Manhattan
  - Bronx
  - Brooklyn
  - Queens
  - Staten Island

## Overview
NYC is divided into five boroughs: Manhattan, the Bronx, Brooklyn, Queens, and Staten Island. Homes are categorized into one-family, two-family, and three-family types. This model focuses on the average sale price of each home type in their respective boroughs and aims to predict future average property prices.
### File Layout
```
CSV
  - 2010-2021.csv
  - 2022.csv
nynta2020_24b
  - nynta2020 multi-polygon files
[BOROUGH] (manhattan, bronx, brooklyn, queens, staten island)
  - [BOROUGH]_one
  - [BOROUGH]_two
  - [BOROUGH]_three
  - [BOROUGH]_test
  - xOne.png
  - xTwo.png
  - xThree.png
  - readme.md
```
- BOROUGH_one contains predictions for One Family Homes
- BOROUGH_two contains predictions for Two Family Homes
- BOROUGH_three contains predictions for Three Family Homes
- BOROUGH_test contains prediction comparisons for ALL three home types to 2022's average sale price
- .png are the predictions projected on to maps of neighborhoods

## Setting up Notebook
### Installing Python
MacOS already comes with Python3 (system python, don't mess with it)
- To check current python verison open the terminal and type `python --verison`
- returns something like: `Python 3.9.6` 
  
We want a fresh install, of the latest verison of Python3 as 3.9's support is ending this year
- This project will use [Python 3.12.4 64-bit](https://www.python.org/downloads/)
- The version should change by itself, we can check by typing `python --verison`
- We can also look at the path by using command `which python3`
- returns: **/Library/Frameworks/Python.framework/Versions/3.12/bin/python3**
- Make sure at the end of the install to get the **certificate file** with (Install Certificates.command) in folder

---
### Dependencies
- `pip3 install python-dotenv`
- `python3 -m venv .venv`
- `pip3 install notebook pandas scikit-learn geopandas matplotlib`
- `jupyter notebook`
    - opens notebook in browser
    - links to notebook is in terminal 
- `pip3 freeze > requirements.txt`

## [The Data](https://opendata.cityofnewyork.us/data/)
### Property Sales
| BOROUGH | NEIGHBORHOOD | TYPE OF HOME | NUMBER OF SALES | LOWEST SALE PRICE | AVERAGE SALE PRICE | MEDIAN SALE PRICE | HIGHEST SALE PRICE | YEAR |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| String | String | String | Int | Int | Int | Int | Int | String |

### Cleaning Property Sales
| Column | Original | Clean |
| --- | --- | --- |
| TYPE OF HOME | 01 ONE FAMILY DWELLINGS <br> 01-ONE FAMILY DWELLINGS | 01 ONE FAMILY HOMES | 
| TYPE OF HOME | 02 TWO FAMILY DWELLINGS <br> 02-TWO FAMILY DWELLINGS | 02 TWO FAMILY HOMES | 
| TYPE OF HOME | 03 THREE FAMILY DWELLINGS <br> 03-THREE FAMILY DWELLINGS | 03 THREE FAMILY HOMES |
| BOROUGH | 1 | MANHATTAN |
| BOROUGH | 2 | BRONX |
| BOROUGH | 3 | BROOKLYN |
| BOROUGH | 4 | QUEENS |
| BOROUGH | 5 | STATEN ISLAND |
| NEIGHBORHOOD | Nothing at index 1168 | DELETED ROW |

### Neighborhood Shapes
| BoroCode | BoroName | CountyFIPS | NTA2020 | NTAName | NTAAbbrev | NTAType | CDTA2020 | Shape_Leng | Shape_Area | CDTAName | geometry |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Int | String | Int | String | String | String | Int | Int | Int | Int | String | MultiPolygon |

### Matching Neighborhood Shapes and Property Sales
Neighborhood names from Property Sales and Neighborhood Shapes differed from eachother so for each borough neighborhood names needs to be matched. Since there are three sets being worked on in each borough, the pairing needs to be done three times per borough as sales at X neighborhood from ONE may not appear in TWO, and vice versa. ie) Manhattan ONE and TWO
```python
pair_one = {
    'Financial District-Battery Park City': 'SOUTHBRIDGE',
    'Tribeca-Civic Center': 'TRIBECA',
    'The Battery-Governors Island-Ellis Island-Liberty Island': None,
    'SoHo-Little Italy-Hudson Square': ['SOHO', 'LITTLE ITALY'],
    'Greenwich Village': 'GREENWICH VILLAGE-CENTRAL',
    'West Village': 'GREENWICH VILLAGE-WEST',
    'Chinatown-Two Bridges': None,
    'Lower East Side': 'LOWER EAST SIDE',
    'East Village': ['EAST VILLAGE', 'ALPHABET CITY'],
    'Chelsea-Hudson Yards': 'CHELSEA',
    "Hell's Kitchen": 'CLINTON',
    'Midtown South-Flatiron-Union Square': 'FLATIRON',
    'Midtown-Times Square': 'MIDTOWN EAST',
    'Stuyvesant Town-Peter Cooper Village': None,
    'Gramercy': 'GRAMERCY',
    'Murray Hill-Kips Bay': ['MURRAY HILL', 'KIPS BAY'],
    'East Midtown-Turtle Bay': None,
    'United Nations': None,
    'Upper West Side-Lincoln Square': ['UPPER WEST SIDE (59-79)', 'UPPER WEST SIDE (79-96)'],
    'Upper West Side (Central)': 'UPPER WEST SIDE (79-96)',
    'Upper West Side-Manhattan Valley': ['MANHATTAN VALLEY', 'UPPER WEST SIDE (96-116)'],
    'Upper East Side-Lenox Hill-Roosevelt Island': 'UPPER EAST SIDE (59-79)',
    'Upper East Side-Carnegie Hill': 'UPPER EAST SIDE (79-96)',
    'Upper East Side-Yorkville': None,
    'Morningside Heights': None,
    'Manhattanville-West Harlem': 'HARLEM-CENTRAL',
    'Hamilton Heights-Sugar Hill': 'HARLEM-WEST',
    'Harlem (South)': 'HARLEM-EAST',
    'Harlem (North)': 'HARLEM-UPPER',
    'East Harlem (South)': 'HARLEM-EAST',
    'East Harlem (North)': 'HARLEM-UPPER',
    "Randall's Island": None,
    'Washington Heights (South)': 'WASHINGTON HEIGHTS LOWER',
    'Washington Heights (North)': 'WASHINGTON HEIGHTS UPPER',
    'Inwood': 'INWOOD',
    'Highbridge Park': None,
    'Inwood Hill Park': None,
    'Central Park': None
}

pair_two = {
    'Financial District-Battery Park City': 'SOUTHBRIDGE',
    'Tribeca-Civic Center': 'TRIBECA',
    'The Battery-Governors Island-Ellis Island-Liberty Island': None,
    'SoHo-Little Italy-Hudson Square': ['SOHO', 'LITTLE ITALY'],
    'Greenwich Village': 'GREENWICH VILLAGE-CENTRAL',
    'West Village': 'GREENWICH VILLAGE-WEST',
    'Chinatown-Two Bridges': 'CHINATOWN',
    'Lower East Side': 'LOWER EAST SIDE',
    'East Village': 'EAST VILLAGE',
    'Chelsea-Hudson Yards': 'CHELSEA',
    "Hell's Kitchen": 'CLINTON',
    'Midtown South-Flatiron-Union Square': 'MIDTOWN WEST',
    'Midtown-Times Square': 'MIDTOWN EAST',
    'Stuyvesant Town-Peter Cooper Village': None,
    'Gramercy': 'GRAMERCY',
    'Murray Hill-Kips Bay': ['MURRAY HILL', 'KIPS BAY'],
    'East Midtown-Turtle Bay': 'MIDTOWN EAST',
    'United Nations': None,
    'Upper West Side-Lincoln Square': 'UPPER WEST SIDE (59-79)',
    'Upper West Side (Central)': 'UPPER WEST SIDE (79-96)',
    'Upper West Side-Manhattan Valley': 'UPPER WEST SIDE (96-116)',
    'Upper East Side-Lenox Hill-Roosevelt Island': 'UPPER EAST SIDE (59-79)',
    'Upper East Side-Carnegie Hill': 'UPPER EAST SIDE (79-96)',
    'Upper East Side-Yorkville': None,
    'Morningside Heights': None,
    'Manhattanville-West Harlem': 'HARLEM-CENTRAL',
    'Hamilton Heights-Sugar Hill': 'HARLEM-WEST',
    'Harlem (South)': 'HARLEM-EAST',
    'Harlem (North)': 'HARLEM-UPPER',
    'East Harlem (South)': 'HARLEM-EAST',
    'East Harlem (North)': 'HARLEM-UPPER',
    "Randall's Island": None,
    'Washington Heights (South)': 'WASHINGTON HEIGHTS LOWER',
    'Washington Heights (North)': 'WASHINGTON HEIGHTS UPPER',
    'Inwood': 'INWOOD',
    'Highbridge Park': None,
    'Inwood Hill Park': None,
    'Central Park': None
}
```

## The Model
The model usings 2010-2021 property sales data. The dataset is split 80/20 where the 80 is used for linear regression and the 20 is used for testing the model's accuracy. 
- independent_variables = ['YEAR', 'MEDIAN SALE PRICE', 'NUMBER OF SALES']
- dependent_variable = ['AVERAGE SALE PRICE']

The final predictions of prices are then displayed on a map. Example of Queens, darker regions are more expensive.
| One | Two | Three |
| --- | --- | --- |
| ![Queens One Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qOne.png?raw=true) | ![Queens Two Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qTwo.png?raw=true) | ![Queens Three Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qThree.png?raw=true) |

## Predictions compared to 2022
- [Manhattan](https://github.com/dongaCS/prediction-model-nyc/tree/main/manhattan)
- [Bronx](https://github.com/dongaCS/prediction-model-nyc/tree/main/bronx)
- [Brooklyn](https://github.com/dongaCS/prediction-model-nyc/tree/main/brooklyn)
- [Queens](https://github.com/dongaCS/prediction-model-nyc/tree/main/queens)
- [Staten Island](https://github.com/dongaCS/prediction-model-nyc/tree/main/staten%20island)

$Remarks$: Looking at the delta values of all five boroughs, the model seems to have under-predicted a majority of home values. Using the following fragment of code to look at errors, it becomes clear that there is bias within the data.
```
mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)

print(f'Mean Squared Error: {mse}')
print(f'Mean Absolute Error: {mae}')
```
Some solutions to this are to increase complexity by adding more variables (e.g., supply and demand, inflation, and other economic factors), use a more complex model (e.g., polynomial regression, decision trees, or neural networks), or get more data (we wait). 