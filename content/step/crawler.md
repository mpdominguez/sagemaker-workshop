---
title: "Create the Crawler"
chapter: false
weight: 11 
---

To use this csv information in the context of a Glue ETL, first we have to create a Glue crawler pointing to the location of each file. The crawler will try to figure out the data types of each column. The safest way to do this process is to create one crawler for each table pointing to a different location.

* Go to the AWS Console.
* Select under Services AWS Glue.
* Or follow this link! https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=crawlers
* Under crawlers Add Crawler and two crawlers: create one pointing to each S3 location (one to billing and one to reseller)

    * Crawler Name: Billing - Next
    * Crawler source type: Data Store - Next
    * Add a data store: S3, Specific Path in my Account, Navigate to your bucket and your folder Billing - Next
    * Add another data store: no - Next
    * Choose an IAM role: create an IAM role billing-crawler-role (if exists, choose the existing) - Next
    * Frequency: run on demand - Next
    * Crawler's output: Add database implementationdb - Next
    * Finish

* After the crawler is created select Run it now.
* Let's add the second crawler:

    * Crawler Name: Reseller - Next
    * Crawler source type: Data Store - Next
    * Add a data store: S3, Specific Path in my Account, Navigate to your bucket and your folder Reseller - Next
    * Add another data store: no - Next
    * Choose an IAM role: create an IAM role reseller-crawler-role (if exists, choose the existing) - Next
    * Frequency: run on demand - Next
    * Crawlers's output: Select database implementationdb - Next
    * Finish
    
* After the crawler is created select Run it now.

After both crawlers run you should see one table is been adeed for each. You can use Athena to inspect the tables and double check the data is been added properly.
Follow this link to ckeck https://us-east-1.console.aws.amazon.com/athena/home?force&region=us-east-1#query
And run these two queries
```
select * from billing
```
Separately
```
select * from reseller
```
