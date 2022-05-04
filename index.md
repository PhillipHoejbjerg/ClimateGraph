

<h1> Human-made Climate Change discussion on Reddit </h1>

![Reddit Climate Change](docs/assets/Reddit-Climate-Change-Poster2445147325.jpegdocs/assets/Reddit-Climate-Change-Poster2445147325.jpeg)

This blog-post investigates the phenomena of Climate Change, more specifically the "man-made or not"-discussion on Reddit and which factors influence the opinion of Redditors regarding the Climate Change discussion.

The investigations are based on submissions and comments posted on Reddit, in the period of Fall 2014 to Spring 2022 - the period between the release of the 5th and 6th edition of the IPCC assessment report. The data consists of ??? comments and ??? submissions about climate change, from a total of ??? Redditors from a multitude of subreddits. Each comment has been labeled with an opinion score through machine learning, either Pro, Anti or Neutral - understood as either believing, not believing or being neutral about man-made climate change. In addition, real world data, in the shape of 1704 natural disasters in that same period, and the severity of these based on the amount of people affected has been downloaded from The international disasters database, EM-DAT. The data is available [here](https://drive.google.com/drive/folders/1e2uLI2JjoN1DJW5UrvhNofq_fbWcLBev), while the details of our analysis can be accessed through our [GitHub](https://github.com/albertkjoller/Reddit-ClimateGraph).

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
| **P-value**             | 0.78          | 0.17          | 0.88          | 0.019         | 0.00036       | 0.00061       | 0.00037       | 0.50          |
| **Changed Opinion**     | False         | False         | False         | True          | True          | True          | True          | False         |

Though we can't say much about the years prior, we see that the opinion of the specific Redditors change from year to year in the period of 2017-2021, with 95% confidence!

However, what could cause a change of opinion among the Redditors? - Is it simply due to an influx of Reddit submissions or comments, or could outside sources such as Natural Disasters be the reason of change?

We will try to answer this by diving deeper into the data, looking at the average *daily* opinion of the Redditors! - zooming in will provide you the individual dates.

{% include Fig1hypeVsOpinion.html %}

Plotting the daily opinion scores underneath the daily number of posts (comments and submissions) don't immediately provide any proof of one affecting the other. In fact, they don't seem to be correlated at all! Plotting them, one as a function of the other however, seems to tell a different story.

{% include Fig2numPostsOpinion.html %}

Now this is more interesting! The plot seems to show that days of *negative* avg. opinion, i.e. not believing in man-made climate change, contain way more posts than days of Pro opinion - finally, a Pearson correlation test with a p-value of 0.0000 agrees, the two are correlated!

Next - we'd like to see how outside sources, namely Natural Disasters, affect the opinion of the Redditors.

{% include fig3naturalDisasters.html %}

Scattering the various events provided from the EMDAT dataset on top of the previously seen plot, provides a timeline of the various natural disasters. It clearly shows how disaster-types appear in groups, at different times of year - however one would expect the events to stay within the local minima- and maxima- if they were to influence the opinion of Redditors, something we don't see in this instance. Once again, it is advised to zoom into the plot to look at the individually scattered points.

But just taking the time of event into account might not be enough - instead we can plot the number of posts, as well as the opinion, as a function of the **Severity** of the disaster - understood as the average amount of people affected by disasters on any given day. The plots are plotted in log-log scale for easier visualization.

{% include fig5_SeverityVS.html %}

The two plots seem to show the same story, namely that the severity of the natural disasters, don't affect either number of posts or opinion of the Redditors. However, the Pearson Correlations disagrees! With Pearson correlations of p-value 0.023 and 0.16 respectively, we can't conclude any correlation based on the opinion (with 95% confidence) - but we *can* say that the severity is correlated with the number of posts!

Thinking of the kinds of data we have available, we can say with certainty that the number of posts, don't influence the severity of Natural Disasters - at least we hope not, as that would mean that posting on Reddit strengthens natural disasters :O)

But the opposite way of thinking is extremely interesting - do the severity of natural disasters directly affect the size of debate on Reddit or maybe even the opinion? - The perfect candidate for a Granger causality test!

{% include fig6_Granger_3plots.html %}

The Granger Causality test is a test which indicates whether one time-series can help forecasting another - and as such could also be thought of as incorporating some form of attention span into the causality test, as we know the two won't happen simultaneously! Thus, what we'd like to check is whether the data within the downmost subplot is affecting the trends in the two topmost subplots.

After making sure that the time-series are stationary - independent of time - through an ADF-test, we can perform the Granger causality test.

| Granger-test        | **P-value** |
|---------------------|-------------|
| Severity -> Posts   | 0.28        |
| Severity -> Opinion | 0.84        |

However, the p-values suggests that the Severity don't Granger-cause neither number of posts or opinion of the Redditors. As such, though the amount of posts and the severity is correlated, we can't prove that one is affecting the other.

So far, we have looked at whether the daily opinion of Redditors is affected by the number of posts or outside events, such as natural disasters. Now, our investigation will take a shift - For the next part of the investigation, we'd like to look at the interaction *between* Redditors, and how the opinion of one can affect the opinions of others.

To do so, we need to visualize our Reddit data as a network!

{% include ZoomNetwork1.html %}


The visualisation shows the network of Redditors, where each link corresponds to a correspondence between them, either through replying or being replied to in any of the submissions or comments collected. The colors correspond to the average opinion of each Redditor, based on the average of all their posts.
An average score of <img src="https://render.githubusercontent.com/render/math?math=$[-1,-1/3]$"> indicates an anti man-made climate change opinion, and will be colored red. <img src="https://render.githubusercontent.com/render/math?math=$]-1/3, 1/3[$"> corresponds to neutral, beige, while <img src="https://render.githubusercontent.com/render/math?math=$[1/3,1]$"> indicates a pro man-made climate change opinion which, aptly, is colored green.

As is seen, the network is dominated by the beige color - which might come as a surprise to some. The top nodes come in all colors, thereby opinions, while typically being linked to a lot of smaller nodes. This is probably due to a popular submissions with many commenters, though it could also be a Redditors commenting on a lot of smaller posts.

As mentioned, part of what we'd like to investigate is the concept of echo-chambers, i.e. environments where the opinion of users is reinforced by other users of similar opinion. This is explored through the concept of community structures - groups of nodes that are more connected internally than the rest of the network - as echo-chambers naturally would create some of these strongly interconnected nodes.

In order to find these so-called communities, the Louvain algorithm will be applied on the network. Next, we will look at the opinion of the nodes within them, as echo-chambers would be indicated by a dominance of pro or anti opinions. This is based on *textual information* within communities, and as such we have, arbitrarily, excluded communities containing *less* than 80 nodes.

{% include ZoomNetwork3.html %}

As seen in the plot above, the community sizes are described by a power-law, with most communities being of relatively small sizes, and as such, this decision - though we find it necessary - will result in a lot of information loss. With this cut-off we're left with the 100 largest communities, out of what started out as being 8706. However, since almost 8000 of the removed communities consisted of 3 nodes or less, this decision only results in a removal of 24.94% of the Redditors in the network.

{% include ZoomNetwork2.html %}

The final network, colored based on the Louivain partition, is seen above, while the original opinion-partition can be seen by clicking on the preview below the image. The modularity of the networks - the measure of how well a network is split into submodules - is summarized in the table below.

|                | **Original** | **Louvain** |
|:--------------:|:------------:|-------------|
| **Modularity** |    0.0082    | 0.92        |

The table shows how the original partition of only 3 communities is an almost undivided network, meaning there don't seem to be much of a rule about who's talking to who based on opinion. The newly found partitions have way higher modularity, however, that comes as no surprise as it's been split into 100 different communities.

Diving into each of these communities, we can take a look at the distribution of opinions within them. Ideally, if we were to prove echo-chambers, we'd like to see a majority of one opinion within each, however as seen below, that is not the case.

{% include ZoomNetwork4.html %}

First of all, the figure clearly shows a majority of neutral opinions within each community, something that comes as no surprise after seeing the network. The top-most plot is sorted based on size of each community, while the bottom-most has been normalized to show the distribution within each. Once only considering the distribution of Pro vs. Anti, there seem to be a trend of Anti dominating the communities - something we also saw way back in the first plot of the blog-post. In general, however, none of the communities indicate echo-chambers of any opinion.

**Diving deeper into communities**

In order to do qualitative analysis on the communities and their textual contents, we're diving further into 4 specific communities, based on the following criterias;

> Neutral: Community with the largest proportion of neutral opinions.
> Anti: Community with the largest proportion of anti opinions.
> Pro: Community with the largest proportion of pro opinions.
> Balanced: Community with the smallest difference between its anti and pro proportions.

{% include bar_all_4.html %}

The bar-plots showing the distribution of each of the 4 communities is seen above, while the wordclouds, based on only the unique words of each community, is visualized below.

{% include ZoomNetwork6.html %}

The Pro- wordcloud is full of words like 'forage', 'poultry', 'butcher', 'food' and 'science', while Anti- has words like 'superstitious', 'emission', 'repercussion' etc. - All words you'd expect to see in a climate debate. The blue *balanced* wordcloud however, funnily enough, seems as though it's made up of characters and places from *Game of Thrones*, such as 'Qarth', 'Daenerys' and 'Ramsay'.

Finally, the community-network of each community is visualized below. The second network, 'High on Anti', could indicate an echo-chamber, as the larger node has a majority of red-colored nodes around it. Overall, however, that is the only indication of any echo-chambers so far.

{% include ZoomNetwork5.html %}

The network could also indicate that the opinion of the hub is influenced by the larger middle-node, which is why we'll take a look at authorities and their effect on the hubs surrounding them later in this blogpost.



All our conclusions, however, heavily depend on whether we trust the decisions of our opinion classifier or not. And as such our conclusions would be more believable with a manually labeled opinion-dataset, however, this is simply not feasible when working with Big Data.
