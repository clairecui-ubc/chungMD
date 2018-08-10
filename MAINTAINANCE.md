# Maintainance of the master spreadsheet

## Syncronize the data in AtoM and Chung Master
There are two major copies of data for Chung Collection available now - one on AtoM, another locally with all metadata of Chung Collection stored in a local database. It is important to keep the two copies syncronized, in the meanwhile track changes of any updates.

### Proposed Workflow
1. If there are any information required to be updated, update it in AtoM. This allows multiple people working on the archival description.
2. Update the local Master Spreadsheet from AtoM export
  a. Export all data from Chung Collection from AtoM using Clipboard 
  b. Import the exported AtoM csv file to the database
  c. Check updated information using the query created in the database, including newly added records, removed records, and updated fields
  d. Execute related records using query in the database
3. Kept update log with "Updated on XXX" or "Added on XXX", etc.

