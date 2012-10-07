
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

### HTML Response

http://api.occupy-data.org/v1/haircolr/WH/age/41/?results&per_page=100&html

### JSON Response

The API supports a json response by default. For some visitors unfamiliar with JSON, you may want to install JSONview, for [Chrome](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) or [Firefox](https://addons.mozilla.org/en-US/firefox/addon/jsonview/)

### Description

An API to access the New York Police Department (NYPD) Stop, Question, and Frisk Databases from 2003-2011, which were previously only available in proprietary database formats. Data elements include searches, stops, frisks, and the use of force, as well as demographic information about the individuals stopped under the policy.

Civil Rights and Criminal Justice are not included by the Washington Post Issue Engine API among "the 18 most important issues of the election." Our data suggest that they should be. In the first three months of 2012, 203,500 New Yorkers were stopped by the police. 181,457 were totally innocent (89 percent); 108,097 were black (54 percent); 69,043 were Latino (33 percent); 18,387 were white (9 percent).

This project developed as part of the Washington Post, NPR, and Sunlight Foundation's Election Hackathon from October 6-7, 2012. It reflects the work of multiple individuals from many organizations across many hackathons.

Code for the API: https://github.com/boldprogressives/mongo-spreadsheet-api, licensed under GPLv3 license.

### Metadata

- [Field Values and Column Definitions](https://github.com/stopfrisknyc/docs#column-definitions-and-possible-field-results-for-querying)


### [Try it out](http://api.occupy-data.org/v1/?results&value=crossst&value=age&value=race&value=crimsusp&value=sex&value=build&value=frisked&html=&results_per_page=100)


### Usage


The data api is structured on RESTful semantics rather than SQL semantics. 
There are essentially three kinds of API endpoints: overview, totals, and details. 



#### Base URL

```
http://api.occupy-data.org/v1/ 
```

The base URL returns the columns that are indexed in the database. These are the only columns that you can use to "begin" a search.

You can also view columns for other queries, like http://api.occupy-data.org/v1/haircolr/BK

In this case, it returns all the columns, whether or not they're indexed.

In general, the "columns" data is always used to determine what columns are available for filtering the CURRENT results by. For the base URL it has to be an indexed column, but for other URLs it can be any column (with a few edge-case exceptions, like if you've already used that column with an operator)

#### Totals

A "totals" resource which lets you filter to a particular value of a particular column, and tells you how many records exist within that query.  For example, http://api.occupy-data.org/v1/haircolr/WH tells you how many white-haired people are in the database.


#### Details

A "details" resource which lets you return the actual detailed results you've filtered to.  You just do this by appending "?results&results_per_page=100" to the previous URL.  So, for example: http://api.occupy-data.org/v1/haircolr/WH/?results&html=&results_per_page=100 gives you all the full data about all the white-haired people in the database.

#### Limiting Queries

You can limit the "details" resource to particular columns by appending one or more "&value=" strings to the URL.

For example, http://api.occupy-data.org/v1/haircolr/WH?results&value=age&value=eyecolor&value=loc/&html=&results_per_page=100 just returns the age, eye color, and lat/lng of all the white-haired people in the database.


#### Nested results

You can also nest resources to add multiple filters.

http://api.occupy-data.org/v1/haircolr/WH/age/41?results&html=&results_per_page=100

shows you all the 41-year-old white-haired people

Operators

```
"$gt",
"$lt",
"$between"
```

```
Base URL + column to query + '/' + $operator + '=' + value
```

http://api.occupy-data.org/v1/haircolr/WH/age/$gt=10


#### OR 

You can also filter with ORs by repeating a column twice anywhere in the URL.

So http://api.occupy-data.org/v1/haircolr/WH/age/41/haircolr/BK/haircolr/BR shows the precincts in which there are any 41-year-olds with either white, black or brown hair.

#### Geospatial Queries

http://api.occupy-data.org/v1/loc/$near=-73.88:40.78:1.3/?results&value=loc&value=race&value=sex

Returns All results within 1.3 miles of (-73.88, 40.78)

http://api.occupy-data.org/v1/loc/$near=-73.88:40.78:2 

- note that this returns 100 results 
- that's the limit for a geospatial query, so if you have 100 results returned you probably should use a smaller radius.

#### Time/Date Queries

http://api.occupy-data.org/v1/datetime_time/$lt=0600

- stop-and-frisks that occurred before 6 AM (meaning between midnight and 6 AM)

http://api.occupy-data.org/v1/datetime_time/$between=0600:0900

- between 6 AM and 9 AM, inclusive

http://api.occupy-data.org/v1/datetime/$between=2009:2010

http://api.occupy-data.org/v1/datetime/$between=2009-10-01:dec-07-2009

- between October 1 and December 7 in 2009 

- note that the datetime parser is smart enough to understand multiple formats (any machine-formatted datetime string should be unambiguously accepted, as well as some human readable strings like above)



#### Form-based Queries

```
Base URL + column of interest + '/?results=&html=&results_per_page=100&page=0'
```


http://api.occupy-data.org/v1/datetime/?results&html&results_per_page=100&page=0


### Column Definitions and Possible Field Results for Querying

Column Value | Column Label | Field Value | Field Label
--- | ---- | --- | ---- |
pct | Precinct of Stop | 1-123 | 1-123
inout | Was Stop Inside or Outside? | I | Inside
inout | Was Stop Inside or Outside? | O | Outside
trhsloc | Was Location Housing or Transit Authority? |  | Neither
trhsloc | Was Location Housing or Transit Authority? | H | Housing
trhsloc | Was Location Housing or Transit Authority? | T | Transit
typeofid | Stopped Person's Identification Type | O | Other
typeofid | Stopped Person's Identification Type | P | Photo
typeofid | Stopped Person's Identification Type | R | Refused
typeofid | Stopped Person's Identification Type | V | Verbal
explnstp | Did Officer Explain Reason For Stop? | N | No
explnstp | Did Officer Explain Reason For Stop? | Y | Yes
othpers | Were Other Persons Stopped, Questioned or Frisked? | N | No
othpers | Were Other Persons Stopped, Questioned or Frisked? | Y | Yes
arstmade | Was an Arrest Made? | N | No
arstmade | Was an Arrest Made? | Y | Yes
sumissue | Was A Summons Issued? | N | No
sumissue | Was A Summons Issued? | Y | Yes
offunif | Was Officer in Uniform? | N | No
offunif | Was Officer in Uniform? | Y | Yes
officrid | id Card Provided by Officer (if Not in Uniform) |  | Not Listed
officrid | id Card Provided by Officer (if Not in Uniform) | 0 | No
officrid | id Card Provided by Officer (if Not in Uniform) | I | Id
frisked | Was Suspect Frisked? | N | No
frisked | Was Suspect Frisked? | Y | Yes
searched | Was Suspect Searched? | N | No
searched | Was Suspect Searched? | Y | Yes
contrabn | Was Contraband Found on Suspect? | N | No
contrabn | Was Contraband Found on Suspect? | Y | Yes
adtlrept | Were Additional Reports Prepared? | N | No
adtlrept | Were Additional Reports Prepared? | Y | Yes
pistol | Was A Pistol Found on Suspect? | N | No
pistol | Was A Pistol Found on Suspect? | Y | Yes
riflshot | Was A Rifle Found on Suspect? | N | No
riflshot | Was A Rifle Found on Suspect? | Y | Yes
asltweap | Was an Assault Weapon Found on Suspect? | N | No
asltweap | Was an Assault Weapon Found on Suspect? | Y | Yes
knifcuti | Was A Knife or Cutting Instrument Found on Suspect? | N | No
knifcuti | Was A Knife or Cutting Instrument Found on Suspect? | Y | Yes
machgun | Was A Machine Gun Found on Suspect? | N | No
machgun | Was A Machine Gun Found on Suspect? | Y | Yes
othrweap | Was Another Type of Weapon Found on Suspect | N | No
othrweap | Was Another Type of Weapon Found on Suspect | Y | Yes
pf_hands | Physical Force Used by Officer - Hands | N | No
pf_hands | Physical Force Used by Officer - Hands | Y | Yes
pf_wall | Physical Force Used by Officer - Suspect on Ground | N | No
pf_wall | Physical Force Used by Officer - Suspect on Ground | Y | Yes
pf_grnd | Physical Force Used by Officer - Suspect Against Wall | N | No
pf_grnd | Physical Force Used by Officer - Suspect Against Wall | Y | Yes
pf_drwep | Physical Force Used by Officer - Weapon Drawn | N | No
pf_drwep | Physical Force Used by Officer - Weapon Drawn | Y | Yes
pf_ptwep | Physical Force Used by Officer - Weapon Pointed | N | No
pf_ptwep | Physical Force Used by Officer - Weapon Pointed | Y | Yes
pf_baton | Physical Force Used by Officer - Baton | N | No
pf_baton | Physical Force Used by Officer - Baton | Y | Yes
pf_hcuff | Physical Force Used by Officer - Handcuffs | N | No
pf_hcuff | Physical Force Used by Officer - Handcuffs | Y | Yes
pf_pepsp | Physical Force Used by Officer - Pepper Spray | N | No
pf_pepsp | Physical Force Used by Officer - Pepper Spray | Y | Yes
pf_other | Physical Force Used by Officer - Other | N | No
pf_other | Physical Force Used by Officer - Other | Y | Yes
radio | Radio Run | N | No
radio | Radio Run | Y | Yes
ac_rept | Additional Circumstances - Report by Victim/Witness/Officer | N | No
ac_rept | Additional Circumstances - Report by Victim/Witness/Officer | Y | Yes
ac_inves | Additional Circumstances - Ongoing Investigation | N | No
ac_inves | Additional Circumstances - Ongoing Investigation | Y | Yes
rf_vcrim | Reason For Frisk - Violent Crime Suspected | N | No
rf_vcrim | Reason For Frisk - Violent Crime Suspected | Y | Yes
rf_othsw | Reason For Frisk - Other Suspicion of Weapons | N | No
rf_othsw | Reason For Frisk - Other Suspicion of Weapons | Y | Yes
ac_proxm | Additional Circumstances - Proximity to Scene of Offense | N | No
ac_proxm | Additional Circumstances - Proximity to Scene of Offense | Y | Yes
rf_attir | Reason For Frisk - Inappropriate Attire For Season | N | No
rf_attir | Reason For Frisk - Inappropriate Attire For Season | Y | Yes
cs_objcs | Reason For Stop - Carrying Suspicious Object | N | No
cs_objcs | Reason For Stop - Carrying Suspicious Object | Y | Yes
cs_descr | Reason For Stop - Fits A Relevant Description | N | No
cs_descr | Reason For Stop - Fits A Relevant Description | Y | Yes
cs_casng | Reason For Stop - Casing A Victim or Location | N | No
cs_casng | Reason For Stop - Casing A Victim or Location | Y | Yes
cs_lkout | Reason For Stop - Suspect Acting as A Lookout | N | No
cs_lkout | Reason For Stop - Suspect Acting as A Lookout | Y | Yes
rf_vcact | Reason For Frisk-  Actions of Engaging in A Violent Crime | N | No
rf_vcact | Reason For Frisk-  Actions of Engaging in A Violent Crime | Y | Yes
cs_cloth | Reason For Stop - Wearing Clothes Commonly Used in A Crime | N | No
cs_cloth | Reason For Stop - Wearing Clothes Commonly Used in A Crime | Y | Yes
cs_drgtr | Reason For Stop - Actions Indicative of A Drug Transaction | N | No
cs_drgtr | Reason For Stop - Actions Indicative of A Drug Transaction | Y | Yes
ac_evasv | Additional Circumstances - Evasive Response to Questioning | N | No
ac_evasv | Additional Circumstances - Evasive Response to Questioning | Y | Yes
ac_assoc | Additional Circumstances - Associating With Known Criminals | N | No
ac_assoc | Additional Circumstances - Associating With Known Criminals | Y | Yes
cs_furtv | Reason For Stop - Furtive Movements | N | No
cs_furtv | Reason For Stop - Furtive Movements | Y | Yes
rf_rfcmp | Reason For Frisk - Refuse to Comply W Officer's Directions | N | No
rf_rfcmp | Reason For Frisk - Refuse to Comply W Officer's Directions | Y | Yes
ac_cgdir | Additional Circumstances - Change Direction at Sight of Officer | N | No
ac_cgdir | Additional Circumstances - Change Direction at Sight of Officer | Y | Yes
rf_verbl | Reason For Frisk - Verbal Threats by Suspect | N | No
rf_verbl | Reason For Frisk - Verbal Threats by Suspect | Y | Yes
cs_vcrim | Reason For Stop - Actions of Engaging in A Violent Crime | N | No
cs_vcrim | Reason For Stop - Actions of Engaging in A Violent Crime | Y | Yes
cs_bulge | Reason For Stop - Suspicious Bulge | N | No
cs_bulge | Reason For Stop - Suspicious Bulge | Y | Yes
cs_other | Reason For Stop - Other | N | No
cs_other | Reason For Stop - Other | Y | Yes
ac_incid | Additional Circumstances - Area Has High Crime Incidence | N | No
ac_incid | Additional Circumstances - Area Has High Crime Incidence | Y | Yes
ac_time | Additional Circumstances - Time of Day Fits Crime Incidence | N | No
ac_time | Additional Circumstances - Time of Day Fits Crime Incidence | Y | Yes
rf_knowl | Reason For Frisk - Knowledge of Prior Crim Behav | N | No
rf_knowl | Reason For Frisk - Knowledge of Prior Crim Behav | Y | Yes
ac_stsnd | Additional Circumstances - Sights or Sounds of Criminal Activity | N | No
ac_stsnd | Additional Circumstances - Sights or Sounds of Criminal Activity | Y | Yes
ac_other | Additional Circumstances - Other | N | No
ac_other | Additional Circumstances - Other | Y | Yes
sb_hdobj | Basis of Search - Hard Object | N | No
sb_hdobj | Basis of Search - Hard Object | Y | Yes
sb_outln | Basis of Search - Outline of Weapon | N | No
sb_outln | Basis of Search - Outline of Weapon | Y | Yes
sb_admis | Basis of Search - Admission by Suspect | N | No
sb_admis | Basis of Search - Admission by Suspect | Y | Yes
sb_other | Basis of Search - Other | N | No
sb_other | Basis of Search - Other | Y | Yes
rf_furt | Reason For Frisk - Furtive Movements | N | No
rf_furt | Reason For Frisk - Furtive Movements | Y | Yes
rf_bulg | Reason For Frisk - Suspicious Bulge | N | No
rf_bulg | Reason For Frisk - Suspicious Bulge | Y | Yes
offverb | Verbal Statement Provided by Officer (if Not in Uniform) |  | Not Listed
offverb | Verbal Statement Provided by Officer (if Not in Uniform) | 0 | No
offverb | Verbal Statement Provided by Officer (if Not in Uniform) | V | Verbal
offshld | Shield Provided by Officer (if Not in Uniform) |  | Not Listed
offshld | Shield Provided by Officer (if Not in Uniform) | 0 | No
offshld | Shield Provided by Officer (if Not in Uniform) | S | Shield
forceuse | Reason Force Used |  | Not Listed
forceuse | Reason Force Used | DS | Defense Of Self
forceuse | Reason Force Used | DO | Defense Of Other
forceuse | Reason Force Used | OR | Overcome Resistence
forceuse | Reason Force Used | OT | Other
forceuse | Reason Force Used | SF | Suspected Flight
forceuse | Reason Force Used | SW | Suspected Weapon
sex | Sex |  | Not Listed
sex | Sex | F | Female
sex | Sex | M | Male
sex | Sex | Z | Unknown
race | Race |  | Not Listed
race | Race | A | Asian/Pacific Islander
race | Race | B | Black
race | Race | I | American Indian/Alaskan Native
race | Race | P | Black-Hispanic
race | Race | Q | White-Hispanic
race | Race | W | White
race | Race | X | Unknown
race | Race | Z | Other
haircolr | Haircolor |  | Not Listed
haircolr | Haircolor | BA | Bald
haircolr | Haircolor | BK | Black
haircolr | Haircolor | BL | Blond
haircolr | Haircolor | BR | Brown
haircolr | Haircolor | DY | Dyed
haircolr | Haircolor | FR | Frosted
haircolr | Haircolor | GY | Gray
haircolr | Haircolor | RD | Red
haircolr | Haircolor | SN | Sandy
haircolr | Haircolor | SP | Salt And Pepper
haircolr | Haircolor | WH | White
haircolr | Haircolor | XX | Unknown
haircolr | Haircolor | ZZ | Other
eyecolor | Eye Color |  | Not Listed
eyecolor | Eye Color | BK | Black
eyecolor | Eye Color | BL | Blue
eyecolor | Eye Color | BR | Brown
eyecolor | Eye Color | DF | Two Different
eyecolor | Eye Color | GR | Green
eyecolor | Eye Color | GY | Gray
eyecolor | Eye Color | HA | Hazel
eyecolor | Eye Color | MA | Maroon
eyecolor | Eye Color | PK | Pink
eyecolor | Eye Color | VI | Violet
eyecolor | Eye Color | XX | Unknown
eyecolor | Eye Color | ZZ | Other
build | Build |  | Not Listed
build | Build | H | Heavy
build | Build | M | Medium
build | Build | T | Thin
build | Build | U | Muscular
build | Build | Z | Unknown
detailCM | Crime Code Description | 1 | Abandonment Of A Child
detailCM | Crime Code Description | 2 | Abortion
detailCM | Crime Code Description | 3 | Absconding
detailCM | Crime Code Description | 4 | Adultery
detailCM | Crime Code Description | 5 | Aggravated Assault
detailCM | Crime Code Description | 6 | Aggravated Harassment
detailCM | Crime Code Description | 7 | Aggravated Sexual Abuse
detailCM | Crime Code Description | 8 | Arson
detailCM | Crime Code Description | 9 | Assault
detailCM | Crime Code Description | 10 | Auto Stripping
detailCM | Crime Code Description | 11 | Bigamy
detailCM | Crime Code Description | 12 | Bribe Receiving
detailCM | Crime Code Description | 13 | Bribery
detailCM | Crime Code Description | 14 | Burglary
detailCM | Crime Code Description | 15 | Coercion
detailCM | Crime Code Description | 16 | Computer Tampering
detailCM | Crime Code Description | 17 | Computer Trespass
detailCM | Crime Code Description | 18 | Course Of Sexual Conduct
detailCM | Crime Code Description | 19 | Cpsp
detailCM | Crime Code Description | 20 | Cpw
detailCM | Crime Code Description | 21 | Creating A Hazard
detailCM | Crime Code Description | 22 | Criminal Contempt
detailCM | Crime Code Description | 23 | Criminal Mischief
detailCM | Crime Code Description | 24 | Criminal Possesion Of Controlled Substance
detailCM | Crime Code Description | 25 | Criminal Possession Of Computer Material
detailCM | Crime Code Description | 26 | Criminal Possession Of Forged Instrument
detailCM | Crime Code Description | 27 | Criminal Possession Of Marihuana
detailCM | Crime Code Description | 28 | Criminal Sale Of Controlled Substance
detailCM | Crime Code Description | 29 | Criminal Sale Of Marihuana
detailCM | Crime Code Description | 30 | Criminal Tampering
detailCM | Crime Code Description | 31 | Criminal Trespass
detailCM | Crime Code Description | 32 | Custodial Interference
detailCM | Crime Code Description | 33 | Eaves Dropping
detailCM | Crime Code Description | 34 | Endanger The Welfare Of A Child
detailCM | Crime Code Description | 35 | Escape
detailCM | Crime Code Description | 36 | Falsify Business Records
detailCM | Crime Code Description | 37 | Forgery
detailCM | Crime Code Description | 38 | Forgery Of A Vin
detailCM | Crime Code Description | 39 | Fortune Telling
detailCM | Crime Code Description | 40 | Fraud
detailCM | Crime Code Description | 41 | Fraudulent Accosting
detailCM | Crime Code Description | 42 | Fraudulent Make Electronic Access Device
detailCM | Crime Code Description | 43 | Fraudulent Obtaining A Signature
detailCM | Crime Code Description | 44 | Gambling
detailCM | Crime Code Description | 45 | Grand Larceny
detailCM | Crime Code Description | 46 | Grand Larceny Auto
detailCM | Crime Code Description | 47 | Harassment
detailCM | Crime Code Description | 48 | Hazing
detailCM | Crime Code Description | 49 | Hindering Prosecution
detailCM | Crime Code Description | 50 | Incest
detailCM | Crime Code Description | 51 | Insurance Fraud
detailCM | Crime Code Description | 52 | Issue A False Certificate
detailCM | Crime Code Description | 53 | Issue A False Financial Statement
detailCM | Crime Code Description | 54 | Issuing Abortion Articles
detailCM | Crime Code Description | 55 | Jostling
detailCM | Crime Code Description | 56 | Kidnapping
detailCM | Crime Code Description | 57 | Killing Or Injuring A Poilce Animal
detailCM | Crime Code Description | 58 | Loitering
detailCM | Crime Code Description | 59 | Making Graffiti
detailCM | Crime Code Description | 60 | Menacing
detailCM | Crime Code Description | 61 | Misapplication Of Property
detailCM | Crime Code Description | 62 | Murder
detailCM | Crime Code Description | 63 | Obscenity
detailCM | Crime Code Description | 64 | Obstructing Firefighting Operations
detailCM | Crime Code Description | 65 | Obstructing Governmental Administration
detailCM | Crime Code Description | 66 | Offering A False Instrument
detailCM | Crime Code Description | 67 | Official Misconduct
detailCM | Crime Code Description | 68 | Petit Larceny
detailCM | Crime Code Description | 69 | Possession Of Burglar Tools
detailCM | Crime Code Description | 70 | Possession Of Eaves Dropping Devices
detailCM | Crime Code Description | 71 | Possession Of Graffiti Instruments
detailCM | Crime Code Description | 72 | Prohibited Use Of Weapon
detailCM | Crime Code Description | 73 | Promoting Suicide
detailCM | Crime Code Description | 74 | Prostitution
detailCM | Crime Code Description | 75 | Public Display Of Offensive Sexual Material
detailCM | Crime Code Description | 76 | Public Lewdness
detailCM | Crime Code Description | 77 | Rape
detailCM | Crime Code Description | 78 | Reckless Endangerment
detailCM | Crime Code Description | 79 | Reckless Endangerment Property
detailCM | Crime Code Description | 80 | Refusing To Aid A Peace Or Police Officer
detailCM | Crime Code Description | 81 | Rent Gouging
detailCM | Crime Code Description | 82 | Resisting Arrest
detailCM | Crime Code Description | 83 | Reward Official Misconduct
detailCM | Crime Code Description | 84 | Riot
detailCM | Crime Code Description | 85 | Robbery
detailCM | Crime Code Description | 86 | Self Abortion
detailCM | Crime Code Description | 87 | Sexual Abuse
detailCM | Crime Code Description | 88 | Sexual Misconduct
detailCM | Crime Code Description | 89 | Sexual Performance By A Child
detailCM | Crime Code Description | 90 | Sodomy
detailCM | Crime Code Description | 91 | Substitution Of Children
detailCM | Crime Code Description | 92 | Tampering With A Public Record
detailCM | Crime Code Description | 93 | Tampering With Consumer Product
detailCM | Crime Code Description | 94 | Tampering With Private Communications
detailCM | Crime Code Description | 95 | Terrorism
detailCM | Crime Code Description | 96 | Theft Of Services
detailCM | Crime Code Description | 97 | Trademark Counterfeiting
detailCM | Crime Code Description | 98 | Unlawfully Dealing With Fireworks
detailCM | Crime Code Description | 99 | Unauthorized Recording
detailCM | Crime Code Description | 100 | Unauthorized Use Of A Vehicle
detailCM | Crime Code Description | 101 | Unauthorized Use Of Computer
detailCM | Crime Code Description | 102 | Unlawful Assembly
detailCM | Crime Code Description | 103 | Unlawful Duplication Of Computer Material
detailCM | Crime Code Description | 104 | Unlawful Possession Of Radio Devices
detailCM | Crime Code Description | 105 | Unlawful Use Of Credit Card, Debit Card
detailCM | Crime Code Description | 106 | Unlawful Use Of Secret Scientific Material
detailCM | Crime Code Description | 107 | Unlawful Wearing A Body Vest
detailCM | Crime Code Description | 108 | Unlawfull Imprisonment
detailCM | Crime Code Description | 109 | Unlawfully Dealing With A Child
detailCM | Crime Code Description | 110 | Unlawfully Use Slugs
detailCM | Crime Code Description | 111 | Vehicular Assault
detailCM | Crime Code Description | 112 | Other
detailCM | Crime Code Description | 113 | Forcible Touching





## License

[Creative Commons](http://creativecommons.org/licenses/by-nc-sa/3.0/)
