

<h1> Human-made Climate Change discussion on Reddit </h1>

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
An average score of <img src="https://render.githubusercontent.com/render/math?math=$[-1,-1/3]$"> indicates an anti man-made climate change opinion, and will be colored red. <img src="https://render.githubusercontent.com/render/math?math=$]-1/3, 1/3[$"> corresponds to neutral - beige, while <img src="https://render.githubusercontent.com/render/math?math=$[1/3,1]$"> indicates a pro man-made climate change opinion which, aptly, is colored green.

As is seen, the network is dominated by the beige color - which might come as a surprise to some. The top nodes come in all colors, thereby opinions, while typically being linked to a lot of smaller nodes. This is probably due to a popular submission with many commenters, though it could also be a Redditor commenting on a lot of smaller posts.

As mentioned, part of what we'd like to investigate is the concept of echo-chambers, i.e. environments where the opinion of users is reinforced by other users of similar opinion. This is explored through the concept of community structures - groups of nodes that are more connected internally than the rest of the network - as echo-chambers naturally would create some of these strongly interconnected nodes.

In order to find these so-called communities, the Louvain algorithm will be applied on the network. Next, we will look at the opinion of the nodes within them, as echo-chambers would be indicated by a dominance of pro or anti opinions.

{% include ZoomNetwork2.html %}



All our conclusions, however, heavily depend on whether we trust the decisions of our opinion classifier or not. And as such our conclusions would be more believable with a manually labeled opinion-dataset, however, this is simply not feasible when working with Big Data.
