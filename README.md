
```
   _____ __                                 __   ____     _      __          __      __           ___    ____  ____
  / ___// /_____  ____     ____ _____  ____/ /  / __/____(_)____/ /__   ____/ /___ _/ /_____ _   /   |  / __ \/  _/
  \__ \/ __/ __ \/ __ \   / __ `/ __ \/ __  /  / /_/ ___/ / ___/ //_/  / __  / __ `/ __/ __ `/  / /| | / /_/ // /  
 ___/ / /_/ /_/ / /_/ /  / /_/ / / / / /_/ /  / __/ /  / (__  ) ,<    / /_/ / /_/ / /_/ /_/ /  / ___ |/ ____// /   
/____/\__/\____/ .___/   \__,_/_/ /_/\__,_/  /_/ /_/  /_/____/_/|_|   \__,_/\__,_/\__/\__,_/  /_/  |_/_/   /___/   
              /_/
```


====

Documentation for the stop and frisk data API


## Metadata 

See [column name and value label descriptions from the source databases here](https://github.com/stopfrisknyc/docs/blob/gh-pages/metadata.md)

### Description

Code for the API: https://github.com/boldprogressives/mongo-spreadsheet-api, licensed under GPLv3 license.


### Usage


The data api is structured on RESTful semantics rather than SQL semantics. 
There are essentially three kinds of API endpoints: overview, totals, and details. 


#### Base URL

```
http://api.occupy-data.org/stopandfrisk/v1/ 
```

The base URL returns the columns that are indexed in the database. These are the only columns that you can use to "begin" a search.

You can also view columns for other queries, like http://api.occupy-data.org/stopandfrisk/v1/haircolr/BK

In this case, it returns all the columns, whether or not they're indexed.

In general, the "columns" data is always used to determine what columns are available for filtering the CURRENT results by. For the base URL it has to be an indexed column, but for other URLs it can be any column (with a few edge-case exceptions, like if you've already used that column with an operator)



#### Totals

A "totals" resource which lets you filter to a particular value of a particular column, and tells you how many records exist within that query.  For example, http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH tells you how many white-haired people are in the database.


#### Details

A "details" resource which lets you return the actual detailed results you've filtered to.  You just do this by appending "?detail" to the previous URL.  So, for example: http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH?detail gives you all the full data about all the white-haired people in the database.

#### Limiting Queries

You can limit the "details" resource to particular columns by appending one or more "&value=" strings to the URL.

For example, http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH?results&value=age&value=eyecolor&value=loc just returns the age, eye color, and lat/lng of all the white-haired people in the database.


#### Nested results

You can also nest resources to add multiple filters.

So the URL http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age is back to a new "overview" resource that tells you all the possible ages of the white-haired people in the database; 

http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/41/?results

shows you all the 41-year-old white haired people

http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/41/pct shows the precincts in which there are any 41-year-old white haired people, and so on.


Operators

```
"$gt",
"$lt",
"$between"
```

```
Base URL + column to query + '/' + $operator + '=' + value
```

http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/$gt=10


#### OR 

You can also filter with ORs by repeating a column twice anywhere in the URL.

So http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/41/haircolr/BK/haircolr/BR shows the precincts in which there are any 41 year olds with either white, black or brown hair.



#### Geospatial Queries

http://api.occupy-data.org/stopandfrisk/v1/loc/$near=-73.88:40.78:1.3/?results&value=loc&value=race&value=sex

Returns All results within 1.3 miles of (-73.88, 40.78)

http://api.occupy-data.org/stopandfrisk/v1/loc/$near=-73.88:40.78:2 

-- note that this returns 100 results 
-- that's the limit for a geospatial query, so if you have 100 results returned you probably should use a smaller radius.

#### Time/Date Queries

http://api.occupy-data.org/stopandfrisk/v1/datetime_time/$lt=0600

-- stop-and-frisks that occurred before 6 AM (meaning between midnight and 6 AM)

http://api.occupy-data.org/stopandfrisk/v1/datetime_time/$between=0600:0900

-- between 6 AM and 9 AM, inclusive

http://api.occupy-data.org/stopandfrisk/v1/datetime/$between=2009:2010

http://api.occupy-data.org/stopandfrisk/v1/datetime/$between=2009-10-01:dec-07-2009

 -- between October 1 and December 7 in 2009 

-- note that the datetime parser is smart enough to understand multiple formats (any machine-formatted datetime string should be unambiguously accepted, as well as some human readable strings like above)



#### Form-based Queries

```
Base URL + column of interest + '/?results=&html=&results_per_page=100&page=0'
```

http://api.occupy-data.org/stopandfrisk/v1/datetime/?results=&html=&results_per_page=100&page=0

http://api.occupy-data.org/stopandfrisk/v1/datetime/?results=&html=&results_per_page=100&page=0





## License

[Creative Commons](http://creativecommons.org/licenses/by-nc-sa/3.0/)
