---
title: "Creating Parallel Solutions"
chapter: false
weight: 4
---
### Create Item-to-Item Similarities Solution 

Using the same methods as before, go to the Services drop-down in the console and navigate to the Amazon Personalize service in another tab, and select Dataset groups. You will see the dataset group that you created earlier, and click on the name of your dataset group.

![datasetGroups](/images/datasetGroups.png)

The left-hand side, which will show you the solution that you're currently creating via your notebook. Then, select Solutions and recipes, then click on the Create solution button.

![solutionList](/images/solutionList.png)

Enter a suitable name for this solution, such as **similar-items-solutions** and choose the ***aws-sims*** recipe and click Next - we don't need to change anything in the advanced configuration section

![solutionConfig](/images/solutionConfig.png)

In the "Solutions overview" screen just hit the ***Finish*** button and a new solution version will start to be created.

### Create Personal Ranking Solution

Let's do exactly the same thing again, but this time we'll create a ranking solition. From the Solutions and Recipes screen that you are on, click Create solution, give it a name like ***rankings-solution*** but this time select the ***aws-personalized-ranking*** recipe. Click ***Next*** and ***Finished*** as before.

![recipeRanking](/images/recipeRanking.png)

You now have three solutions being built off of the same dataset, and all three will slot into the application later. Please now go back to the notebook and continue to build your recommendation campaign and do some quick testing - if the notebook solution still hasn't completed then you may continue with the first part of the next section.
