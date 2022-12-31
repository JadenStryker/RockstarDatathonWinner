# TEAM_GTA6
## Cluster Segmentation and Machine Learning: User profiles and percentile behavior

Our project consists on analyzing the behavior and nature of different percentiles of players, based on the time they engage with the video game. Given the considerable size of the data and the nature of time-serie dataset, most of the factors included were pretty noisy, and that imposed a problem into drawing conclusions about user profiles and overall behavior. Thus, we decided to analyze different percentiles and create variables that emcomposed insightful information from the three tables (by matching user’s IDs, and using ML models for time-series datapoints). Together with the creation of dummy variable representation of activities, we ran a logistic regression to see which activity clusters were meaningful for each percentile group. By doing so, we were able to clarify basic hypothesis regarding the behavior of each group, and finally identifying multiple user profiles using K-means clustering.

#### Models used: Logistic regression, K-means clustering, Random forest

### Variables (generated):
We created a variety of different variables from the existing datasets to better understand the data.

Play_time_per_day: Measures how long players spend in-game per day. We don’t want to penalize users who started playing at a later date than others, so we check when the first time each player recorded activity in the game and normalizing based on how long they have played. 

Diversity_activity: Represents how diverse the events choices of each player are. Of the 7 different event types we find how many different types the player has engaged in and normalize on a scale of 0-1

Diversity_items: Representing  how diverse the purchase choices of each player are. Similar to diverse_activity, we check how many of the 12 item types the player has acquired, and normalize from 1-12

Modal_activity: This variable checks which activity the player has engaged in the most.

Modal_item: This variable checks which item the player has acquired the most of.

Force: Measuring how often a player is taking a break from the game. In this variable we check every week in our 3 month period that the player has registered activity in the game. We then check for gaps between the weeks that they have played. The longer a gap the player takes, the more they are penalized. We choose to penalize the player using an exponential model. At the very end, we take the log of this value to change the scale to something more useful. Similar to play_time_per_day, we do not punish players for having started later than others.

Daily_money_spent: Measures how much in game money players are spending per day. Once again, not punishing players for starting later.

Deposit: Represents whether the player has spent real money on the game.


### Findings:

In game engagement: Playing a variety of event increases average playtime. While this may seem intuitive, it is worth noting that players that are engaging in a variety of activities are more involved in the game than users that are specifically dialed in on one or two different game modes.

Playing habits: Playing every week not only increases total playtime, but average playtime. While it was expected that playing every week increases every week increases total playtime, it is interesting that it also increase how long the players are playing per sitting. One other area of interest we found was that if we don’t account for start time. I.e. Players who start playing later are penalized more harshly, taking breaks actually increases the average amount of time spent playing the game for the super users. But for normal users there was little to no correlation.

User profiles: Through random forest and pearson correlation square, we found that the 90th percentiles (super users), force is not strongly correlated (negatively) with playtime, but it is stronger for the general population of players. This means that if a regular player takes breaks (meaning power is higher), it hurts them more in terms of average play time, in comparison to the 90th percentile. While through K-means clustering, we found two groups as the optimal cluster using elbow distribution and silhouette score. We indexed these groups and we found that it divides in user who “engage deeper with the game” by varying through activities and investing real money on the game, against people who don’t. 

Machine Learning Discoveries: In the k-means cluster we discovered the number of centroids is 2 through silhouette which is a measure of grouping in the data, as well as elbow which shows the score which optimizes a l2 loss. In this dual segmentation with k-means we can see a clear segmentation in time played per day on average, diversity metrics, and a super strong distinction with deposit. As expected a person who deposits real money into the game will have a higher number of involvement with the world. Thus our two segments are hyper-users(making up a small percentage of net players) vs more casual players. Fascinating enough is that we see mean force nearly the same between the two groups. Meaning that taking breaks doesn't always prevent someone from being a highly involved player. We also saw this in the logistic regression. As a note we immediately encounter a class imbalance problem due to the 10% of the data. This causes over fitting and bad classification. THis is why we used AUC as a criteria as it reveals bad classifications in the model. We were able to overcome this by weighting errors in the positive class according to their proportion, we brought AUC from .55 to .77 using this technique. By analyzing the betas we were able to see likelihood weight applied to the different values. These were normalized between 1 and 0 with min max scaling which is more effective for non normal distributions than a Standard Scalar. Thus the betas give a lot of intonation in the highly correlated world of the data compared to the direct correlation values between variables. We see for a hyper user force is negatively weighted against time spent. However, when looking at super users and non super users the force is 3 times more negatively correlated for average user then super user. Taking a break greatly reduced the time spent per day for an average, but does not for a super user. This leads us to questions of binge users and money spent in game. 




### Recommendations:

Given the time constraint a datathon presents, we weren’t able to get into deeper questions regarding the behavior of each percentile of players and user profiles. Here are some of the things that can be constructed from our dataset and we consider feasible goals:

Which activity does a user first do after not playing for a week or more, which will likely be the reason why they are coming back to the game for, providing insightful information into which activities have the higher returning rates.

We could identify the behavior of binge players that have high amounts of hours spend but at the same time, weeks of separation between each time they play, and see how they engage with the game.

Further hypothesis testing in seeing how significant certain player behavior characteristics are in yielding more time spent in the game, and in each activity

### Table of content:

Rahul’s contribution on partition by weeks, filling up keys not present across datasets with null values, and normalizing vectors for further investigation

Mateus’ contributions on setting up dummy variables for each different activity the user engages with and identifying the noise each component had

Carlos’ contribution on the data displayed on the presentation slides, using SQL for basic data analysis and behavior patterns in terms of money and time spent.

Jaden’s contributions on variable creation, ML techniques for standardization, cross validation (AUROC, useful for the challenges to outperform given the size of the data) and cluster segmentation of possible user’s profiles.

### How to install:

Everything is written on a single Google collab document that we will be adding a link to, as well as the data already pulled up in github repository. The code is solemnly based in Python, but in some parts we use a SQL interphase through python packages for data sorting.

### Credits:

Jaden Stryker, CS-DS, EGD 2023
Rahul Sawhney, CS-DS, EGD 2023
Mateus Aragao, CS-DS, EGD 2023
Carlos Figueroa, ECON-DS, EGD 2023

##### NYU-Rockstar datathon, Spring 2022.


