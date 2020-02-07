---
title: "Agenda"
chapter: false
weight: 4
---

The steps below outline the process of building your own recommendation model, improving it, and then cleaning up all of your resources to prevent any unwanted charges. To get started executing these follow the steps in the next section.

1. ```Personalize_BuildCampaign.ipynb``` - Guides you through building your first campaign and recommendation algorithm.
2. ```View_Campaign_And_Interactions.ipynb``` - Showcase how to generate a recommendation and how to modify it with real time intent.
3. ```Cleanup.ipynb``` - Deletes anything that was created so you are not charged for additional resources.

## Personalize Recipes

HRNN - Hierarchical recurrent neural network (HRNN), which is able to model the changes in user behavior.

HRNN-Metadata - Similar to the HRNN recipe with additional features derived from contextual, user, and item metadata (Interactions, Users, and Items datasets, respectively). Provides accuracy benefits over non-metadata models when high quality metadata is available.

HRNN-Coldstart - Similar to the HRNN-Metadata recipe, while adding personalized exploration of new items. Use this recipe when you are frequently adding new items to the Items dataset and require the items to immediately appear in the recommendations.
Popularity-Count - Popularity-count returns the top popular items from a dataset. A popular item is defined by the number of times it occurs in the dataset. The recipe returns the same popular items for all users.

Personalized-Ranking - Provides a user with a ranked list of items.

SIMS - Leverages user-item interaction data to recommend items similar to a given item. In the absence of sufficient user behavior data for an item, this recipe recommends popular items.
