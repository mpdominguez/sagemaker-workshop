---
title: "Create the Crawler"
chapter: false
weight: 11 
---

To use this csv information in the context of a Glue ETL, first we have to create a Glue crawler pointing to the location of each file. The crawler will try to figure out the data types of each column. The safest way to do this process is to create one crawler for each table pointing to a different location.

* Go to the AWS Console.
* Select under Services AWS Glue.
* Under crawlers Add Crawler and two crawlers: create one pointing to each S3 location (one to billing and one to reseller)

    * Name: Billing, Data Store, Specific Path in my Account, Navigate to your bucket and your folder Billing, create an IAM role billing-crawler-role, add database implementationdb, Next, Finish

    * After the crawler is created select Run it now.

    *  Name: Reseller, Data Store, Specific Path in my Account, Navigate to your bucket and your folder Reseller, create an IAM role reseller-crawler-role, select database implementationdb, Next, Finish

    *  After the crawler is created select Run it now.

After both crawlers run you should see one table is been adeed for each. You can use Athena to inspect the tables and double check the data is been added properly.
