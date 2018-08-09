# Chung Metadata Migaration Project Report

## Table of Contents
* [Background](#background)
* [Goals](#goals)
* [Major Tasks](#major-tasks)
* [Methods](#methods)
  - [ ] [Merge RBSC, DI and EX datasets](#merge-rbsc-di-and-ex-datasets)
  - [ ] [Remove Duplicates](#remove-duplicates)
  - [ ] [Check mismatched RBSC and DI columns](#check-mismatched-rbsc-and-di-columns)
  - [ ] [Identify possible gaps](#identify-possible-gaps)
  - [ ] [Check Data Integrity](#check-data-integrity)
  - [ ] [Creators Cleaning](#creators-cleaning)
  - [ ] [CSV Import](#csv-import)
* [Outcomes](#outcomes)
* [Range of Access Identifiers](#range-of-access-identifiers)
* [Appendix](#appendix)

## Background
UBC Rare Books and Special Collections (RBSC) of UBC Library is working with Digital Initiatives at UBC library to decommission the current Chung database and move information to a complete system of record (AtoM software). The Chung collection is a major asset to UBC, and holds over 25, 000 items that document immigration and settlement to British Columbia, the Canadian Pacific Railway, and early exploration. The collection is particularly rich in its documentation of the Canadian-Chinese diaspora. 
### Introduction on the dataset
I was given two Excel Spreadsheet contains records of Chung collection respectively from RBSC and DI. RBSC dataset was exported from the Drupal database, which can be accessed online through chung.library.ubc.ca. The spreadsheet I received on May 14, 2018 has 19875 records, and 21 descriptive fields.

The source repository of DI dataset is CONTENTdm. The data can be accessed online through open.library.ubc.ca. The spreadsheet I received on May 15, 2018 has 19023 records and 34 fields.
 

Among the 21 fields of RBSC dataset and 34 fields of DI, 20 of them are shared by both datasets. Those fields are

|RBSC|	DI	|Description|
|----|-------|----------|
|Access Identifier|Access Identifier|
|Digital Identifier|Digital Identifier|	
|Title	|Title|	
|Alternative Title	|Alternative Title	|
|Location	|Location|	
|Notes	|Notes|	
|Creator	|Creator|	
|Description	|Description|	
|Date Created	|Date Created|	
|Date Issued	|Date Issued|
|Sort Date	|Sort Date|
|Category|Category|
|Language|Language|	
|Genre|Genre|	
|Subject	|Subject|	
|Subject - Geographic	|Subject - Geographic|	
|Publisher	|Publisher|	
|Publisher Original	|Publisher Original|
|Personal Names	|Personal Names|
|Physical Description|Extent|	

## Goals
The goal of the project is to have a full list of current collections available for import into AtoM, the new metadata management system. The final list should be both easy to manage, as well as easy to access for users. To ensure that, this list should meet the following requirements by the end of the project:
* ~~The format of the data should comply with the current Canadian Rules of Description~~
### For easy access
* A hirarchy was established for collection arrangement
* The list should include all collections that have been described or received in the Chung Collection, though gaps may still exist for missing items
* The list need to ensure that all description information for the items are correct, including
  * All discrepancies between RBSC and DI dataset on fields to import to AtoM are checked (Access Identifier, Title, Alternative Title, Location, Notes, Creator, Description, Date Created, Date Issued, Language, Physical Description)
  * Locations are correctly assigned for existing items, which means all items should be assigned a location. If the item is found missing, the location field should be indicated as “missing”, while the original location should be recorded in “Staff_Notes” field.
  * The information described the items should keep consistent all the way, i.e. use the same format and for the same attribute, and the same name for the same entity (e.g. creators)
* The dates are indexed and could be searched on date range in Advanced Search in AtoM, which means startDate and endDate of an event in the form of YYYY-MM-DD should be added to the dataset.
### For easy administration
* Each item should be assigned an unique Access Identifier, i.e. 
  * There are no duplicated identifiers
  * New items could be easily assigned and Access Identifier in compliance with the current format, which means there should be enough reserved numbers for future records
* The list should be easy to maintain even if the administrator changed, i.e.
  * The list is in a easy to manipulate format, such as Excel spreadsheet.
  * The Master List is mapped to the AtoM CSV Template, so that information in the Master List and AtoM always keep consistent and updated
  * Current management rules should be written down

## Methods

### Merge RBSC, DI and EX datasets
(Detailed steps and more information in "2018-05-17_MergingMethod.docx")

The original RBSC dataset has 19875 records, and the DI dataset has 19023 records. I updated some of the identifiers based on the exhibition items list file. After a full join on the two datasets, 19981 records were in the dataset in total. Full join will keep all records in both RBSC and DI dataset. All columns in both datasets will also be kept and showed in the query. 

Another 37 records from the Chung Exhibition Room list were also added to the master dataset.

After verify the physical records of gaps existing for RBSC and DI datasets, the metadata information of those gaps were copied from DI dataset.


Date|Actions on dataset|Total Before|Change|Total After|Subset Before|Subset after|Notes
-|-|-|-|-|-|-|-
2018-05-22|RBSC/DI Merge|19875/19023|-|19968|||
2018-05-24|Add new PH-09370 Records|19968|+340|20308|1407|1747|
2018-05-24|Mismatched Titles|20308|-4|20304|111|107|
2018-05-24|Duplicates removed by CC|20304|-44|20260|80|36|
2018-05-25|Library Items|20260|-35|20225|35|0|
2018-05-25|Duplicates removed by JL|20225|-21|20204|36|15|
2018-05-29|Add Ex Newly Added records|20204|+37|20241|0|37|EX Newly Added
2018-05-29|Add CC-TX-267-10-5 which was found during the Box 261~277 audit|20241|+1|20242|0|1|Further Description Required
2018-06-04|Add Items in Box 131|20242|+5|20247|0|5|
2018-06-05|Add Skipped Identifiers in Box 1|20247|+14|20261|185|199|Newly added on June 5, 2018 by Claire Cui. Further description required.
2018-06-05|Add Skipped Identifiers in Box 2~5|20261|+13|20274|149|162|Newly added on June 5, 2018 by Claire Cui. Further description required.
2018-06-11|Add skipped identifiers in Box 6~59|20274|+19|20293|0|19|Newly added on June 11, 2018 by Claire Cui. 
2018-06-12|Add draft records in current AtoM|20293|+9|20302|0|9|Newly added on June 12 from AtoM Draft

### Identify possible gaps
Some gaps are also identified during checking the metadata. Some of the identifiers are skipped. After checked the potential location of those skipped identifiers, some items were added to the master dataset. Those records were marked as "Draft", which means it requires further description, or "Skipped", which means the skipped identifier was not located in Chung Collection.

### Remove Duplicates
Use Check Duplicated Wizard of Access, and “remove duplicates” function of Excel.
The identification of duplicated records are based on the identifier. If one identifier appeared more than once in the database, it will be identified and investigated. 
I first removed those records which contain exactly the same information in each column. What left was records with the same identifier, but some of the fields are different. Jacky Lai investigated into those records and determined what the right information to keep in the end was. All duplicates were removed during this process, meaning each item are assigned a unique identifier.

### Check mismatched RBSC and DI columns
During the project, several fields are compared to ensure the information are consistent, including: titles, locations, date created, notes and descriptions. Creator is also a field that should be compared, however we decided to clean creator information separately from the process, based on that: 1. Neither RBSC and DI has done cleaning for the whole dataset; 2. there should be no different forms of the same creator in order to maintain the integrity of the authority records in AtoM. 
After merging the datasets, I could compare discrepancies in RBSC and DI information using SQL query in Access. For required fields like title, location and date created, except for mismatched values, empty values were also checked and filled out during this project.
e.g. Compare mismatched date created
```SQL
SELECT *
FROM ChungMaster
WHERE ( ([DI_Date Created]<>[RBSC_Date Created]) 
                 OR ((([DI_Date Created] IS NULL) OR ([DI_Date Created] = "NULL") 
                 AND ([DI_Access Identifier] IS NOT NULL)) 
                 OR ((([RBSC_Date Created] IS NULL) OR ([RBSC_Date Created]= "NULL")) 
                 AND ([RBSC_Access Identifier] IS NOT NULL))));
 ```
     
After Claire C. pulled the list of mismatched columns, Jacky Lai decided what information to keep in the master database. The decisions have been recorded in folder “Mismatched DI and RBSC Columns”.


### Check Data Integrity
* Make notes of updates on the Master spreadsheet on column “Notes_Merging_CC”

### Creators Cleaning
#### Find different forms of names of the same creator
To create such a list, we need to ensure that the same entity use the same form of names, according to the requirements from RAD on authority records. For example, “Yip, Sang” and “Yip Sang” should all be “Yip, Sang”; “Steele & Co.”, “Steele & Company” and “Steele and Company” should all be “Steele and Company”. 
And to ensure that all creators in the archival description imported into AtoM correctly connect to authority records in AtoM, we need to prepare an authority records list, which contains all creators that appeared in the Chung metadata collection. The list will be imported before importing the Chung metadata information. (Citation required)
I made use of a python library named “fuzzywuzzy” to find: 
1. Similar creators in this same collection
2. Similar authority records in AtoM of RBSC authority records list
“Fuzzywuzzy” could compare the similar rate between two phrases, for example, 
```python
fuzz.token_set_ratio("[Victoria Chinese Public School]","Chinese Public School in Victoria")
```
100
```python
fuzz.token_set_ratio("Wand, C.B.","Wand Cecil B.")
```
86

In this case, I will look into the creators with a similar rate greater than 80%. For those records has a similar rate lower than 80%, it could hardly be the same creator as far as I could tell.
The process of the calculation is available at https://github.com/clairecui-ubc/chungMD/blob/master/CreatorCleaning/fuzzy/FindSimilarCreators.ipynb

#### Creator Cleaning
Based on the work of previous step, Claire Williams and Claire Cui both investigated some suspicious creators, including check the records or find historical information online, to correct the form of the records. The list of old and updated creator is available in “Creators List.xlsx”
#### Replace creators
With the cleaned list, I used python programming to replace the updated list, where the code is available at https://github.com/clairecui-ubc/chungMD/blob/master/CreatorCleaning/UpdateCreators_0705.ipynb

#### The outcome of this step would include:
* An authority list contains all creators in Chung collection, based on the CSV template provided by Artefactual
* A dictionary of all pairs of old and updated creators in Chung collection
* The frequency of each creator appeared in the Chung collection

### CSV Import
After the previous steps for data cleaning for Chung Collection Metadata were done, we created CSV import files for AtoM upload. We reference the [CSV Import steps from Artefactual AtoM 2.4 documentation documentation](https://www.accesstomemory.org/en/docs/2.4/user-manual/import-export/csv-import/#csv-import), and map the data to the template. Based on the Chung Metadata, the fields are mapped based on the following relationship:

Field Name|CSV Template|AtoM Field
----|----|----
ID| |		
RBSC_ID| |		
Status|	|
legacyID|legacyID|	
parentID|parentID|	
Access Identifier|identifier|Identifier
Digital Identifier|alternativeIdentifiers and alternativeIdentifierLabels|Add alternative identifier(s)
Title|title|Title proper
Alternative Title|alternateTitle|Parallel title
GeneralMaterialDesignation|radGeneralMaterialDesignation|General material designation
Location|physicalObjectName|Physical Storage (Appeared on right column)
Date Created|eventDates and eventTypes|Date
eventStartDates|eventStartDates|Dates of creation - Start
eventEndDates|eventEndDates|Dates of creation - End
Date Issued| |	
Sort Date|	|
Notes|generalNote|Other notes - General note
NEW_Creator|eventActors|Creator
RBSC_Creator| |		
Description|scopeAndContent|Scope and content
Physical_Description|extentAndMedium|Physical description
Publisher Original|radTitleProperOfPublishersSeries|Title proper of publisher’s series
Publisher| |
Subject|[subjectAccessPoints?]|	
Subject - Geographic|		|
NEW_Category|*Used for Series Classification*|	
RBSC_Category|	|
Personal Names|[nameAccessPoints?]|
RBSC_Genre|[genreAccessPoints?]	|
DI_Genre|		|
language_CSV|language|Language of material
Language| |
levelOfDescription|levelOfDescription|Level of description
RBSC_Staff_Notes|	|	
Notes_Merging_CC|	|	

The master dataset was broken into several files based on series and subseries to reduce the risk of uploading failure. During the first upload, only records marked "Ready" for the "Status" were uploaded. The first upload happened on August 8 to August 9, 2018. 19658 records were uploaded in total.

ID|Series/Subseries|SeriesCount
---|----|----
1_1|Chinese immigration and settlement to Canada|1581
1_2|The Yip family and Yip Sang Company|1178
1_3|Cheekungtong (Chinese Freemasons)|30
1_4|Clandonald and Scottish immigration to Canada|258
2_1|Early British Columbia History|1883
2_2|Early Canadian History|5
3_1|Canadian Pacific Railway|2310
3_2|C.P.R. steamships|5292
3_3|Travel and tourism on the C.P.R.|5446
3_4|British Columbia Coast Steamship Service|766
3_5|Artwork and images of the C.P.R.|340
3_6|C.P.R. artefacts|371
3_7|Working for the C.P.R.|76
3_8|Esquimalt and Nanaimo Railway Company|100
4|Canadian Transportation|4
5|Chung Collection promotional material and memorabilia|18
|Grand Total| |19658


## Outcomes
### Collection Arrangement
* Collection: Chung Collection
  * Series: Immigration and Settlement
    * Subseries: Chinese immigration and settlement to Canada (Old Chung Categories: “The Asian Experience in North America” and “Images of Chinese People and Communities in North America”)
    * Subseries: The Yip Family and Yip Sang Company
    * Subseries: Chinese Freemasons (Cheekungtong)
    * Subseries: Clandonald and Scottish immigration to Canada
  * Series: Early British Columbia and Canadian History 
    * Subseries: Early British Columbia History (Old Chung Category: British Columbia historical documents, photographs and artifacts)
    * Subseries: Early Canadian History (Newly added)
  * Series: Canadian Pacific Railway Company
    * Subseries: Canadian Pacific Railway
    * Subseries: C.P.R. steamships
    * Subseries: Travel and Tourism with the C.P.R.
    * Subseries: British Columbia Coast Steamship Service
    * Subseries: Artwork and images of the C.P.R.
    * Subseries: C.P.R. artefacts
    * Subseries: Working for the C.P.R.
    * Subseries: Esquimalt and Nanaimo Railway Company
  * Series: Canadian transportation (Newly added)
  * Series: Chung Collection promotional material and memorabilia (Newly added)

### Major tasks finished
- [x] A full list of metadata available
  - [x] Include all records from RBSC (from Drupal, the decommissioned database), DI and Newly added Exhibition items
  - [x] Duplicates removed
  - [x] Library items removed
  - [x] Gaps identified: added skipped identifiers
  - [ ] __*Possible Drupal export gap (not finished)*__
- [x] Archival descriptions are consistant, including
  - [x] Identifiers are updated, including EX identifiers and misassigned identifiers
  - [x] RBSC and DI discrepancies checked: Title, Location, Date, Description, Notes, Physical description
  - [x] Creators Cleaning:  Unify different forms of creators
- [x] The format of the data complied with AtoM import requirements
  - [x] Addded `General Material Designation` for all records based on identifier, genre and description. This field was added because the physical descriptions are not available for all records
  - [x] Added `level of description` for all records, including collection, series, subseries, file and item
  - [x] Added `start date` and `end date` for all records
  - [x] Added `language_CSV` in the format of ISO-639-1 to support CSV import
- [x] Established hirarchy of items in the collection
  - [x] Created a hirarchy based on current categories
  - [x] Assigned one and only one category for each record
  - [x] Established album/photo links using parentID field
- [x] Kept documentation and records of changes and decisions made to the Chung Metadata
  - [x] Added columns in the master spreadsheet to mark changes: Status, Notes_Merging_CC, RBSC_Staff_Notes
  - [x] Keep logs: master daily backup, Updates Log, Co-op work log
  - [ ] Documentation: report, work flow, follow up tasks, policies and procedures, etc.

### Major Updates
*  Identifiers updated because of mismatching of the item and corresponding metadata. The update records is located in Update_logs.xlsx. Before update any related information in other related database, such as DI dataset, it should be start with identifier updates.
*  Updated identifiers, archival descriptions and added new records for items in Chung Exhibition room.
*  Creators cleaned, except for Canadian Pacific Railway Company related creators.
*  Added skipped identifiers


## Range of Access Identifiers
* Artefacts: CC-AR-00001~CC-AR-00787
* Audio and visual materials: CC-AV-190-1~CC-AV-190-13
* Graphic materials: CC-GR-00001~CC-GR-00089
* Oversized items: CC-OS-00001~CC-OS-00411
* Albums: CC-PH-9-1~CC-PH-92-1
* Photos: CC-PH-00001~CC-PH-10742
  * Skipped: CC-PH-00680~699; CC-PH-01688~1718; CC-PH-01728~01992; CC-PH-08862~9067
* Textual records: CC-TX-61-1~CC-TX-286-1

* Other skipped identifiers are noted “skipped” in the “Status” column in the master spreadsheet.

## Appendix
### Update Logs
See “T:\RBSC\PE files (staff files)\PE Claire Cui\Merged datasets\000_UpdateLog.xlsx”

### Major Tasks
* Merge RBSC and DI spreadsheets
  * Remove duplicates
  * Identify gaps, missing items (eg. skipped identifiers)
    * Krisztina: Issue of missing items from the drupel database that are not represented in the CSV export from Drupel
  * Fix mismatched fields
  * Decide metadata fields to keep
* Modify current dates to RAD compliant dates
  * Add start and end dates in new master spreadsheet to enable date range search in AtoM
* Assign the three major themes (Canadian Pacific Railway Company, Immigration and Settlement, Early British Columbia History) to every item
  * Krisztina: Please think about how to manage the large amount of data going into AtoM in a way that will enhance the user experience.   Is it practical to do it this way?  Should we attempt to identify sub-series, or at this stage is it better to stick to the three themes?  
* Identify and assign location to items currently without one

