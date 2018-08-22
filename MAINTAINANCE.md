# Maintainance of the master spreadsheet

## Syncronize the data in AtoM and Chung Master
There are two major copies of data for Chung Collection available now - one on AtoM, another locally with all metadata of Chung Collection stored in a local database. It is important to keep the two copies syncronized, in the meanwhile track changes of any updates.

### Proposed Workflow
#### For a few records
1. When metadata of a few records require updates, update it in AtoM by edit the page in AtoM. This allows multiple people working on the archival description.
2. Update the local Master Spreadsheet from AtoM export
  a. Export all data from Chung Collection from AtoM using Clipboard
  b. Import the exported AtoM csv file to the database
  c. Check updated information using the query created in the database, including newly added records, removed records, and updated fields
  d. Execute related records using query in the database
3. Kept update log with "Updated on DD/MM/YYYY" or "Added on DD/MM/YYYY", etc.

#### For updates in batch
1. Prepare the CSV file with information to be updated
2. For some fields (physical description, scope and content), it could be updated through the second update method "update and skip the blank ones". Just make sure to include legacyID AND/OR identifier AND/OR identifier
3. For linked fields, such as dates, creators and locations, the "update" method can only add a new entry under the old ones, but the old entry will still be kept. Thus, in this case, it is recommended to use the "delete and replace" method. In this case, the "skip unmatched" should NOT be used.

## For Canadian Pacific Railway Company Related records
Since this is a follow up task, we need to think in a forward way, to allow the person who works on the creator cleaning could easily clean the data; and the authority list in AtoM also well-maintained. In this import, CPR related creators are all aggregated under a virtual authority record "Canadian Pacific Railway Company (Chung Collection)". After the CPR creator cleaning work is finished, this authority record could be deleted and all links would be removed automatically. It is worth noting that this creator is added after the actual creator, thus delete the authority record won't cause lost of creator information.
Plan (Krisztina, Jacky and Claire)