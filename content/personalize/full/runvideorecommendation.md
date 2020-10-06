---
title: "Running the Video Recommendation App"
chapter: false
weight: 6
---
## Running the Video Recommendation App 

You are now ready to run the application server! Simply follow the steps below to run the runmyserver script, and you should see status messages appearing quickly - these initial ones are the Load Balancer health-checks, and after a minute or so the instance should be declared healthy by the Load Balancer Target Group. Note, you will see some warnings around the psycopg2 component, but this can be ignored.

_Steps to execute the runmyserver script_

1. Go to the console, to EC2 services and click on instances to find out the public DNS of the EC2 instance created by CF
2. SSH , using the key created at lab start, to the EC2 instance 
3. Became "root" to move to the app location
4. Change dir to /root/personalize-video-recs/videorecs/
5. Execute runmyserver

![appFrontScreen](/images/runMyServer.png)

The URL of the server is your ALB followed by the /recommend/ path, although there is also an /admin/ path configured that we'll use later. For now connect to your server - in my example the server can be found at http://TestS-Appli-ADS60FMCKPMG-1862985075.us-east-1.elb.amazonaws.com/recommend

You should see the following screen in your browser - no Model Precision Metrics are available, as we haven't added any models yet to the application. You can also see that documentation for this is present, but be aware that it may not be 100% up to date with coding changes on the demo.

![appFrontScreen](/images/appFrontScreen.png)

If you hit Select Random User then you'll be taken to the main Recommendation screen, which starts by showing you a random user's top-25 movie review titles. However, you'll see on the Model dropdown on the left that there are no models available, and if you change the Personalize Mode to either Personal Ranking or Similar Items then it's the same story - you can see the movie reviews, and most-popular titles in a genre, but no recommendations. We need to get the solutions and campaigns built in the notebook, then you can come back and plug in the models.

![appRecNoModels](/images/appRecNoModels.png)

At this point we require the solution that is being built in the notebook to complete and the associated campaign to have been created - until that time we cannot move forward, so you may wish to get some refreshments if you are still waiting for those two steps to complete.
