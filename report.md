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
  - [ ] [Creators Cleaning](##creators-cleaning)
* [Outcomes](#outcomes)
* [Range of Access Identifiers](#range-of-access-identifiers)
* [Appendix](#appendix)

## Background
UBC Rare Books and Special Collections (RBSC), UBC Library is working with Digital Initiatives at UBC library to decommission the current Chung database and move information to a complete system of record (AtoM software). The Chung collection is a major asset to UBC, and holds over 25, 000 items that document immigration and settlement to British Columbia, the Canadian Pacific Railway, and early exploration.  The collection is particularly rich in its documentation of the Canadian-Chinese diaspora.   RBSC is working with Digital Initiatives (DI) at UBC library to decommission the current Chung database and move information to a complete system of record (AtoM software). 
Introduction on the dataset
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
The goal of the project is to have a full list of current collections available for import into AtoM, the new metadata management system. This list should meet the following requirements by the end of the project:
* The format of the data should comply with the current Canadian Rules of Description
* The list should include all collections that have been described or received in the Chung Collection, though gaps may still exist for missing items
* The list need to ensure that all description information for the items are correct, including
  * No discrepancy between RBSC and DI dataset on fields to import to AtoM (Access Identifier, Title, Alternative Title, Location, Notes, Creator, Description, Date Created, Date Issued, Language, Physical Description)
  * Locations are correctly assigned for existing items, which means all items should be assigned a location. If the item is found missing, the location field should be indicated as “missing”, while the original location should be recorded in “Staff_Notes” field.
  * The information described the items should keep consistent all the way, i.e. use the same format and for the same attribute, and the same name for the same entity (e.g. creators)
* The list should assign an Access Identifier for each item as the single identifier for future management, i.e. 
  * There are no duplicated identifiers
  * New items could be easily assigned and Access Identifier in compliance with the current format
* The list should be easy to maintain even if the administrator changed, i.e.
  * The list is in a easy to manipulate format
  * Current management rules should be written down

## Major Tasks
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

## Methods

### Merge RBSC, DI and EX datasets
The original RBSC dataset has 19875 records, and the DI dataset has 19023 records. I updated some of the identifiers based on the exhibition items list file. After a full join on the two datasets, 19981 records
See “T:\RBSC\PE files (staff files)\PE Claire Cui\Merged datasets\2018-05-17_MergingMethod.docx”

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

### Identify possible gaps
Through skipped identifiers and locations; audit number of items, etc.
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
* 

## Outcomes
### In spreadsheet
* No duplicated identifiers
* Identified skipped locations
* Identified skipped identifiers of artifacts (CC-AR), photos (CC-PH, not include photo albums), CC-GR and CC-OS.

### Collection Arrangement
* Collection: Chung Collection
  * Series: Immigration and Settlement
    * Subseries: Chinese immigration and settlement to Canada (Chung Categories: “The Asian Experience in North America” and “Images of Chinese People and Communities in North America”)
    * Subseries: The Yip Family and Yip Sang Company
    * Subseries: Chinese Freemasons (Cheekungtong)
    * Subseries: Clandonald and Scottish immigration to Canada
  * Series: Early British Columbia and Canadian History 
    * Subseries: Early British Columbia History (Chung Category: British Columbia historical documents, photographs and artifacts)
    * Subseries: Early Canadian History
  * Series: Canadian Pacific Railway Company
    * Subseries: Canadian Pacific Railway
    * Subseries: C.P.R. steamships
    * Subseries: Travel and Tourism with the C.P.R.
    * Subseries: British Columbia Coast Steamship Service
    * Subseries: Artwork and images of the C.P.R.
    * Subseries: C.P.R. artefacts
    * Subseries: Working for the C.P.R.
    * Subseries: Esquimalt and Nanaimo Railway Company
  * Series: Canadian transportation

### Major tasks finished
- [x] A full list of metadata available: RBSC+DI
- [x] Update identifiers: EX identifiers
- [x] Duplicates removed:
- [x] Library items removed: 
- [x] RBSC and DI discrepancies checked: Title, Location, Date, Description, Notes, Physical description
- [x] Album/photo links established using parentID field.
- [x] Assign unique categories for all records
- [x] Add skipped identifiers
- [x] Creators Cleaning:  unify different forms of creators; match with AtoM authority list (except for CPR related creators)
- [x] Add new columns and fill in information: GMD, level of description, start date and end date, language_CSV
- [x] Add columns to mark changes: Status, Notes_Merging_CC, RBSC_Staff_Notes
- [x] Keep logs: master daily backup, Updates Log, Co-op work log

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
