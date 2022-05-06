

<h1> Human-made Climate Change discussion on Reddit </h1>
- Albert K. Jacobsen, Aron D. Jacobsen & Phillip C. Højbjerg

<center>
<img src="https://media.vocativ.com/photos/2015/01/Reddit-Climate-Change-Poster2445147325.jpg" alt="Reddit Climate Change" width="600"/>
</center>


This blog-post investigates the phenomena of Climate Change, more specifically the "man-made or not"-discussion on Reddit and which factors influence the opinion of Redditors regarding the Climate Change discussion.

The investigations are based on submissions and comments posted on Reddit, in the period of Fall 2014 to Spring 2022 - the period between the release of the 5th and 6th edition of the IPCC assessment report. The data consists of **1,020,327** comments and **572,747** submissions about climate change, from a total of **345,645** Redditors from a multitude of subreddits. Each comment has been labeled with an opinion score through machine learning, either Pro, Anti or Neutral - understood as either believing, not believing or being neutral about man-made climate change. In addition, real world data, in the shape of 1704 natural disasters in that same period, and the severity of these based on the amount of people affected has been downloaded from The international disasters database, EM-DAT. All of the data is available [here](https://drive.google.com/drive/folders/1e2uLI2JjoN1DJW5UrvhNofq_fbWcLBev), while the details of our analysis can be accessed through our [GitHub](https://github.com/albertkjoller/Reddit-ClimateGraph).

The investigation will take us through the evolution of the opinion of Redditors through time, as well as look at some of the multiple different factors that could affect the opinion of the Redditors - namely the amount of engagement on the relevant subreddits of the social network, the severity of natural disasters in the real world, as well as whether the opinion of Redditors are affected by what is known as echo-chambers, or specific authorities in the social network.

So let's get started!

Firstly, we'd like to see how our data evolves on a yearly basis. The top-most plot of the following shows how the size of the network evolves through time, scattered with regards to the average yearly opinion score, ranging from -1 to 1, Anti to Pro. The bottom-most plot is a more informative measure of the amount of Redditors per year.

{% include TemporalEvolution.html %}

The plot clearly shows that the discussion of climate on Reddit has been growing throughout the specific period. Note the tiny dot present in 2014, compared to the moderately sized point of 2022 - based on only 4 months of data. Furthermore it seems as though the general opinion is upward-trending, though minimally so. The average yearly opinion of the entire period is on the anti-side of the scale, very close to neutral however.

But one begins to wonder; is the upward trend due to Redditors changing their opinion, or is it simply due to the opinion of new Redditors joining the discussion.

This is tested by applying a paired statistical test on all Redditors present in any two subsequent years, throughout the entire period. In other words, we test whether each Redditor, present in the network in any two subsequent years, has a significant change of opinion. Since the opinion score distributions from year to year don't follow a Gaussian distribution, the statistical test of choice is the Wilcoxon Signed Rank-Sum test (aka. Wilcoxon paired t-test).

|                         | **2014-2015** | **2015-2016** | **2016-2017** | **2017-2018** | **2018-2019** | **2019-2020** | **2020-2021** | **2021-2022** |
|-------------------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|
| **Recurring Redditors** | 589           | 3066          | 3591          | 4048          | 8257          | 9763          | 7359          | 3976          |
| **Changed Opinion**     | False         | False         | False         | True          | True          | True          | True          | False         |

<!---
| **P-value**             | 0.78          | 0.17          | 0.88          | 0.019         | 0.00036       | 0.00061       | 0.00037       | 0.50          |
--->

Though we can't say much about the years prior, we see that the opinion of the specific Redditors change from year to year in the period of 2017-2021, with 95% confidence!

However, what could cause a change of opinion among the Redditors? - Is it simply due to an influx of Reddit submissions or comments, or could outside sources such as Natural Disasters be the reason of change?

We will try to answer this by diving deeper into the data, looking at the average *daily* opinion of the Redditors! - zooming in will provide you the individual dates.

{% include Fig1hypeVsOpinion.html %}

Plotting the daily opinion scores underneath the daily number of posts (comments and submissions), don't immediately provide any proof of one affecting the other. In fact, they don't seem to be correlated at all! Plotting them, one as a function of the other however, seems to tell a different story.

{% include Fig2numPostsOpinion.html %}

Now this is more interesting! The plot seems to show that days of *negative* avg. opinion, i.e. not believing in man-made climate change, contain way more posts than days of Pro opinion - finally, a Pearson correlation test agrees, meaning that the two are correlated!
 <!---with a p-value of 0.0000 --->

Next - we'd like to see how outside sources, namely Natural Disasters, affect the opinion of the Redditors.

{% include fig3naturalDisasters.html %}

Scattering the various events provided from the EMDAT dataset on top of the previously seen plot, provides a timeline of the various natural disasters. It clearly shows how disaster-types appear in groups, at different times of year - however one would expect the events to stay within the local minima- and maxima- if they were to influence the opinion of Redditors, something we don't see in this instance. Once again, it is advised to zoom into the plot to look at the individually scattered points.

But just taking the time of event into account might not be enough - instead we can plot the number of posts, as well as the opinion, as a function of the **Severity** of the disaster - understood as the average amount of people affected by disasters on any given day. The plots are plotted in log-log scale for easier visualization.

{% include fig5_SeverityVS.html %}

The two plots seem to show the same story, namely that the severity of the natural disasters, don't affect either number of posts or opinion of the Redditors. However, the Pearson Correlation disagrees! Based on Pearson correlation test, we can't conclude any correlation based on the *opinion* (with 95% confidence) - but we *can* say that the severity is correlated with the number of posts!
<!--- of p-value 0.023 and 0.16 respectively --->

Thinking of the kinds of data we have available, we can say with certainty that the number of posts, don't influence the severity of Natural Disasters - at least we hope not, as that would mean that posting on Reddit strengthens natural disasters :O)

But the opposite way of thinking is extremely interesting - do the severity of natural disasters directly affect the size of debate on Reddit or maybe even the opinion? - The perfect candidate for a Granger causality test!

{% include fig6_Granger_3plots.html %}

The Granger Causality test is a test which indicates whether one time-series can help forecasting another - and as such could also be thought of as incorporating some form of attention span into the causality test, as we know the two won't happen simultaneously! Thus, what we'd like to check is whether the data within the downmost subplot is affecting the trends in the two topmost subplots.

After making sure that the time-series are stationary - independent of time - through an ADF-test, we can perform the Granger causality test.
<!---
| Granger-test        | **P-value** |
|---------------------|-------------|
| Severity -> Posts   | 0.28        |
| Severity -> Opinion | 0.84        |
--->
| Granger-test        | **X causes Y?** |
|---------------------|-----------------|
| Severity -> Posts   | False           |
| Severity -> Opinion | False           |

However, the Granger causality tests suggest that the Severity don't Granger-cause neither number of posts or opinion of the Redditors. As such, though the amount of posts and the severity is correlated, we can't prove that one is affecting the other.

So far, we have looked at whether the daily opinion of Redditors is affected by the number of posts or outside events, such as natural disasters. Now, our investigation will take a shift - For the next part of the investigation, we'll tighten our scope by only taking the year of 2020 into consideration, as we'd like to look at the interaction *between* Redditors in a single year, and how the opinion of one can affect the opinions of others.

**Echo-chambers of opinion**

To do so, we need to visualize our Reddit data as a network!

{% include ZoomNetwork1.html %}

The visualisation shows the network of Redditors, where each link corresponds to a correspondence between them, either through *replying* or *being replied to* in any of the submissions or comments collected. The colors correspond to the average opinion of each Redditor, based on the average opinion of all their posts.

* An average score of <img src="https://render.githubusercontent.com/render/math?math=$[-1,-1/3]$"> indicates an anti man-made climate change opinion, and has been colored red
* A score of <img src="https://render.githubusercontent.com/render/math?math=$]-1/3, 1/3[$"> corresponds to neutral, which is colored beige
* While a score of <img src="https://render.githubusercontent.com/render/math?math=$[1/3,1]$"> indicates a pro man-made climate change opinion which, aptly, is colored green.

As is seen, the network is dominated by the beige color - which shouldn't come as a surprise as we've previously seen that the yearly avg. is close to 0. The top nodes come in all colors, thereby opinions, while typically being linked to a lot of smaller nodes. This is probably due to a popular submission with many commenters, though it could also be a Redditor commenting on a lot of smaller posts.

As mentioned, part of what we'd like to investigate is the concept of echo-chambers, i.e. environments where the opinion of users is reinforced by other users of similar opinion. This is explored through the concept of community structures - groups of nodes that are more connected internally than the rest of the network - as echo-chambers naturally would create some of these strongly interconnected nodes.

In order to find these so-called communities, the Louvain algorithm will be applied on the network. Next - we will look at the opinion of the nodes within them, as echo-chambers would be indicated by a dominance of pro or anti opinions. This investigation is based on *textual information* within communities, and as such we have, arbitrarily, excluded communities containing *less* than 80 Redditors.

{% include ZoomNetwork3.html %}

As seen in the plot above, the community sizes are described by a power-law, with most communities being of relatively small sizes, and as such, this decision - though we find it necessary - will result in a lot of information loss. With this cut-off we're left with the 100 largest communities, out of what started out as being 8706. However, since almost 8000 of the removed communities consisted of 3 nodes or less, this decision only results in a removal of 24.94% of the Redditors in the network.

{% include ZoomNetwork2.html %}

The final network, colored based on the Louivain partition, is seen above, while the original opinion-partition can be seen by clicking on the preview below the image. The modularity of the networks - the measure of how well a network is split into submodules - is summarized in the table below.

|                | **Original** | **Louvain** |
|:--------------:|:------------:|-------------|
| **Modularity** |    0.008     | 0.92        |

The table shows how the original partition of only 3 communities is an almost undivided network, meaning there don't seem to be much of a rule about who's talking to who based on opinion. The newly found partitions have way higher modularity, however, that comes as no surprise as it's been split into 100 different communities, in the best possible way according to Louvain.

Diving into each of these communities, we can take a look at the distribution of opinions within them. Ideally, if we were to prove echo-chambers, we'd like to see a majority of one opinion within each, however as seen below, that is not the case.

{% include ZoomNetwork4.html %}

First of all, the figure clearly shows a majority of neutral opinions within each community, something that comes as no surprise after seeing the network. The top-most plot is sorted based on size of each community, while the bottom-most has been normalized to show the distribution within each. Once only considering the distribution of Pro vs. Anti, there seem to be a trend of Anti dominating the communities - something we also saw hinted at way back in the first plot of the blog-post. In general, however, none of the communities indicate echo-chambers of any opinion.

In order to do *qualitative* analysis on the communities and their textual contents, we're diving further into 4 specific communities, one for each of the following criterias;

* Neutral: Community with the largest proportion of neutral opinions.

* Anti: Community with the largest proportion of anti opinions.

* Pro: Community with the largest proportion of pro opinions.

* Balanced: Community with the smallest difference between its anti and pro proportions.

{% include bar_all_4.html %}

The bar-plots showing the distribution of each of the 4 communities is seen above, while the wordclouds, based only on the unique words of each community, is visualized below.

{% include ZoomNetwork6.html %}

The Pro- wordcloud is full of words like 'forage', 'poultry', 'butcher' and 'science', while Anti- has words like 'superstitious', 'emission', 'repercussion' etc. - all of which are words you'd expect to see in a climate debate. The blue *balanced* wordcloud however, funnily enough, seems as though it's made up of characters and places from *Game of Thrones*, such as 'Qarth', 'Daenerys' and 'Ramsay'. Indicating that the Reddit querying keywords used,  might've had some overlap with the terminology of the Game of Thrones discussion on Reddit - This however, is only a single community out of an initial 8706.

Finally, the community-network of each community is visualized below.

{% include ZoomNetwork5.html %}

The second network, 'High on Anti', looks like it could indicate an echo-chamber at first, as the larger node has a majority of red-colored nodes around it. As none of the linked nodes have any connection to each-other, however, the structure more likely seems to resemble that of an *authority node* and its *hub*!

**Authorities and hubs**

The investigation of authority-nodes and the hubs surrounding them now brings us back to scale of the entire 2020 network graph. We're categorizing authority-nodes as Redditors of high importance, measured in total amount of Reddit-awards received per Redditor, as well as Redditors with high in-degree. The following will bring statistical tests on the opinion of both of these types of Authorities, compared to the hubs surrounding them - hoping to prove that the authority nodes are able to affect its surroundings. In other words - we want to see whether the opinion of Redditors is influenced by other Redditors of higher status.

First off - as Reddit-awards are the highest form of recognition a Redditor can receive, the *award-based* authorities are defined as any Redditor having 1 award or higher - this definition leaves us with 135 authority-Redditors in the Reddit-graph. Meanwhile, the *in-degree based* authorities are defined as any node with 50 or more in-going edges - meaning, any Redditor who has been answered by at least 50 other Redditors regarding the climate debate in the year of 2020 - leaving us with 98 authorities.

The award-based authorities have hubs of sizes 0 to 646, while the in-degree authorities have hubs of sizes 51 to 732, clearly influenced by the individual definition of authority we have given them - the ten "hubs" of size 0, and their corresponding authorities, have been removed from analysis, as empty hubs shouldn't be regarded as hubs:)

The distribution of opinion-score per authority, as well as the avg. opinion score of their corresponding hub is visualized for each of the authority-definitions in the plot below:

{% include ZoomNetwork7.html %}

The plot shows a clear tendency of the opinion-score of the authorities being scattered across a larger range than that of the hubs - which usually is closer to 0. This comes as no surprise, as the hubs are an average of a much larger group, and as such will easier be brought towards zero - as that is also the value of being *neutral* of opinion, which the majority of Redditors are. It could also be expected that posts of stronger opinion lead to more traction on the social network, and as such authorities would naturally be of a larger spectrum than that of the hubs.

The mean opinion-scores of the authorities as well as their hubs are seen below.

|                            | **In-degree** | **Awards** |
|----------------------------|:--------------:|:----------:|
| **Avg. Authority opinion** |    -0.028      |  -0.058    |
| **Avg. Hub opinion**       |    -0.057      |  -0.058    |


As seen, the mean values between hub and authority are quite similar, which come as no surprise after seeing the plot - suggesting that we won't be able to conclude that authorities are able to affect the opinion of their hubs. However, the analysis can't be done without a statistical test!

As the standard unpaired t-test is a parametric test, it means that the variance of the variables tested, should be of similar size. Calculating the variance of the two, we see the following;

|                            | **In-degree** | **Awards** |
|----------------------------|:--------------:|:----------:|
| **Var. Authority opinion** |    0.026       |  0.032     |
| **Var. Hub opinion**       |    0.002       |  0.006     |

However, the variance of authority- vs. hub-opinion can't be regarded similar for either of the authority-definitions. As such, we'll need the Welch's t-test!

|             | **Awards** | **In-degree** |
|-------------|:----------:|:-------------:|
| **P-value** |    0.99    |     0.086     |

The two statistical tests show very different results!! However, the null-hypothesis, being that authority opinion is not different from the hub-opinion, cannot be rejected in either of the two authority-definitions at a confidence level of 95%.

It is worth mentioning however, that a confidence level of 90% would flip the conclusion about the in-degree authorities! However, as the means of both the authority and hub are so close to being neutral, we can't possibly confidently say that authorities affect the opinion of others - even at 90% confidence.  

**Conclusion**

Thus concludes our analysis of the Climate Change debate on the social network of Reddit.

Though the analysis has taken us far and wide, into which factors can influence the opinion of Redditors regarding climate change, our statistical tests have time and time again been unable to reject our null-hypotheses.

We were however able to conclude that the opinion of Redditors regarding climate change was correlated with the amount of posts on any given day, while we also saw correlation between number of posts and the severity of climate change events on any given day, based on the amount of people affected!

However, though we saw correlation, we weren't able to conclude that the severity Granger-caused the number of posts, and much less was unable to conclude any correlation to the opinion.

Once taking the analysis into a yearly perspective, looking at the influence Redditors have on one another, both in the case of echo-chambers and high-authority Redditors, we weren't able to conclude what we'd hoped, namely that the opinion of Redditors is reinforced or affected by the opinion of others.

All our conclusions, however, heavily depend on whether we trust the decisions of our opinion classifier or not. And as such our conclusions would be more believable with a manually labeled opinion-dataset, however, this is simply not feasible when working with Big Data.
