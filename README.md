# Chung Collection Metadata Cleaning
This repository contains the codes used for Chung Metadata Migration Project of RBSC at UBC Library during May to August, 2018. 

## Creator Cleaning
The original data contains inconsistency of creator names due to historical accumulation of errors. In this project, two steps were taken to emilinate those errors.

### 1. Find similar creators
The first step is to identify potential diffferent forms of the same creator. Due to the large amount of data, it would be inefficient and inaccurate to find those creators manually one by one. Thus, I used the "fuzzywuzzy" library in python to calculate similarity rate between different creators. If the similar rate is high (85% or above), then it's possible that those two names are the same creator. I listed the test results in `/CreatorCleaning/fuzzy/FindSimilarCreators.ipynb`


