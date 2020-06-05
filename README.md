# Open COVID-19 Dataset

This repository contains datasets of daily time-series data related to COVID-19 for 30+ countries
around the world. For most countries, the data is at the spatial resolution of states/provinces,
although for US, UK, NL and CO, it is at the finer resolution of county/municipality. All regions
are assigned a unique key, which resolves discrepancies between ISO/ NUTS/ FIPS codes, etc.

There are multiple types of data:
* Outcome data `Y(i,t)`, such as cases, deaths, tests, for regions i and time t
* Static covariate data `X(i)`, such as population size, GDP, latitude/ longitude
* Dynamic covariate data `X(i,t)`, such as mobility, weather
* Dynamic interventional data `A(i,t)`, such as government lockdowns

The data is drawn from multiple sources, as listed [below](#sources-of-data).

The data is stored in separate csv/ json files, which can be easily merged due to the use of
consistent geographic (and temporal) keys.

| Table | Keys<sup>1</sup> | Content | URL | Source<sup>2</sup> |
| ----- | ---------------- | ------- | --- | ------------------ |
| [Master](#master) | `[key][date]` | Flat table with records from all other tables joined by `key` and `date` | [master.csv](https://open-covid-19.github.io/data/v2/master.csv) | All tables below |
| [Index](#index) | `[key]` | Various names and codes, useful for joining with other datasets | [index.csv](https://open-covid-19.github.io/data/v2/index.csv), [index.json](https://open-covid-19.github.io/data/v2/index.json) | Wikidata, DataCommons |
| [Demographics](#demographics) | `[key]` | Various (current<sup>3</sup>) population statistics | [demographics.csv](https://open-covid-19.github.io/data/v2/demographics.csv), [demographics.json](https://open-covid-19.github.io/data/v2/demographics.json) | Wikidata, DataCommons |
| [Economy](#economy) | `[key]` | Various (current<sup>3</sup>) economic indicators | [economy.csv](https://open-covid-19.github.io/data/v2/economy.csv), [economy.json](https://open-covid-19.github.io/data/v2/economy.json) | Wikidata, DataCommons |
| [Epidemiology](#epidemiology) | `[key][date]` | COVID-19 cases, deaths, recoveries and tests | [epidemiology.csv](https://open-covid-19.github.io/data/v2/epidemiology.csv), [epidemiology.json](https://open-covid-19.github.io/data/v2/epidemiology.json) | Various<sup>2</sup> |
| [Geography](#geography) | `[key]` | Geographical information about the region | [geography.csv](https://open-covid-19.github.io/data/v2/geography.csv), [geography.json](https://open-covid-19.github.io/data/v2/geography.json) | Wikidata |
| [Health](#health) | `[key]` | Health indicators for the region | [health.csv](https://open-covid-19.github.io/data/v2/health.csv), [health.json](https://open-covid-19.github.io/data/v2/geography.json) | Wikidata, WorldBank |
| [Mobility](#mobility) | `[key][date]` | Various metrics related to the movement of people | [mobility.csv](https://open-covid-19.github.io/data/v2/mobility.csv), [google-mobility.json](https://open-covid-19.github.io/data/v2/google-mobility.json) | Google, Apple |
| [Oxford Government Response](#oxford-government-response) | `[key][date]` | Government interventions and their relative stringency | [oxford-government-response.csv](https://open-covid-19.github.io/data/v2/oxford-government-response.csv), [oxford-government-response.json](https://open-covid-19.github.io/data/v2/oxford-government-response.json) | University of Oxford |
| [Weather](#weather) | `[key][date]` | Dated meteorological information for each region | [weather.csv](https://open-covid-19.github.io/data/v2/weather.csv), [weather.json](https://open-covid-19.github.io/data/v2/weather.json) | NOAA |
| [WorldBank](#worldbank) | `[key]` | Latest record for each indicator from WorldBank for all reporting countries | [worldbank.csv](https://open-covid-19.github.io/data/v2/worldbank.csv), [worldbank.json](https://open-covid-19.github.io/data/v2/worldbank.json) | WorldBank |

<sup>1</sup> `key` is a unique string for the specific geographical region built from a combination
of codes such as `ISO 3166`, `NUTS`, `FIPS` and other local equivalents.\
<sup>2</sup> Refer to the [data sources](#sources-of-data) for specifics about each data source and
the associated terms of use.\
<sup>3</sup> Datasets without a `date` column contain the most recently reported information for
each datapoint to date.

For more information about how to use these files see the section about
[using the data](#use-the-data), and for more details about each dataset see the section about
[understanding the data](#understand-the-data).



## Why another dataset?

There are many other public COVID-19 datasets. However, we believe this dataset is unique in the way
that it merges multiple global sources, at a fine spatial resolution, using a consistent set of
region keys. We hope this will make it easier for researchers to use. We are also very transparent
about the [data sources](#sources-of-data), and the [code](src/README.md) for ingesting and merging
the data is easy to understand and modify.


## Explore the data

|     |     |
| --- | --- |
| A simple visualization tool was built to explore the Open COVID-19 datasets, the [Open COVID-19 Explorer][12]: [![](https://github.com/open-covid-19/explorer/raw/master/screenshots/explorer.png)][12] | If you want to see [interactive charts with a unique UX][14], don't miss what [@Mahks](https://github.com/Mahks) built using the Open COVID-19 dataset: [![](https://i.imgur.com/cIODOtp.png)][14] |
| You can also check out the great work of [@quixote79](https://github.com/quixote79), [a MapBox-powered interactive map site][13]: [![](https://i.imgur.com/nFwxJId.png)][13] | Experience [clean, clear graphs with smooth animations][15] thanks to the work of [@jmullo](https://github.com/jmullo): [![](https://i.imgur.com/xdCzsUO.png)][15] |
| Become an armchair epidemiologist with the [COVID-19 timeline simulation tool][19] built by [@LeviticusMB](https://github.com/LeviticusMB): [![](https://i.imgur.com/4iWaP7E.png)][19] | Whether you want an interactive map, compare stats or look at charts, [@saadmas](https://github.com/saadmas) has you covered with a [COVID-19 Daily Tracking site][20]: [![](https://i.imgur.com/rAJvLSI.png)][20] |
| Compare per-million data at [Omnimodel][21] thanks to [@OmarJay1](https://github.com/OmarJay1): [![](https://i.imgur.com/RG7ZKXp.png)][21] |  |



## Use the data

The data is available as CSV and JSON files, which are published in Github Pages so they can be
served directly to Javascript applications without the need of a proxy to set the correct headers
for CORS and content type. Even if you only want the CSV files, using the URL served by Github Pages
is preferred in order to avoid caching issues and potential, future breaking changes.

For the purpose of making the data as easy to use as possible, there is a [master](#master) table
which contains the columns of all other tables joined by `key` and `date`. However,
performance-wise, it may be better to download the data separately and join the tables locally.

Each table has a full version as well as subsets with only the last 30, 14, 7 and 1 days of data.
The full version is accessible at the URL described [in the table above](#open-covid-19-dataset).
The subsets can be found by appending the number of days to the path. For example, the subsets of
the master table are available at the following locations:
* Full version: https://open-covid-19.github.io/data/v2/master.csv
* Last 30 days: https://open-covid-19.github.io/data/v2/30/master.csv
* Last 14 days: https://open-covid-19.github.io/data/v2/14/master.csv
* Last 7 days: https://open-covid-19.github.io/data/v2/7/master.csv
* Latest: https://open-covid-19.github.io/data/v2/latest/master.csv

Note that the `latest` version contains the last non-null record for each key, whereas all others
contain the last `N` days of data (all of which could be null for some keys).

If you are trying to use this data alongside your own datasets, then you can use the [Index](#index)
table to get access to the ISO 3166 / NUTS / FIPS code, although administrative subdivisions are
not consistent among all reporting regions. For example, for the intra-country reporting, some EU
countries use NUTS2, others NUTS3 and many ISO 3166-2 codes.

You can find several examples in the [examples subfolder](examples) with code showcasing how to load
and analyze the data for several programming environments. If you want the short version, here are a
few snippets to get started.

### Google Colab
You can use Google Colab if you want to run your analysis without having to install anything in your
computer, simply go to this URL: https://colab.research.google.com/github/open-covid-19/data.

### R
If you prefer R, then this is all you need to do to load the epidemiology data:
```R
data <- read.csv("https://open-covid-19.github.io/data/v2/master.csv")
```

### Python
In Python, you need to have the package `pandas` installed to get started:
```python
import pandas
data = pandas.read_csv("https://open-covid-19.github.io/data/v2/master.csv")
```

### jQuery
Loading the JSON file using jQuery can be done directly from the output folder,
this code snippet loads the master table into the `data` variable:
```javascript
$.getJSON("https://open-covid-19.github.io/data/v2/master.json", data => { ... }
```

### Powershell
You can also use Powershell to get the latest data for a country directly from
the command line, for example to query the latest data for Australia:
```powershell
Invoke-WebRequest 'https://open-covid-19.github.io/data/v2/latest/master.csv' | ConvertFrom-Csv | `
    where Key -eq 'AU' | select country_name,date,total_confirmed,total_deceased,total_recovered
```



## Understand the data

Make sure that you are using the URL [linked at the table above](#open-covid-19-dataset) and not the
raw GitHub file, the latter is subject to change at any moment in non-compatible ways, and due to
the configuration of GitHub's raw file server you may run into potential caching issues.

Missing values will be represented as nulls, whereas zeroes are used when a true value of zero is
reported.

### Master
Flat table with records from all other tables joined by `key` and `date`. See below for information
about all the different tables and columns.

### Index
Non-temporal data related to countries and regions. It includes keys, codes and names for each
region, which is helpful for displaying purposes or when merging with other data:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | US_CA_06001 |
| **wikidata** | `string` | Wikidata ID corresponding to this key | Q107146 |
| **country_code** | `string` | ISO 3166-1 alphanumeric 2-letter code of the country | US |
| **country_name** | `string` | American English name of the country, subject to change | United States of America |
| **subregion1_code** | `string` | (Optional) ISO 3166-2 or NUTS 2/3 code of the subregion | CA |
| **subregion1_name** | `string` | (Optional) American English name of the subregion, subject to change | California |
| **subregion2_code** | `string` | (Optional) FIPS code of the county (or local equivalent) | 06001 |
| **subregion2_name** | `string` | (Optional) American English name of the county (or local equivalent), subject to change | Alameda County |
| **3166-1-alpha-2** | `string` | ISO 3166-1 alphanumeric 2-letter code of the country | US |
| **3166-1-alpha-3** | `string` | ISO 3166-1 alphanumeric 3-letter code of the country | USA |
| **aggregation_level** | `integer` `[0-2]` | Level at which data is aggregated, i.e. country, state/province or county level | 2 |

### Demographics
Information related to the population demographics for each region:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | KR |
| **population** | `integer` | Total count of humans | 51606633 |
| **male_population** | `integer` | Total count of males | 25846211 |
| **female_population** | `integer` | Total count of females | 25760422 |
| **rural_population** | `integer` | Population in a rural area | 9568386 |
| **urban_population** | `integer` | Population in an urban area | 42038247 |
| **largest_city_population** | `integer` | Population in the largest city of the region | 9963497 |
| **clustered_population** | `integer` | Population in urban agglomerations of more than 1 million | 25893097 |
| **population_density** | `double` `[persons per squared kilometer]` | Population per squared kilometer of land area | 529.3585 |
| **human_development_index** | `double` `[0-1]` | Composite index of life expectancy, education, and per capita income indicators | 0.903 |

### Economy
Information related to the economic development for each region:

| Name | Name | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | CN_HB |
| **gdp** | `integer` `[USD]` | Gross domestic product; monetary value of all finished goods and services | 24450604878 |
| **gdp_per_capita** | `integer` `[USD]` | Gross domestic product divided by total population | 1148 |
| **human_capital_index** | `double` `[0-1]` | Mobilization of the economic and professional potential of citizens | 0.765 |

### Epidemiology
Information related to the COVID-19 infections for each date-region pair:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **date** | `string` | ISO 8601 date (YYYY-MM-DD) of the datapoint | 2020-03-30 |
| **key** | `string` | Unique string identifying the region | CN_HB |
| **new_confirmed\*** | `integer` | Count of new cases confirmed after positive test on this date | 34 |
| **new_deceased\*** | `integer` | Count of new deaths from a positive COVID-19 case on this date | 2 |
| **new_recovered\*** | `integer` | Count of new recoveries from a positive COVID-19 case on this date | 13 |
| **total_confirmed\*\*** | `integer` | Cumulative sum of cases confirmed after positive test to date | 6447 |
| **total_deceased\*\*** | `integer` | Cumulative sum of deaths from a positive COVID-19 case to date | 133 |
| **total_recovered\*\*** | `integer` | Cumulative sum of recoveries from a positive COVID-19 case to date | 133 |

\*Values can be negative, typically indicating a correction or an adjustment in the way they were
measured. For example, a case might have been incorrectly flagged as recovered one date so it will
be subtracted from the following date.

\*\*Total count will not always amount to the sum of daily counts, because many authorities make
changes to criteria for counting cases, but not always make adjustments to the data. There is also
potential missing data. All of that makes the total counts *drift* away from the sum of all daily
counts over time, which is why the cumulative values, if reported, are kept in a separate column.

### Geography
Information related to the geography for each region:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | CN_HB |
| **latitude** | `double` | Floating point representing the geographic coordinate | 30.9756 |
| **longitude** | `double` | Floating point representing the geographic coordinate | 112.2707 |
| **elevation** | `integer` `[meters]` | Elevation above the sea level | 875 |
| **area** | `integer` [squared kilometers] | Area encompassing this region | 3729 |

### Health
Health related indicators for each region:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | BN |
| **life_expectancy** | `double` `[years]` |Average years that an individual is expected to live | 75.722 |
| **smoking_prevalence** | `double` `[%]` | Percentage of smokers in population | 16.9 |
| **diabetes_prevalence** | `double` `[%]` | Percentage of persons with diabetes in population | 13.3 |
| **infant_mortality_rate** | `double` | Infant mortality rate (per 1,000 live births) | 9.8 |
| **adult_male_mortality_rate** | `double` | Mortality rate, adult, male (per 1,000 male adults) | 143.719 |
| **adult_female_mortality_rate** | `double` | Mortality rate, adult, female (per 1,000 male adults) | 98.803 |
| **pollution_mortality_rate** | `double` | Mortality rate attributed to household and ambient air pollution, age-standardized (per 100,000 population) | 13.3 |
| **comorbidity_mortality_rate** | `double` `[%]` | Mortality from cardiovascular disease, cancer, diabetes or cardiorespiratory disease between exact ages 30 and 70 | 16.6 |
| **hospital_beds** | `double` | Hospital beds (per 1,000 people) | 2.7 |
| **nurses** | `double` | Nurses and midwives (per 1,000 people) | 5.8974 |
| **physicians** | `double` | Physicians (per 1,000 people) | 1.609 |
| **health_expenditure** | `double` `[USD]` | Health expenditure per capita | 671.4115 |
| **out_of_pocket_health_expenditure** | `double` `[USD]` | Out-of-pocket health expenditure per capita | 34.756348 |

Note that the majority of the health indicators are only available at the country level.

### Mobility
[Google's][17] and [Apple's][22] Mobility Reports] are presented in CSV form as
[mobility.csv](https://open-covid-19.github.io/data/v2/mobility.csv) with the
following columns:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **date** | `string` | ISO 8601 date (YYYY-MM-DD) of the datapoint | 2020-03-30 |
| **key** | `string` | Unique string identifying the region | US_CA |
| **mobility_driving** | `double` `[%]` |  Percentage change in movement via driving compared to baseline | -15 |
| **mobility_transit** | `double` `[%]` |  Percentage change in movement via public transit compared to baseline | -15 |
| **mobility_walking** | `double` `[%]` |  Percentage change in movement via walking compared to baseline | -15 |
| **mobility_transit_stations** | `double` `[%]` |  Percentage change in visits to transit station locations compared to baseline | -15 |
| **mobility_retail_and_recreation** | `double` `[%]` |  Percentage change in visits to retail and recreation locations compared to baseline | -15 |
| **mobility_grocery_and_pharmacy** | `double` `[%]` |  Percentage change in visits to grocery and pharmacy locations compared to baseline | -15 |
| **mobility_parks** | `double` `[%]` |  Percentage change in visits to park locations compared to baseline | -15 |
| **mobility_residential** | `double` `[%]` |  Percentage change in visits to residential locations compared to baseline | -15 |
| **mobility_workplaces** | `double` `[%]` |  Percentage change in visits to workplace locations compared to baseline | -15 |

### Oxford Government Response
Summary of a government's response to the events, including a *stringency index*, collected from
[University of Oxford][18]:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **date** | `string` | ISO 8601 date (YYYY-MM-DD) of the datapoint | 2020-03-30 |
| **key** | `string` | Unique string identifying the region | US_CA |
| **school_closing** | `integer` `[0-3]` | Schools are closed | 2 |
| **workplace_closing** | `integer` `[0-3]` | Workplaces are closed | 2 |
| **cancel_public_events** | `integer` `[0-3]` | Public events have been cancelled | 2 |
| **restrictions_on_gatherings** | `integer` `[0-3]` | Gatherings of non-household members are restricted | 2 |
| **public_transport_closing** | `integer` `[0-3]` | Public transport is not operational | 0 |
| **stay_at_home_requirements** | `integer` `[0-3]` | Self-quarantine at home is mandated for everyone | 0 |
| **restrictions_on_internal_movement** | `integer` `[0-3]` | Travel within country is restricted | 1 |
| **international_travel_controls** | `integer` `[0-3]` | International travel is restricted | 3 |
| **income_support** | `integer` `[USD]` | Value of fiscal stimuli, including spending or tax cuts | 20449287023 |
| **debt_relief** | `integer` `[0-3]` | Debt/contract relief for households | 0 |
| **fiscal_measures** | `integer` `[USD]` | Value of fiscal stimuli, including spending or tax cuts | 20449287023 |
| **international_support** | `integer` `[USD]` | Giving international support to other countries | 274000000 |
| **public_information_campaigns** | `integer` `[0-2]` | Government has launched public information campaigns | 1 |
| **testing_policy** | `integer` `[0-3]` | Country-wide COVID-19 testing policy | 1 |
| **contact_tracing** | `integer` `[0-2]` | Country-wide contact tracing policy | 1 |
| **emergency_investment_in_healthcare** | `integer` `[USD]` | Emergency funding allocated to healthcare | 500000 |
| **investment_in_vaccines** | `integer` `[USD]` | Emergency funding allocated to vaccine research | 100000 |
| **stringency_index** | `double` `[0-100]` | Overall stringency index | 71.43 |

For more information about each field and how the overall stringency index is
computed, see the [Oxford COVID-19 government response tracker][18].

### Weather
Daily weather information from nearest station reported by NOAA:

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **date** | `string` | ISO 8601 date (YYYY-MM-DD) of the datapoint | 2020-03-30 |
| **key** | `string` | Unique string identifying the region | US_CA |
| **noaa_station** | `string` | Identifier for the weather station | USC00206080 |
| **noaa_distance** | `double` `[kilometers]` | Distance between the location coordinates and the weather station | 28.693 |
| **minimum_temperature** | `double` `[celsius]` | Recorded hourly minimum temperature | 1.7 |
| **maximum_temperature** | `double` `[celsius]` | Recorded hourly maximum temperature | 19.4 |
| **rainfall** | `double` `[millimeters]` | Rainfall during the entire day | 51.0 |
| **snowfall** | `double` `[millimeters]` | Snowfall during the entire day | 0.0 |

### WorldBank
Most recent value for each indicator of the [WorldBank Database][25].

| Name | Type | Description | Example |
| ---- | ---- | ----------- | ------- |
| **key** | `string` | Unique string identifying the region | ES |
| **`<indicator>`** | `double` | Value of the indicator corresponding to this column, column name is indicator code | 0 |

Refer to the [WorldBank documentation][25] for more details, or refer to the
[worldbank_indicators.csv](./src/data/worldbank_indicators.csv) file for a short description of each
indicator. Each column uses the indicator code as its name, and the rows are filled with the values
for the corresponding `key`.

Note that WorldBank data is only available at the country level and it's not included in the master
table. If no values are reported by WorldBank for the country since 2015, the row value will be
null.

### Notes about the data
For countries where both country-level and subregion-level data is available, the entry which has a
null value for the subregion level columns in the `index` table indicates upper-level aggregation.
For example, if a data point has values
`{country_code: US, subregion1_code: CA, subregion2_code: null, ...}` then that record will have
data aggregated at the subregion1 (i.e. state/province) level. If `subregion1_code`were null, then
it would be data aggregated at the country level.

Another way to tell the level of aggregation is the `aggregation_level` of the `index` table, see
the [schema documentation](#index) for more details about how to interpret it.

Please note that, sometimes, the country-level data and the region-level data come from different
sources so adding up all region-level values may not equal exactly to the reported country-level
value. See the [data loading tutorial][7] for more information.

There is also a [notices.csv](src/data/notices.csv) file which is manually updated with quirks about
the data. The goal is to be able to query by key and date, to get a list of applicable notices to
the requested subset.

### Backwards compatibility
Please note that the following datasets are maintained only to preserve backwards compatibility, but
shouldn't be used in any new projects:
* [Data](https://open-covid-19.github.io/data/data.csv)
* [Latest](https://open-covid-19.github.io/data/data_latest.csv)
* [Minimal](https://open-covid-19.github.io/data/data_minimal.csv)
* [Forecast](https://open-covid-19.github.io/data/data_forecast.csv)
* [Mobility](https://open-covid-19.github.io/data/mobility.csv)
* [Weather](https://open-covid-19.github.io/data/weather.csv)



## Licensing

The output data files are published under the [CC BY-SA](./CC-BY-SA) license. All data is
subject to the terms of agreement individual to each data source, refer to the
[sources of data](#sources-of-data) table for more details. All other code and assets are published
under the [Apache License 2.0](./LICENSE).



## Sources of data

All data in this repository is retrieved automatically. When possible, data is retrieved directly
from the relevant authorities, like a country's ministry of health.

| Data | Source | License and Terms of Use |
| ---- | ------ | ------------------------ |
| Metadata | [Wikipedia](https://wikidata.org) | [CC BY-SA][24] |
| Demographics | [Wikidata](https://wikidata.org) | [CC0][23] |
| Demographics | [DataCommons](https://datacommons.org) | [Attribution required](https://policies.google.com/terms) |
| Demographics | [WorldBank](https://worldbank.org) | [CC BY 4.0](https://www.worldbank.org/en/about/legal/terms-of-use-for-datasets) |
| Economy | [Wikidata](https://wikidata.org) | [CC0][23] |
| Economy | [DataCommons](https://datacommons.org) | [Attribution required](https://policies.google.com/terms) |
| Economy | [WorldBank](https://worldbank.org) | [CC BY 4.0](https://www.worldbank.org/en/about/legal/terms-of-use-for-datasets) |
| Geography | [Wikidata](https://wikidata.org) | [CC0][23] |
| Geography | [WorldBank](https://worldbank.org) | [CC BY 4.0](https://www.worldbank.org/en/about/legal/terms-of-use-for-datasets) |
| Health | [Wikidata](https://wikidata.org) | [CC0][23] |
| Health | [WorldBank](https://worldbank.org) | [CC BY 4.0](https://www.worldbank.org/en/about/legal/terms-of-use-for-datasets) |
| Weather | [NOAA](https://www.ncei.noaa.gov) | [Attribution required, non-commercial use](https://www.wmo.int/pages/prog/www/ois/Operational_Information/Publications/Congress/Cg_XII/res40_en.html) |
| Apple Mobility data | <https://www.apple.com/covid19/mobility> | [Attribution required](https://www.google.com/help/terms_maps/?hl=en) |
| Google Mobility data | <https://www.google.com/covid19/mobility/> | [Attribution required](https://www.google.com/help/terms_maps/?hl=en) |
| Government response data | [Oxford COVID-19 government response tracker][18] | [CC BY 4.0](https://github.com/OxCGRT/covid-policy-tracker/blob/master/LICENSE.txt) |
| Country-level data | [ECDC](https://www.ecdc.europa.eu) | [Attribution required](https://www.ecdc.europa.eu/en/copyright) |
| Country-level data | [Our World in Data](https://ourworldindata.org) | [CC BY 4.0](https://ourworldindata.org/how-to-use-our-world-in-data#how-is-our-work-copyrighted) |
| Argentina | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/Argentina_medical_cases) | [CC BY-SA][24] |
| Australia | <https://covid-19-au.com/> | [Attribution required, educational and academic research purposes](https://covid-19-au.com/faq) |
| Austria | [COVID19 EU Data](https://github.com/covid19-eu-zh/covid19-eu-data) | [MIT](https://github.com/covid19-eu-zh/covid19-eu-data/issues/57) |
| Bolivia | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/Bolivia_medical_cases) | [CC BY-SA][24] |
| Brazil | <https://github.com/elhenrico/covid19-Brazil-timeseries> | [Public Domain](https://github.com/elhenrico/covid19-Brazil-timeseries/blob/master/README.md#public-domain-and-open-data) |
| Canada | [Department of Health Canada](https://www.canada.ca/en/public-health) | [Attribution required](https://www.canada.ca/en/transparency/terms.html) |
| Chile | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/Chile_medical_cases) | [CC BY-SA][24] |
| China | [DXY COVID-19 dataset](https://github.com/BlankerL/DXY-COVID-19-Data) | [MIT](https://github.com/BlankerL/DXY-COVID-19-Data/blob/master/LICENSE) |
| Colombia | [Government Authority](https://www.datos.gov.co) | [Attribution required](https://herramientas.datos.gov.co/es/terms-and-conditions-es) |
| France | [data.gouv.fr](https://data.gouv.fr) | [Open License 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence) |
| Germany | <https://github.com/jgehrcke/covid-19-germany-gae> | [MIT](https://github.com/jgehrcke/covid-19-germany-gae/blob/master/LICENSE) |
| India | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/India_medical_cases) | [CC BY-SA][24] |
| Indonesia | <https://catchmeup.id/covid-19> | [Permission required](https://catchmeup.id/ketentuan-pelayanan) |
| Italy | [Italy's Department of Civil Protection](https://github.com/pcm-dpc/COVID-19) | [CC BY 4.0](https://github.com/pcm-dpc/COVID-19/blob/master/LICENSE) |
| Japan | <https://github.com/swsoyee/2019-ncov-japan> | [MIT](https://github.com/swsoyee/2019-ncov-japan/blob/master/LICENSE) |
| Luxembourg | [data.public.lu](https://data.public.lu/fr/datasets/donnees-covid19)| [CC0](https://data.public.lu/fr/datasets/?license=cc-zero) |
| Malaysia | [Wikipedia](https://en.wikipedia.org/wiki/2020_coronavirus_pandemic_in_Malaysia) | [CC BY-SA][24] |
| Mexico | <https://github.com/mexicovid19/Mexico-datos> | [MIT](https://github.com/mexicovid19/Mexico-datos/blob/master/LICENSE.md) |
| Netherlands | [RIVM](https://data.rivm.nl/covid-19) | [Public Domain](https://databronnencovid19.nl/Disclaimer) |
| Norway | [COVID19 EU Data](https://github.com/covid19-eu-zh/covid19-eu-data) | [MIT](https://github.com/covid19-eu-zh/covid19-eu-data/issues/57) |
| Pakistan | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/Pakistan_medical_cases) | [CC BY-SA][24] |
| Peru | [Wikipedia](https://es.wikipedia.org/wiki/Pandemia_de_enfermedad_por_coronavirus_de_2020_en_Per%C3%BA) | [CC BY-SA][24] |
| Poland | [COVID19 EU Data](https://github.com/covid19-eu-zh/covid19-eu-data) | [MIT](https://github.com/covid19-eu-zh/covid19-eu-data/issues/57) |
| Portugal | [COVID-19: Portugal](https://github.com/carlospramalheira/covid19) | [MIT](https://github.com/carlospramalheira/covid19/blob/master/LICENSE) |
| Russia | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019-20_coronavirus_pandemic_data/Russia_medical_cases) | [CC BY-SA][24] |
| Slovenia | <https://www.gov.si> | [CC BY-SA][24] |
| South Korea | [Wikipedia](https://en.wikipedia.org/wiki/Template:2019%E2%80%9320_coronavirus_pandemic_data/South_Korea_medical_cases) | [CC BY-SA][24] |
| Spain | [Government Authority](https://covid19.isciii.es) | [Attribution required](https://www.mscbs.gob.es/avisoLegal/home.html) |
| Sweden | [Public Health Agency of Sweden](https://www.folkhalsomyndigheten.se/the-public-health-agency-of-sweden/) |  |
| Switzerland | [OpenZH data](https://open.zh.ch) | [CC 4.0](https://github.com/openZH/covid_19/blob/master/LICENSE) |
| United Kingdom | <https://github.com/tomwhite/covid-19-uk-data> | [The Unlicense](https://github.com/tomwhite/covid-19-uk-data/blob/master/LICENSE.txt) |
| USA | [NYT COVID Dataset](https://github.com/nytimes) | [Attribution required, non-commercial use](https://github.com/nytimes/covid-19-data/blob/master/LICENSE) |
| USA | [COVID Tracking Project](https://covidtracking.com) | [CC BY 4.0](https://covidtracking.com/license) |



## Running the data extraction pipeline

To update the contents of the [output folder](output), first install the dependencies:
```sh
pip install -r requirements.txt
```

Then run the following script from the source folder to update all datasets:
```sh
cd src
python run.py
```

See the [source documentation](src) for more technical details.



## Contribute

If you spot an error in the data, or there's a country you would like to include, the best way to
contribute to this project is by helping maintain the data on the relevant Wikipedia article. Not
only can that data be parsed automatically by this project, but it will also help inform millions of
others that receive their information from Wikipedia.

For code contributions, take a look at the [source directory](src/README.md) for more information.

If you do something cool with the data (e.g., visualization or analysis), please let us know!



## Contributors

The main creator of this project is Oscar Wahltinez.
Other contributors will be listed here in the future.

[1]: https://github.com/CSSEGISandData/COVID-19
[2]: https://www.ecdc.europa.eu
[3]: https://github.com/BlankerL/DXY-COVID-19-Data
[4]: https://web.archive.org/web/20200314143253/https://www.salute.gov.it/nuovocoronavirus
[5]: https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports
[6]: https://github.com/open-covid-19/data/issues/16
[7]: https://github.com/open-covid-19/data/examples/data_loading.ipynb
[8]: https://web.archive.org/web/20200320122944/https://www.mscbs.gob.es/profesionales/saludPublica/ccayes/alertasActual/nCov-China/situacionActual.htm
[9]: https://covidtracking.com
[10]: https://github.com/pcm-dpc/COVID-19
[11]: https://github.com/datadista/datasets/tree/master/COVID%2019
[12]: https://open-covid-19.github.io/explorer
[13]: https://kepler.gl/demo/map?mapUrl=https://dl.dropboxusercontent.com/s/lrb24g5cc1c15ja/COVID-19_Dataset.json
[14]: https://www.starlords3k.com/covid19.php
[15]: https://kiksu.net/covid-19/
[16]: https://www.canada.ca/en/public-health/services/diseases/2019-novel-coronavirus-infection.html
[17]: https://www.google.com/covid19/mobility/
[18]: https://www.bsg.ox.ac.uk/research/research-projects/oxford-covid-19-government-response-tracker
[19]: https://auditter.info/covid-timeline
[20]: https://www.coronavirusdailytracker.info/
[21]: https://omnimodel.com/
[22]: https://www.apple.com/covid19/mobility
[23]: https://www.wikidata.org/wiki/Wikidata:Licensing
[24]: https://en.wikipedia.org/wiki/Wikipedia:Copyrights
[25]: https://worldbank.org
