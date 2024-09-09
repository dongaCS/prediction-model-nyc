# Prediction Model - NYC


## Table of Contents
- [Setting up Notebook](#Setting-up-Notebook)
- [The Data](#The-Data)
  - Property Sales
  - Neighborhood Shapes
- [Predictions](#Predictions)
  - Manhattan

## Setting up Notebook
This project will use Python [3.12.4 64-bit](https://www.python.org/downloads/)
- `python3 -m venv .venv`
- `pip3 install notebook pandas scikit-learn geopandas matplotlib`
- `jupyter notebook`
    - opens notebook in browser
    - links to notebook is in terminal 
- `pip3 freeze > requirements.txt`

## The Data
[Source](https://opendata.cityofnewyork.us/data/)
### Property Sales
| BOROUGH | NEIGHBORHOOD | TYPE OF HOME | NUMBER OF SALES | LOWEST SALE PRICE | AVERAGE SALE PRICE | MEDIAN SALE PRICE | HIGHEST SALE PRICE | YEAR |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| String | String | String | Int | Int | Int | Int | Int | String |

#### Cleaning of Data
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

## Predictions
### Manhattan
<div class="jp-OutputArea jp-Cell-outputArea">
<div class="jp-OutputArea-child jp-OutputArea-executeResult">
<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html" tabindex="0">
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
</div>
</div>
</div>
</div>
</div>