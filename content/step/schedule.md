---
title: "Schedule the State Machine"
chapter: false
weight: 18
---
Well! we have all greens on our state machine, time to put the cron in order to forget about this

We are going to use Cloudwacth Event Rules  to create a scheduled task in order to execute this once a day.

Follow this link:  https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#rules:action=create

And we fill the form like this:
* Event Source: Schedule
* Cron expression: 0 10 * * ? * (every day at 10 AM UTC)
* Targets: Step Functions state machine
    * State machine: the name of your state machine
    * Create a new role for this specific resource - Configure details
    * Name: MLcron - Create rule

And that's it! the rule will be triggered in the specific time every day.
More examples of cron expressions here:
https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions
