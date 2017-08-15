# [Data Archiving ]()

## 1. Problem Statement

Due to our policy of never commiting destructive updates and having versioned changes, it's easy and useful to produce datasets in the hundreds of gigabytes or terabyte range, therefore we need support for using consistent snapshots as an input to a bulk data pipeline (without any possibility of affecting production traffic) as well as the ability to very quickly serve the output of them.

#### Goals
      
1. Need To Store Data Safely And Reliably and be able to retrieve data automatically based on pre-defined SLAs
      - Ability to Encrypt Sensitive Information - encrypt/obfuscate sensitive data based on retention policies
      - Ability to Authenticate - The archiving system should be able to authenticate the creation and integrity of documents in the archive.
      - Long-term Reliability - An effective archiving system must be reliable. If we don't have long-term reliability, we don't have a long-term archive.
2. Avoid any processing of HOT data from being impacted by processing of any other types mentioned above.
3. Store data optimally based on usage patterns and needs.
4. Need to save all types of transactional data, documents, logs, emails, notes etc.
5. Need to preserve large volumes of data
6. Need to search and retrieve based on SLAs
7. Need to preserve according to longitivity agreements/policies and then delete. For financial transactions that is normally 7 years.
8. Separate applications and data layers from data archiving and de-archiving needs and processes.
9. Need to manage data movement - archiving is really about controlling your data. Spanning across compliance, discovery, privacy and cost factors, control is paramount.
Drive control in the following ways:
      - Know what records and data we have
      - Know where our data resides
      - Seemlessly enable preservation of data when required
      - Ensure deletion of data as needed
      - Secure access to data
        
#### Functional Requirements
1. Classify data based on rules driven usage patterns. (frequency of updates, reads, last read ...)
2. Ability to classify data as sensitive and store it as encrypted data.
3. Define archiving policies and being able to plug in new policies.
4. Ability to visualize data location after and before archiving.

---

## 2. Proposal

We need to classify our data based on usage patterns and use the most optimal storage policies and mechanisms based on it. To determine how data should be stored we need to categorize tha data as 
      
 1. Frequent - used inter day, with heavy updates
 2. Regular - used intra-day, can change. E.g. an order being updated before being executed, a trade being amended before settlement.
 3. Periodic - changes occur on a periodic basis such has EOD position calculated and updated, weekly recons etc
 4. Occasional 
 5. Rare
 6. On-demand or Request
 7. Never - apart from compliance or legal enquiries
      
In general, all our data can be classified as;
### Types of Data (based on usage patterns)
- ***Hot***: used inter day and data is updated frequently
- ***Warm:*** not used frequently inter-day but used intra-day such as calculating start-of-day position.
- ***Cold:*** rarely changes but used mainly for historical reporting with with weekly or monthly frequency.
- ***Frozen:*** data that is needed by special access for compiance, audit etc. Will not be accessed via regular application and will have a different SLA.

We must also consider the time dimension. Over time, business requirements for data can change, with data assigned to different service levels and migrated to different storage tiers to reduce costs.  But the first and most important step is to classify the active data sets into the right service levels, and to place them on appropriate storage tiers when they're created.

The overall process flow for data archiving is illustrated in the following diagram:
![](Data-Archiving-Diagram.jpg)


---
## 3. Options for adapting existing solutions

---
## 4. Unresolved questions

##### 1. UI and Scheduling
This will require a good amount of ui work in order to define, restore, view and control backup data.

##### 2. Time Travelling in Restored Data
Users who would be interested in the option to back up and restore time travel data 

##### 3. Restoring a Fraction of a Table (Options)
- We may want first-class support for restoring a segment of primary keys (or a secondary index) out of a table in a backup.
- The user could be instructed to bring up a new cluster and restore the whole table. This could be used to dump only what they wanted.

##### 4. Schema and Data Compatibility

As data is Frozen and is stored in long term storage, the issue arises when database and schema upgrades makes it almost impossible to restore the data back from storage to the current database, when needed. Financial information should be stored for at least seven years, during which we may undergo several database and schema upgrades. Therefore we need to do the following;
 - Store data along with the schema that it is compatible with.
 - When we upgrade databases to major versions that is incompatible with previous versions, we need to preserve database software such that it can be used to recover data.
 - Ideally we should be able to store data and schemas in vendor neutral format so that it can be restored into any SQL/non-SQL  compliant database.

