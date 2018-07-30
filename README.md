# Chung Collection Metadata Cleaning
This repository contains the codes used for Chung Metadata Migration Project of RBSC at UBC Library during May to August, 2018. 

## Creator Cleaning
The original data contains inconsistency of creator names due to historical accumulation of errors. In this project, two steps were taken to emilinate those errors.

### 1. Find similar creators
The first step is to identify potential diffferent forms of the same creator. Due to the large amount of data, it would be inefficient and inaccurate to find those creators manually one by one. Thus, I used the "fuzzywuzzy" library in python to calculate similarity rate between different creators. If the similar rate is high (85% or above), then it's possible that those two names are the same creator. I listed the test results in [/CreatorCleaning/fuzzy/FindSimilarCreators.ipynb](/CreatorCleaning/fuzzy/FindSimilarCreators.ipynb)

### 2. Replace creators
After a research on the creators name, we now have a list of old and new creators (`dict.csv`). We need to replace the creators' name in the Chung collection based on that dictionary. The detailed codes are in [/CreatorCleaning/UpdateCreators_0705.ipynb](../CreatorCleaning/UpdateCreators_0705.ipynb)

## Assign Start Dates and End Dates based on "Date Created"
To index the dates in AtoM, the new metadata management system to be used for Chung Collection, the start date and end date need to be added before uploading the data into the system. The detailed codes are in [/date/startAndEndDate.ipynb](../date/startAndEndDate.ipynb) 



