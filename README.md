# Prediction Model - NYC

## Table of Contents
- [Overview](#Overview)
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
  - [Manhattan](#Manhattan)
  - [Queens](#Queens)

## Overview



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

The final predictions of prices are then displayed on a map. Example of Queens.
| One | Two | Three |
| --- | --- | --- |
| ![Queens One Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qOne.png?raw=true) | ![Queens Two Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qTwo.png?raw=true) | ![Queens Three Family Home](https://github.com/dongaCS/prediction-model-nyc/blob/main/queens/qThree.png?raw=true) |


## Predictions compared to 2022
### Manhattan
<div>
<table border="1" class="dataframe">
<thead>
<tr style="text-align: right;">
<th></th>
<th>NEIGHBORHOOD</th>
<th>TYPE OF HOME</th>
<th>AVERAGE SALE PRICE</th>
<th>PREDICT</th>
<th>DELTA</th>
</tr>
</thead>
<tbody>
<tr>
<th>263</th>
<td>ALPHABET CITY</td>
<td>01 ONE FAMILY HOMES</td>
<td>399000</td>
<td>5472405</td>
<td>-5073405</td>
</tr>
<tr>
<th>264</th>
<td>ALPHABET CITY</td>
<td>02 TWO FAMILY HOMES</td>
<td>2999999</td>
<td>3024999</td>
<td>-25000</td>
</tr>
<tr>
<th>265</th>
<td>CHELSEA</td>
<td>01 ONE FAMILY HOMES</td>
<td>7282668</td>
<td>4747177</td>
<td>2535491</td>
</tr>
<tr>
<th>266</th>
<td>CHELSEA</td>
<td>02 TWO FAMILY HOMES</td>
<td>5300000</td>
<td>6433367</td>
<td>-1133367</td>
</tr>
<tr>
<th>267</th>
<td>CHELSEA</td>
<td>03 THREE FAMILY HOMES</td>
<td>3975000</td>
<td>4813492</td>
<td>-838492</td>
</tr>
<tr>
<th>268</th>
<td>CLINTON</td>
<td>01 ONE FAMILY HOMES</td>
<td>3300000</td>
<td>3850000</td>
<td>-550000</td>
</tr>
<tr>
<th>269</th>
<td>CLINTON</td>
<td>02 TWO FAMILY HOMES</td>
<td>3415000</td>
<td>5699999</td>
<td>-2284999</td>
</tr>
<tr>
<th>270</th>
<td>GRAMERCY</td>
<td>02 TWO FAMILY HOMES</td>
<td>6666667</td>
<td>374876</td>
<td>6291791</td>
</tr>
<tr>
<th>271</th>
<td>GREENWICH VILLAGE-CENTRAL</td>
<td>01 ONE FAMILY HOMES</td>
<td>35000000</td>
<td>15584426</td>
<td>19415574</td>
</tr>
<tr>
<th>272</th>
<td>GREENWICH VILLAGE-CENTRAL</td>
<td>02 TWO FAMILY HOMES</td>
<td>6904167</td>
<td>10050000</td>
<td>-3145833</td>
</tr>
<tr>
<th>273</th>
<td>GREENWICH VILLAGE-CENTRAL</td>
<td>03 THREE FAMILY HOMES</td>
<td>9300000</td>
<td>7900000</td>
<td>1400000</td>
</tr>
<tr>
<th>274</th>
<td>GREENWICH VILLAGE-WEST</td>
<td>01 ONE FAMILY HOMES</td>
<td>9603400</td>
<td>8872090</td>
<td>731310</td>
</tr>
<tr>
<th>275</th>
<td>GREENWICH VILLAGE-WEST</td>
<td>02 TWO FAMILY HOMES</td>
<td>11422222</td>
<td>9025096</td>
<td>2397126</td>
</tr>
<tr>
<th>276</th>
<td>GREENWICH VILLAGE-WEST</td>
<td>03 THREE FAMILY HOMES</td>
<td>7481250</td>
<td>8385308</td>
<td>-904058</td>
</tr>
<tr>
<th>277</th>
<td>HARLEM-CENTRAL</td>
<td>01 ONE FAMILY HOMES</td>
<td>2960797</td>
<td>2173006</td>
<td>787791</td>
</tr>
<tr>
<th>278</th>
<td>HARLEM-CENTRAL</td>
<td>02 TWO FAMILY HOMES</td>
<td>2638077</td>
<td>2081572</td>
<td>556505</td>
</tr>
<tr>
<th>279</th>
<td>HARLEM-CENTRAL</td>
<td>03 THREE FAMILY HOMES</td>
<td>2238447</td>
<td>2939700</td>
<td>-701253</td>
</tr>
<tr>
<th>280</th>
<td>HARLEM-EAST</td>
<td>01 ONE FAMILY HOMES</td>
<td>4556900</td>
<td>3516790</td>
<td>1040110</td>
</tr>
<tr>
<th>281</th>
<td>HARLEM-EAST</td>
<td>02 TWO FAMILY HOMES</td>
<td>901221</td>
<td>1194493</td>
<td>-293272</td>
</tr>
<tr>
<th>282</th>
<td>HARLEM-EAST</td>
<td>03 THREE FAMILY HOMES</td>
<td>1300000</td>
<td>2040277</td>
<td>-740277</td>
</tr>
<tr>
<th>283</th>
<td>HARLEM-UPPER</td>
<td>01 ONE FAMILY HOMES</td>
<td>3039500</td>
<td>2664754</td>
<td>374746</td>
</tr>
<tr>
<th>284</th>
<td>HARLEM-UPPER</td>
<td>02 TWO FAMILY HOMES</td>
<td>2940083</td>
<td>1727698</td>
<td>1212385</td>
</tr>
<tr>
<th>285</th>
<td>HARLEM-UPPER</td>
<td>03 THREE FAMILY HOMES</td>
<td>1404333</td>
<td>1519494</td>
<td>-115161</td>
</tr>
<tr>
<th>286</th>
<td>INWOOD</td>
<td>01 ONE FAMILY HOMES</td>
<td>896000</td>
<td>725638</td>
<td>170362</td>
</tr>
<tr>
<th>287</th>
<td>INWOOD</td>
<td>02 TWO FAMILY HOMES</td>
<td>1222000</td>
<td>434910</td>
<td>787090</td>
</tr>
<tr>
<th>288</th>
<td>KIPS BAY</td>
<td>02 TWO FAMILY HOMES</td>
<td>3445325</td>
<td>9344475</td>
<td>-5899150</td>
</tr>
<tr>
<th>289</th>
<td>LITTLE ITALY</td>
<td>01 ONE FAMILY HOMES</td>
<td>5950000</td>
<td>9700000</td>
<td>-3750000</td>
</tr>
<tr>
<th>290</th>
<td>MANHATTAN VALLEY</td>
<td>01 ONE FAMILY HOMES</td>
<td>3725000</td>
<td>3750000</td>
<td>-25000</td>
</tr>
<tr>
<th>291</th>
<td>MANHATTAN VALLEY</td>
<td>02 TWO FAMILY HOMES</td>
<td>3525000</td>
<td>3500000</td>
<td>25000</td>
</tr>
<tr>
<th>292</th>
<td>MIDTOWN EAST</td>
<td>01 ONE FAMILY HOMES</td>
<td>6098358</td>
<td>6477447</td>
<td>-379089</td>
</tr>
<tr>
<th>293</th>
<td>MIDTOWN EAST</td>
<td>02 TWO FAMILY HOMES</td>
<td>7275000</td>
<td>4875000</td>
<td>2400000</td>
</tr>
<tr>
<th>294</th>
<td>MIDTOWN EAST</td>
<td>03 THREE FAMILY HOMES</td>
<td>4750000</td>
<td>319393</td>
<td>4430607</td>
</tr>
<tr>
<th>295</th>
<td>MURRAY HILL</td>
<td>01 ONE FAMILY HOMES</td>
<td>4393750</td>
<td>4200710</td>
<td>193040</td>
</tr>
<tr>
<th>296</th>
<td>MURRAY HILL</td>
<td>02 TWO FAMILY HOMES</td>
<td>4117667</td>
<td>5800102</td>
<td>-1682435</td>
</tr>
<tr>
<th>297</th>
<td>SOHO</td>
<td>01 ONE FAMILY HOMES</td>
<td>14800000</td>
<td>5892535</td>
<td>8907465</td>
</tr>
<tr>
<th>298</th>
<td>SOHO</td>
<td>02 TWO FAMILY HOMES</td>
<td>6000000</td>
<td>14242607</td>
<td>-8242607</td>
</tr>
<tr>
<th>299</th>
<td>SOHO</td>
<td>03 THREE FAMILY HOMES</td>
<td>7200000</td>
<td>-57017876</td>
<td>64217876</td>
</tr>
<tr>
<th>300</th>
<td>SOUTHBRIDGE</td>
<td>01 ONE FAMILY HOMES</td>
<td>3500000</td>
<td>5250000</td>
<td>-1750000</td>
</tr>
<tr>
<th>301</th>
<td>TRIBECA</td>
<td>01 ONE FAMILY HOMES</td>
<td>10223750</td>
<td>10731015</td>
<td>-507265</td>
</tr>
<tr>
<th>302</th>
<td>UPPER EAST SIDE (59-79)</td>
<td>01 ONE FAMILY HOMES</td>
<td>15599082</td>
<td>14150381</td>
<td>1448701</td>
</tr>
<tr>
<th>303</th>
<td>UPPER EAST SIDE (59-79)</td>
<td>02 TWO FAMILY HOMES</td>
<td>10523056</td>
<td>11494961</td>
<td>-971905</td>
</tr>
<tr>
<th>304</th>
<td>UPPER EAST SIDE (59-79)</td>
<td>03 THREE FAMILY HOMES</td>
<td>27000000</td>
<td>5350000</td>
<td>21650000</td>
</tr>
<tr>
<th>305</th>
<td>UPPER EAST SIDE (79-96)</td>
<td>01 ONE FAMILY HOMES</td>
<td>10650618</td>
<td>10310507</td>
<td>340111</td>
</tr>
<tr>
<th>306</th>
<td>UPPER EAST SIDE (79-96)</td>
<td>02 TWO FAMILY HOMES</td>
<td>8500000</td>
<td>10914781</td>
<td>-2414781</td>
</tr>
<tr>
<th>307</th>
<td>UPPER EAST SIDE (79-96)</td>
<td>03 THREE FAMILY HOMES</td>
<td>11400000</td>
<td>6616767</td>
<td>4783233</td>
</tr>
<tr>
<th>308</th>
<td>UPPER WEST SIDE (59-79)</td>
<td>01 ONE FAMILY HOMES</td>
<td>11226805</td>
<td>9953798</td>
<td>1273007</td>
</tr>
<tr>
<th>309</th>
<td>UPPER WEST SIDE (59-79)</td>
<td>02 TWO FAMILY HOMES</td>
<td>5715000</td>
<td>5775319</td>
<td>-60319</td>
</tr>
<tr>
<th>310</th>
<td>UPPER WEST SIDE (79-96)</td>
<td>01 ONE FAMILY HOMES</td>
<td>11082742</td>
<td>7792719</td>
<td>3290023</td>
</tr>
<tr>
<th>311</th>
<td>UPPER WEST SIDE (79-96)</td>
<td>02 TWO FAMILY HOMES</td>
<td>5026084</td>
<td>7085101</td>
<td>-2059017</td>
</tr>
<tr>
<th>312</th>
<td>UPPER WEST SIDE (79-96)</td>
<td>03 THREE FAMILY HOMES</td>
<td>6162500</td>
<td>7801180</td>
<td>-1638680</td>
</tr>
<tr>
<th>313</th>
<td>WASHINGTON HEIGHTS LOWER</td>
<td>01 ONE FAMILY HOMES</td>
<td>1981404</td>
<td>1441192</td>
<td>540212</td>
</tr>
<tr>
<th>314</th>
<td>WASHINGTON HEIGHTS LOWER</td>
<td>02 TWO FAMILY HOMES</td>
<td>1870742</td>
<td>902210</td>
<td>968532</td>
</tr>
<tr>
<th>315</th>
<td>WASHINGTON HEIGHTS LOWER</td>
<td>03 THREE FAMILY HOMES</td>
<td>1551335</td>
<td>2358749</td>
<td>-807414</td>
</tr>
<tr>
<th>316</th>
<td>WASHINGTON HEIGHTS UPPER</td>
<td>01 ONE FAMILY HOMES</td>
<td>1250000</td>
<td>1275000</td>
<td>-25000</td>
</tr>
</tbody>
</table>
</div>



### Queens
<div>
<table border="1" class="dataframe">
<thead>
<tr style="text-align: right;">
<th></th>
<th>NEIGHBORHOOD</th>
<th>TYPE OF HOME</th>
<th>AVERAGE SALE PRICE</th>
<th>PREDICT</th>
<th>DELTA</th>
</tr>
</thead>
<tbody>
<tr>
<th>317</th>
<td>AIRPORT LA GUARDIA</td>
<td>01 ONE FAMILY HOMES</td>
<td>852299</td>
<td>623949</td>
<td>228350</td>
</tr>
<tr>
<th>318</th>
<td>ARVERNE</td>
<td>01 ONE FAMILY HOMES</td>
<td>514303</td>
<td>364046</td>
<td>150257</td>
</tr>
<tr>
<th>319</th>
<td>ARVERNE</td>
<td>02 TWO FAMILY HOMES</td>
<td>757319</td>
<td>572543</td>
<td>184776</td>
</tr>
<tr>
<th>320</th>
<td>ARVERNE</td>
<td>03 THREE FAMILY HOMES</td>
<td>845698</td>
<td>600090</td>
<td>245608</td>
</tr>
<tr>
<th>321</th>
<td>ASTORIA</td>
<td>01 ONE FAMILY HOMES</td>
<td>1241253</td>
<td>947997</td>
<td>293256</td>
</tr>
<tr>
<th>322</th>
<td>ASTORIA</td>
<td>02 TWO FAMILY HOMES</td>
<td>1286027</td>
<td>1032495</td>
<td>253532</td>
</tr>
<tr>
<th>323</th>
<td>ASTORIA</td>
<td>03 THREE FAMILY HOMES</td>
<td>1424063</td>
<td>1157363</td>
<td>266700</td>
</tr>
<tr>
<th>324</th>
<td>BAYSIDE</td>
<td>01 ONE FAMILY HOMES</td>
<td>1059750</td>
<td>823687</td>
<td>236063</td>
</tr>
<tr>
<th>325</th>
<td>BAYSIDE</td>
<td>02 TWO FAMILY HOMES</td>
<td>1441296</td>
<td>981854</td>
<td>459442</td>
</tr>
<tr>
<th>326</th>
<td>BAYSIDE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1393111</td>
<td>1065671</td>
<td>327440</td>
</tr>
<tr>
<th>327</th>
<td>BEECHHURST</td>
<td>01 ONE FAMILY HOMES</td>
<td>1138755</td>
<td>970537</td>
<td>168218</td>
</tr>
<tr>
<th>328</th>
<td>BEECHHURST</td>
<td>02 TWO FAMILY HOMES</td>
<td>1386667</td>
<td>819094</td>
<td>567573</td>
</tr>
<tr>
<th>329</th>
<td>BELLE HARBOR</td>
<td>01 ONE FAMILY HOMES</td>
<td>1167630</td>
<td>837978</td>
<td>329652</td>
</tr>
<tr>
<th>330</th>
<td>BELLE HARBOR</td>
<td>02 TWO FAMILY HOMES</td>
<td>1103370</td>
<td>771547</td>
<td>331823</td>
</tr>
<tr>
<th>331</th>
<td>BELLE HARBOR</td>
<td>03 THREE FAMILY HOMES</td>
<td>780000</td>
<td>1141534</td>
<td>-361534</td>
</tr>
<tr>
<th>332</th>
<td>BELLEROSE</td>
<td>01 ONE FAMILY HOMES</td>
<td>754749</td>
<td>542833</td>
<td>211916</td>
</tr>
<tr>
<th>333</th>
<td>BELLEROSE</td>
<td>02 TWO FAMILY HOMES</td>
<td>843967</td>
<td>548441</td>
<td>295526</td>
</tr>
<tr>
<th>334</th>
<td>BELLEROSE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1113833</td>
<td>298466</td>
<td>815367</td>
</tr>
<tr>
<th>335</th>
<td>BREEZY POINT</td>
<td>01 ONE FAMILY HOMES</td>
<td>762229</td>
<td>629310</td>
<td>132919</td>
</tr>
<tr>
<th>336</th>
<td>BRIARWOOD</td>
<td>01 ONE FAMILY HOMES</td>
<td>813570</td>
<td>603735</td>
<td>209835</td>
</tr>
<tr>
<th>337</th>
<td>BRIARWOOD</td>
<td>02 TWO FAMILY HOMES</td>
<td>1031882</td>
<td>707473</td>
<td>324409</td>
</tr>
<tr>
<th>338</th>
<td>BRIARWOOD</td>
<td>03 THREE FAMILY HOMES</td>
<td>1187838</td>
<td>569417</td>
<td>618421</td>
</tr>
<tr>
<th>339</th>
<td>BROAD CHANNEL</td>
<td>01 ONE FAMILY HOMES</td>
<td>575026</td>
<td>377319</td>
<td>197707</td>
</tr>
<tr>
<th>340</th>
<td>CAMBRIA HEIGHTS</td>
<td>01 ONE FAMILY HOMES</td>
<td>610884</td>
<td>391840</td>
<td>219044</td>
</tr>
<tr>
<th>341</th>
<td>CAMBRIA HEIGHTS</td>
<td>02 TWO FAMILY HOMES</td>
<td>782273</td>
<td>465663</td>
<td>316610</td>
</tr>
<tr>
<th>342</th>
<td>CAMBRIA HEIGHTS</td>
<td>03 THREE FAMILY HOMES</td>
<td>510000</td>
<td>-</td>
<td>0</td>
</tr>
<tr>
<th>343</th>
<td>COLLEGE POINT</td>
<td>01 ONE FAMILY HOMES</td>
<td>875646</td>
<td>650472</td>
<td>225174</td>
</tr>
<tr>
<th>344</th>
<td>COLLEGE POINT</td>
<td>02 TWO FAMILY HOMES</td>
<td>1026910</td>
<td>820294</td>
<td>206616</td>
</tr>
<tr>
<th>345</th>
<td>COLLEGE POINT</td>
<td>03 THREE FAMILY HOMES</td>
<td>1099241</td>
<td>967363</td>
<td>131878</td>
</tr>
<tr>
<th>346</th>
<td>CORONA</td>
<td>01 ONE FAMILY HOMES</td>
<td>937720</td>
<td>847651</td>
<td>90069</td>
</tr>
<tr>
<th>347</th>
<td>CORONA</td>
<td>02 TWO FAMILY HOMES</td>
<td>1071234</td>
<td>786758</td>
<td>284476</td>
</tr>
<tr>
<th>348</th>
<td>CORONA</td>
<td>03 THREE FAMILY HOMES</td>
<td>1327392</td>
<td>969125</td>
<td>358267</td>
</tr>
<tr>
<th>349</th>
<td>DOUGLASTON</td>
<td>01 ONE FAMILY HOMES</td>
<td>1297075</td>
<td>1106420</td>
<td>190655</td>
</tr>
<tr>
<th>350</th>
<td>DOUGLASTON</td>
<td>02 TWO FAMILY HOMES</td>
<td>1450917</td>
<td>1001869</td>
<td>449048</td>
</tr>
<tr>
<th>351</th>
<td>EAST ELMHURST</td>
<td>01 ONE FAMILY HOMES</td>
<td>748024</td>
<td>541121</td>
<td>206903</td>
</tr>
<tr>
<th>352</th>
<td>EAST ELMHURST</td>
<td>02 TWO FAMILY HOMES</td>
<td>1053154</td>
<td>724883</td>
<td>328271</td>
</tr>
<tr>
<th>353</th>
<td>EAST ELMHURST</td>
<td>03 THREE FAMILY HOMES</td>
<td>1199091</td>
<td>737812</td>
<td>461279</td>
</tr>
<tr>
<th>354</th>
<td>ELMHURST</td>
<td>01 ONE FAMILY HOMES</td>
<td>1009013</td>
<td>731199</td>
<td>277814</td>
</tr>
<tr>
<th>355</th>
<td>ELMHURST</td>
<td>02 TWO FAMILY HOMES</td>
<td>1075002</td>
<td>878188</td>
<td>196814</td>
</tr>
<tr>
<th>356</th>
<td>ELMHURST</td>
<td>03 THREE FAMILY HOMES</td>
<td>1390827</td>
<td>988114</td>
<td>402713</td>
</tr>
<tr>
<th>357</th>
<td>FAR ROCKAWAY</td>
<td>01 ONE FAMILY HOMES</td>
<td>751618</td>
<td>526346</td>
<td>225272</td>
</tr>
<tr>
<th>358</th>
<td>FAR ROCKAWAY</td>
<td>02 TWO FAMILY HOMES</td>
<td>691935</td>
<td>-737277</td>
<td>1429212</td>
</tr>
<tr>
<th>359</th>
<td>FAR ROCKAWAY</td>
<td>03 THREE FAMILY HOMES</td>
<td>731831</td>
<td>441781</td>
<td>290050</td>
</tr>
<tr>
<th>360</th>
<td>FLORAL PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>799966</td>
<td>584012</td>
<td>215954</td>
</tr>
<tr>
<th>361</th>
<td>FLORAL PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>974000</td>
<td>740911</td>
<td>233089</td>
</tr>
<tr>
<th>362</th>
<td>FLORAL PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>995000</td>
<td>817500</td>
<td>177500</td>
</tr>
<tr>
<th>363</th>
<td>FLUSHING MEADOW PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>1572000</td>
<td>700000</td>
<td>872000</td>
</tr>
<tr>
<th>364</th>
<td>FLUSHING-NORTH</td>
<td>01 ONE FAMILY HOMES</td>
<td>1060208</td>
<td>824408</td>
<td>235800</td>
</tr>
<tr>
<th>365</th>
<td>FLUSHING-NORTH</td>
<td>02 TWO FAMILY HOMES</td>
<td>1274627</td>
<td>969920</td>
<td>304707</td>
</tr>
<tr>
<th>366</th>
<td>FLUSHING-NORTH</td>
<td>03 THREE FAMILY HOMES</td>
<td>1612256</td>
<td>1224357</td>
<td>387899</td>
</tr>
<tr>
<th>367</th>
<td>FLUSHING-SOUTH</td>
<td>01 ONE FAMILY HOMES</td>
<td>977874</td>
<td>733615</td>
<td>244259</td>
</tr>
<tr>
<th>368</th>
<td>FLUSHING-SOUTH</td>
<td>02 TWO FAMILY HOMES</td>
<td>1110390</td>
<td>798072</td>
<td>312318</td>
</tr>
<tr>
<th>369</th>
<td>FLUSHING-SOUTH</td>
<td>03 THREE FAMILY HOMES</td>
<td>1439035</td>
<td>1102248</td>
<td>336787</td>
</tr>
<tr>
<th>370</th>
<td>FOREST HILLS</td>
<td>01 ONE FAMILY HOMES</td>
<td>1513391</td>
<td>1163120</td>
<td>350271</td>
</tr>
<tr>
<th>371</th>
<td>FOREST HILLS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1310429</td>
<td>975491</td>
<td>334938</td>
</tr>
<tr>
<th>372</th>
<td>FOREST HILLS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1532667</td>
<td>1070399</td>
<td>462268</td>
</tr>
<tr>
<th>373</th>
<td>FRESH MEADOWS</td>
<td>01 ONE FAMILY HOMES</td>
<td>1098746</td>
<td>838059</td>
<td>260687</td>
</tr>
<tr>
<th>374</th>
<td>FRESH MEADOWS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1058667</td>
<td>860269</td>
<td>198398</td>
</tr>
<tr>
<th>375</th>
<td>GLEN OAKS</td>
<td>01 ONE FAMILY HOMES</td>
<td>810995</td>
<td>586025</td>
<td>224970</td>
</tr>
<tr>
<th>376</th>
<td>GLEN OAKS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1513000</td>
<td>859112</td>
<td>653888</td>
</tr>
<tr>
<th>377</th>
<td>GLEN OAKS</td>
<td>03 THREE FAMILY HOMES</td>
<td>810000</td>
<td>998000</td>
<td>-188000</td>
</tr>
<tr>
<th>378</th>
<td>GLENDALE</td>
<td>01 ONE FAMILY HOMES</td>
<td>782107</td>
<td>593729</td>
<td>188378</td>
</tr>
<tr>
<th>379</th>
<td>GLENDALE</td>
<td>02 TWO FAMILY HOMES</td>
<td>883612</td>
<td>702200</td>
<td>181412</td>
</tr>
<tr>
<th>380</th>
<td>GLENDALE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1197091</td>
<td>865690</td>
<td>331401</td>
</tr>
<tr>
<th>381</th>
<td>HAMMELS</td>
<td>01 ONE FAMILY HOMES</td>
<td>610833</td>
<td>385785</td>
<td>225048</td>
</tr>
<tr>
<th>382</th>
<td>HAMMELS</td>
<td>02 TWO FAMILY HOMES</td>
<td>915586</td>
<td>431461</td>
<td>484125</td>
</tr>
<tr>
<th>383</th>
<td>HAMMELS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1036358</td>
<td>373471</td>
<td>662887</td>
</tr>
<tr>
<th>384</th>
<td>HILLCREST</td>
<td>01 ONE FAMILY HOMES</td>
<td>825025</td>
<td>622030</td>
<td>202995</td>
</tr>
<tr>
<th>385</th>
<td>HILLCREST</td>
<td>02 TWO FAMILY HOMES</td>
<td>812600</td>
<td>602182</td>
<td>210418</td>
</tr>
<tr>
<th>386</th>
<td>HOLLIS</td>
<td>01 ONE FAMILY HOMES</td>
<td>713610</td>
<td>422923</td>
<td>290687</td>
</tr>
<tr>
<th>387</th>
<td>HOLLIS</td>
<td>02 TWO FAMILY HOMES</td>
<td>762625</td>
<td>490407</td>
<td>272218</td>
</tr>
<tr>
<th>388</th>
<td>HOLLIS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1001667</td>
<td>587840</td>
<td>413827</td>
</tr>
<tr>
<th>389</th>
<td>HOLLIS HILLS</td>
<td>01 ONE FAMILY HOMES</td>
<td>1189268</td>
<td>907178</td>
<td>282090</td>
</tr>
<tr>
<th>390</th>
<td>HOLLIS HILLS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1130000</td>
<td>675000</td>
<td>455000</td>
</tr>
<tr>
<th>391</th>
<td>HOLLISWOOD</td>
<td>01 ONE FAMILY HOMES</td>
<td>1218311</td>
<td>856923</td>
<td>361388</td>
</tr>
<tr>
<th>392</th>
<td>HOLLISWOOD</td>
<td>02 TWO FAMILY HOMES</td>
<td>995000</td>
<td>680005</td>
<td>314995</td>
</tr>
<tr>
<th>393</th>
<td>HOWARD BEACH</td>
<td>01 ONE FAMILY HOMES</td>
<td>839467</td>
<td>580843</td>
<td>258624</td>
</tr>
<tr>
<th>394</th>
<td>HOWARD BEACH</td>
<td>02 TWO FAMILY HOMES</td>
<td>921155</td>
<td>660504</td>
<td>260651</td>
</tr>
<tr>
<th>395</th>
<td>HOWARD BEACH</td>
<td>03 THREE FAMILY HOMES</td>
<td>1037600</td>
<td>729691</td>
<td>307909</td>
</tr>
<tr>
<th>396</th>
<td>JACKSON HEIGHTS</td>
<td>01 ONE FAMILY HOMES</td>
<td>904369</td>
<td>652801</td>
<td>251568</td>
</tr>
<tr>
<th>397</th>
<td>JACKSON HEIGHTS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1110572</td>
<td>784127</td>
<td>326445</td>
</tr>
<tr>
<th>398</th>
<td>JACKSON HEIGHTS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1280151</td>
<td>941461</td>
<td>338690</td>
</tr>
<tr>
<th>399</th>
<td>JAMAICA</td>
<td>01 ONE FAMILY HOMES</td>
<td>784160</td>
<td>499241</td>
<td>284919</td>
</tr>
<tr>
<th>400</th>
<td>JAMAICA</td>
<td>02 TWO FAMILY HOMES</td>
<td>945266</td>
<td>701223</td>
<td>244043</td>
</tr>
<tr>
<th>401</th>
<td>JAMAICA</td>
<td>03 THREE FAMILY HOMES</td>
<td>1166200</td>
<td>798559</td>
<td>367641</td>
</tr>
<tr>
<th>402</th>
<td>JAMAICA BAY</td>
<td>01 ONE FAMILY HOMES</td>
<td>633819</td>
<td>370822</td>
<td>262997</td>
</tr>
<tr>
<th>403</th>
<td>JAMAICA BAY</td>
<td>02 TWO FAMILY HOMES</td>
<td>602408</td>
<td>480261</td>
<td>122147</td>
</tr>
<tr>
<th>404</th>
<td>JAMAICA ESTATES</td>
<td>01 ONE FAMILY HOMES</td>
<td>1335184</td>
<td>1091073</td>
<td>244111</td>
</tr>
<tr>
<th>405</th>
<td>JAMAICA ESTATES</td>
<td>02 TWO FAMILY HOMES</td>
<td>1013333</td>
<td>962461</td>
<td>50872</td>
</tr>
<tr>
<th>406</th>
<td>JAMAICA HILLS</td>
<td>01 ONE FAMILY HOMES</td>
<td>805213</td>
<td>652560</td>
<td>152653</td>
</tr>
<tr>
<th>407</th>
<td>JAMAICA HILLS</td>
<td>02 TWO FAMILY HOMES</td>
<td>793867</td>
<td>663408</td>
<td>130459</td>
</tr>
<tr>
<th>408</th>
<td>JAMAICA HILLS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1210000</td>
<td>776687</td>
<td>433313</td>
</tr>
<tr>
<th>409</th>
<td>KEW GARDENS</td>
<td>01 ONE FAMILY HOMES</td>
<td>1150974</td>
<td>818566</td>
<td>332408</td>
</tr>
<tr>
<th>410</th>
<td>KEW GARDENS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1082000</td>
<td>654126</td>
<td>427874</td>
</tr>
<tr>
<th>411</th>
<td>KEW GARDENS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1262193</td>
<td>897892</td>
<td>364301</td>
</tr>
<tr>
<th>412</th>
<td>LAURELTON</td>
<td>01 ONE FAMILY HOMES</td>
<td>631955</td>
<td>406345</td>
<td>225610</td>
</tr>
<tr>
<th>413</th>
<td>LAURELTON</td>
<td>02 TWO FAMILY HOMES</td>
<td>796302</td>
<td>499555</td>
<td>296747</td>
</tr>
<tr>
<th>414</th>
<td>LITTLE NECK</td>
<td>01 ONE FAMILY HOMES</td>
<td>1091274</td>
<td>886603</td>
<td>204671</td>
</tr>
<tr>
<th>415</th>
<td>LITTLE NECK</td>
<td>02 TWO FAMILY HOMES</td>
<td>1312903</td>
<td>1076322</td>
<td>236581</td>
</tr>
<tr>
<th>416</th>
<td>LONG ISLAND CITY</td>
<td>01 ONE FAMILY HOMES</td>
<td>881400</td>
<td>984841</td>
<td>-103441</td>
</tr>
<tr>
<th>417</th>
<td>LONG ISLAND CITY</td>
<td>02 TWO FAMILY HOMES</td>
<td>1612910</td>
<td>1347625</td>
<td>265285</td>
</tr>
<tr>
<th>418</th>
<td>LONG ISLAND CITY</td>
<td>03 THREE FAMILY HOMES</td>
<td>1473083</td>
<td>1569330</td>
<td>-96247</td>
</tr>
<tr>
<th>419</th>
<td>MASPETH</td>
<td>01 ONE FAMILY HOMES</td>
<td>789655</td>
<td>582343</td>
<td>207312</td>
</tr>
<tr>
<th>420</th>
<td>MASPETH</td>
<td>02 TWO FAMILY HOMES</td>
<td>953636</td>
<td>696292</td>
<td>257344</td>
</tr>
<tr>
<th>421</th>
<td>MASPETH</td>
<td>03 THREE FAMILY HOMES</td>
<td>1229271</td>
<td>890902</td>
<td>338369</td>
</tr>
<tr>
<th>422</th>
<td>MIDDLE VILLAGE</td>
<td>01 ONE FAMILY HOMES</td>
<td>828213</td>
<td>649886</td>
<td>178327</td>
</tr>
<tr>
<th>423</th>
<td>MIDDLE VILLAGE</td>
<td>02 TWO FAMILY HOMES</td>
<td>987359</td>
<td>788839</td>
<td>198520</td>
</tr>
<tr>
<th>424</th>
<td>MIDDLE VILLAGE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1305251</td>
<td>857741</td>
<td>447510</td>
</tr>
<tr>
<th>425</th>
<td>NEPONSIT</td>
<td>01 ONE FAMILY HOMES</td>
<td>1271242</td>
<td>997102</td>
<td>274140</td>
</tr>
<tr>
<th>426</th>
<td>OAKLAND GARDENS</td>
<td>01 ONE FAMILY HOMES</td>
<td>1076223</td>
<td>831129</td>
<td>245094</td>
</tr>
<tr>
<th>427</th>
<td>OAKLAND GARDENS</td>
<td>02 TWO FAMILY HOMES</td>
<td>1813079</td>
<td>1315409</td>
<td>497670</td>
</tr>
<tr>
<th>428</th>
<td>OAKLAND GARDENS</td>
<td>03 THREE FAMILY HOMES</td>
<td>1303633</td>
<td>967442</td>
<td>336191</td>
</tr>
<tr>
<th>429</th>
<td>OZONE PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>664503</td>
<td>443308</td>
<td>221195</td>
</tr>
<tr>
<th>430</th>
<td>OZONE PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>846317</td>
<td>557015</td>
<td>289302</td>
</tr>
<tr>
<th>431</th>
<td>OZONE PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>945980</td>
<td>637368</td>
<td>308612</td>
</tr>
<tr>
<th>432</th>
<td>QUEENS VILLAGE</td>
<td>01 ONE FAMILY HOMES</td>
<td>652472</td>
<td>445440</td>
<td>207032</td>
</tr>
<tr>
<th>433</th>
<td>QUEENS VILLAGE</td>
<td>02 TWO FAMILY HOMES</td>
<td>761764</td>
<td>551767</td>
<td>209997</td>
</tr>
<tr>
<th>434</th>
<td>QUEENS VILLAGE</td>
<td>03 THREE FAMILY HOMES</td>
<td>902500</td>
<td>735186</td>
<td>167314</td>
</tr>
<tr>
<th>435</th>
<td>REGO PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>941065</td>
<td>862030</td>
<td>79035</td>
</tr>
<tr>
<th>436</th>
<td>REGO PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>1311077</td>
<td>921543</td>
<td>389534</td>
</tr>
<tr>
<th>437</th>
<td>REGO PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>1423833</td>
<td>1168854</td>
<td>254979</td>
</tr>
<tr>
<th>438</th>
<td>RICHMOND HILL</td>
<td>01 ONE FAMILY HOMES</td>
<td>676543</td>
<td>452589</td>
<td>223954</td>
</tr>
<tr>
<th>439</th>
<td>RICHMOND HILL</td>
<td>02 TWO FAMILY HOMES</td>
<td>839231</td>
<td>525082</td>
<td>314149</td>
</tr>
<tr>
<th>440</th>
<td>RICHMOND HILL</td>
<td>03 THREE FAMILY HOMES</td>
<td>969008</td>
<td>615074</td>
<td>353934</td>
</tr>
<tr>
<th>441</th>
<td>RIDGEWOOD</td>
<td>01 ONE FAMILY HOMES</td>
<td>856102</td>
<td>738022</td>
<td>118080</td>
</tr>
<tr>
<th>442</th>
<td>RIDGEWOOD</td>
<td>02 TWO FAMILY HOMES</td>
<td>1100603</td>
<td>852615</td>
<td>247988</td>
</tr>
<tr>
<th>443</th>
<td>RIDGEWOOD</td>
<td>03 THREE FAMILY HOMES</td>
<td>1258391</td>
<td>959137</td>
<td>299254</td>
</tr>
<tr>
<th>444</th>
<td>ROCKAWAY PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>824667</td>
<td>518846</td>
<td>305821</td>
</tr>
<tr>
<th>445</th>
<td>ROCKAWAY PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>998333</td>
<td>584104</td>
<td>414229</td>
</tr>
<tr>
<th>446</th>
<td>ROCKAWAY PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>829500</td>
<td>481712</td>
<td>347788</td>
</tr>
<tr>
<th>447</th>
<td>ROSEDALE</td>
<td>01 ONE FAMILY HOMES</td>
<td>644633</td>
<td>403416</td>
<td>241217</td>
</tr>
<tr>
<th>448</th>
<td>ROSEDALE</td>
<td>02 TWO FAMILY HOMES</td>
<td>843302</td>
<td>545808</td>
<td>297494</td>
</tr>
<tr>
<th>449</th>
<td>ROSEDALE</td>
<td>03 THREE FAMILY HOMES</td>
<td>903541</td>
<td>510710</td>
<td>392831</td>
</tr>
<tr>
<th>450</th>
<td>SO. JAMAICA-BAISLEY PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>584631</td>
<td>361827</td>
<td>222804</td>
</tr>
<tr>
<th>451</th>
<td>SO. JAMAICA-BAISLEY PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>761424</td>
<td>527343</td>
<td>234081</td>
</tr>
<tr>
<th>452</th>
<td>SO. JAMAICA-BAISLEY PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>550000</td>
<td>522468</td>
<td>27532</td>
</tr>
<tr>
<th>453</th>
<td>SOUTH JAMAICA</td>
<td>01 ONE FAMILY HOMES</td>
<td>573821</td>
<td>372264</td>
<td>201557</td>
</tr>
<tr>
<th>454</th>
<td>SOUTH JAMAICA</td>
<td>02 TWO FAMILY HOMES</td>
<td>762773</td>
<td>463771</td>
<td>299002</td>
</tr>
<tr>
<th>455</th>
<td>SOUTH JAMAICA</td>
<td>03 THREE FAMILY HOMES</td>
<td>773464</td>
<td>518138</td>
<td>255326</td>
</tr>
<tr>
<th>456</th>
<td>SOUTH OZONE PARK</td>
<td>01 ONE FAMILY HOMES</td>
<td>653423</td>
<td>430596</td>
<td>222827</td>
</tr>
<tr>
<th>457</th>
<td>SOUTH OZONE PARK</td>
<td>02 TWO FAMILY HOMES</td>
<td>848425</td>
<td>544444</td>
<td>303981</td>
</tr>
<tr>
<th>458</th>
<td>SOUTH OZONE PARK</td>
<td>03 THREE FAMILY HOMES</td>
<td>953920</td>
<td>451845</td>
<td>502075</td>
</tr>
<tr>
<th>459</th>
<td>SPRINGFIELD GARDENS</td>
<td>01 ONE FAMILY HOMES</td>
<td>616412</td>
<td>378196</td>
<td>238216</td>
</tr>
<tr>
<th>460</th>
<td>SPRINGFIELD GARDENS</td>
<td>02 TWO FAMILY HOMES</td>
<td>818832</td>
<td>514011</td>
<td>304821</td>
</tr>
<tr>
<th>461</th>
<td>SPRINGFIELD GARDENS</td>
<td>03 THREE FAMILY HOMES</td>
<td>746600</td>
<td>372007</td>
<td>374593</td>
</tr>
<tr>
<th>462</th>
<td>ST. ALBANS</td>
<td>01 ONE FAMILY HOMES</td>
<td>635420</td>
<td>394308</td>
<td>241112</td>
</tr>
<tr>
<th>463</th>
<td>ST. ALBANS</td>
<td>02 TWO FAMILY HOMES</td>
<td>749394</td>
<td>482620</td>
<td>266774</td>
</tr>
<tr>
<th>464</th>
<td>ST. ALBANS</td>
<td>03 THREE FAMILY HOMES</td>
<td>630750</td>
<td>512452</td>
<td>118298</td>
</tr>
<tr>
<th>465</th>
<td>SUNNYSIDE</td>
<td>01 ONE FAMILY HOMES</td>
<td>1227400</td>
<td>849663</td>
<td>377737</td>
</tr>
<tr>
<th>466</th>
<td>SUNNYSIDE</td>
<td>02 TWO FAMILY HOMES</td>
<td>1402531</td>
<td>938400</td>
<td>464131</td>
</tr>
<tr>
<th>467</th>
<td>SUNNYSIDE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1391667</td>
<td>1018138</td>
<td>373529</td>
</tr>
<tr>
<th>468</th>
<td>WHITESTONE</td>
<td>01 ONE FAMILY HOMES</td>
<td>1198049</td>
<td>874350</td>
<td>323699</td>
</tr>
<tr>
<th>469</th>
<td>WHITESTONE</td>
<td>02 TWO FAMILY HOMES</td>
<td>1418824</td>
<td>989336</td>
<td>429488</td>
</tr>
<tr>
<th>470</th>
<td>WHITESTONE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1809000</td>
<td>1155263</td>
<td>653737</td>
</tr>
<tr>
<th>471</th>
<td>WOODHAVEN</td>
<td>01 ONE FAMILY HOMES</td>
<td>672759</td>
<td>458245</td>
<td>214514</td>
</tr>
<tr>
<th>472</th>
<td>WOODHAVEN</td>
<td>02 TWO FAMILY HOMES</td>
<td>880278</td>
<td>633995</td>
<td>246283</td>
</tr>
<tr>
<th>473</th>
<td>WOODHAVEN</td>
<td>03 THREE FAMILY HOMES</td>
<td>876953</td>
<td>656900</td>
<td>220053</td>
</tr>
<tr>
<th>474</th>
<td>WOODSIDE</td>
<td>01 ONE FAMILY HOMES</td>
<td>825112</td>
<td>679765</td>
<td>145347</td>
</tr>
<tr>
<th>475</th>
<td>WOODSIDE</td>
<td>02 TWO FAMILY HOMES</td>
<td>1127911</td>
<td>876096</td>
<td>251815</td>
</tr>
<tr>
<th>476</th>
<td>WOODSIDE</td>
<td>03 THREE FAMILY HOMES</td>
<td>1196846</td>
<td>1089998</td>
<td>106848</td>
</tr>
</tbody>
</table>
</div>
