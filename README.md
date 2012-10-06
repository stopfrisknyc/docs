
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

### Description


### Usage


The data api is structured on RESTful semantics rather than SQL semantics. 
There are essentially three kinds of API endpoints: overview, totals, and details. 


#### Base URL

```
http://api.occupy-data.org/stopandfrisk/v1/ 
```

This just gives an overview of the data, including the total number of records in the system, and all the columns that exist on the data

```
{
records: 4259606,
columns: [
"_id",
"race",
"forceuse",
"frisk",
"pct",
"datetime_dayofweek",
"datetime_month",
"datetime",
"datetime_time",
"loc",
"sector",
"cs_objcs",
"crossst",
"radio",
"rf_knowl",
"ac_other",
"explnstp",
"rf_verbl",
"rf_othsw",
"premname",
"officrid",
"rf_rfcmp",
"perstop",
"pf_ptwep",
"riflshot",
"knifcuti",
"typeofid",
"linecm",
"beat",
"cs_other",
"pf_hcuff",
"aptnum",
"pistol",
"rf_vcrim",
"ac_rept",
"compyear",
"x",
"addrpct",
"comppct",
"weight",
"offverb",
"revcmd",
"sex",
"haircolr",
"year",
"OGC_FID",
"ser_num",
"ht_inch",
"timestop",
"machgun",
"state",
"contrabn",
"arstmade",
"ac_assoc",
"stname",
"premtype",
"pf_hands",
"cs_lkout",
"ac_incid",
"post",
"ac_proxm",
"cs_furtv",
"cs_cloth",
"sumoffen"
]
}
```



#### Overview

An "overview" resource which tells you what values exist for a given column, within the set of results you've specified.  For example, http://api.occupy-data.org/stopandfrisk/v1/haircolr lists all the possible hair colors in the database.


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

http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/$gt/10


#### OR 

You can also filter with ORs by repeating a column twice anywhere in the URL.

  So http://api.occupy-data.org/stopandfrisk/v1/haircolr/WH/age/41/haircolr/BK/haircolr/BR shows the precincts in which there are any 41 year olds with either white, black or brown hair.



#### Geospatial Queries

http://api.occupy-data.org/stopandfrisk/v1/loc/$near=-73.88:40.78:1.3/?results&value=loc&value=race&value=sex

Returns All results within 1.3 miles of (-73.88, 40.78)






## License

[Creative Commons](http://creativecommons.org/licenses/by-nc-sa/3.0/)
