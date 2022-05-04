<h1> Human-made Climate Change discussion on Reddit </h1>

This blog-post investigates the phenomena of Climate Change, more specifically the "man-made or not"-discussion on Reddit and which factors influence the opinion of Redditors regarding the Climate Change discussion.

The investigations are based on submissions and comments posted on Reddit, in the period of Fall 2014 to Spring 2022 - the period between the release of the 5th and 6th edition of the IPCC assessment report. The data consists of ??? comments and ??? submissions about climate change, from a total of ??? Redditors from a multitude of subreddits. In addition, real world data, in the shape of 1704 natural disasters in that same period, and the severity of these based on the amount of people affected has been downloaded from The international disasters database, EM-DAT. The investigation will take us through the evolution of the opinion of Redditors through time, as well as look at some of the multiple different factors that could affect the opinion of the Redditors - namely the amount of engagement on the relevant parts of the social network, the severity of natural disasters in the real world, as well as whether the opinion of Redditors are affected by what is known as echo-chambers, or specific authorities in the social network.

So let's get started!

Firstly, we'd like to see how our data evolves on a yearly basis. The following plot shows how the size of the network evolves through time, scattered with regards to the average yearly opinion score, ranging from -1, Anti, to 1, Pro man-made climate change opinion.

{% include Fig0sizePerYear.html %}

The plot clearly shows that the discussion of climate on Reddit has been growing throughout the specific period, note the tiny dot present in 2014, compared to the moderately sized point of 2022 - based on only 4 months of data. Furthermore it seems as though the general opinion is upward-trending, though minimally so. The average yearly opinion of the entire period is on the anti-side of the scale, very close to neutral however.

But one begins to wonder; is the upward trend due to Redditors changing their opinion, or is it simply due to the opinion of new Redditors joining the discussion.

This is tested by applying a paired statistical test on all Redditors overlapping in any two subsequent years, throughout the entire period. In other words, we test whether each Redditor, present in the network in any two subsequent years, has a significant change of opinion. Since the opinion score distributions from year to year don't follow a Gaussian distribution, the statistical test of choice is the Wilcoxon Signed Rank-Sum test (aka. Wilcoxon paired t-test).

|                         | **2014-2015** | **2015-2016** | **2016-2017** | **2017-2018** | **2018-2019** | **2019-2020** | **2020-2021** | **2021-2022** |
|-------------------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|
| **Recurring Redditors** | 589           | 3066          | 3591          | 4048          | 8257          | 9763          | 7359          | 3976          |
| **P-value**             | 0.78          | 0.17          | 0.88          | 0.019         | 0.00036       | 0.00061       | 0.00037       | 0.50          |
| **Changed Opinion**     | False         | False         | False         | True          | True          | True          | True          | False         |

Though we can't say much about the years prior, we see that the opinion of the specific Redditors change from year to year in the period of 2017-2021, with 95% confidence!

However, what could cause a change of opinion among the Redditors? - Is it simply due to an influx of Reddit submissions or comments, or could outside sources such as Natural Disasters be the reason of the change?

We will try to answer this by diving deeper into the data, looking at the average daily opinion of the Redditors! - zooming in provides the individual dates.

{% include Fig1hypeVsOpinion.html %}

Plotting the daily opinion scores underneath the daily number of posts don't immediately provide any proof of one affecting the other. In fact, they don't seem to be correlated at all! Plotting them, one as a function of the other however, seems to tell a different story.

{% include Fig2numPostsOpinion.html %}

Now this is more interesting! The plot seems to show that days of *negative* avg. opinion, i.e. not believing in man-made climate change, contain way more posts than days of Pro opinion - finally, a Pearson correlation test with a p-value of 0.0000 agrees, the two are correlated!

Next - we'd like to see how outside sources, namely Natural Disasters, affect the opinion of the Redditors.

{% include fig3naturalDisasters.html %}

Scattering the various events provided from the EMDAT dataset on top of the previously seen plot, provides a timeline of the various natural disasters. It clearly shows how different disaster-types appear at different times of year - however one would expect the events to stay within the local minima- and maxima- if they were to influence the opinion of Redditors, something we don't see in this instance. Once again, it is advised to zoom into the plot to look at the individually scattered points.

But just taking the time of event into account might not be enough - instead we can plot the number of posts, as well as the opinion, as a function of the **Severity** of the disaster - understood as the average amount of people affected by disasters on any given day. The plots are plotted in log-log scale for easier visualization.

{% include fig4_affectedVsPosts.html %}

{% include fig5_affectedVsOpinion.html %}

The two plots seem show the same story, namely that the severity of the natural disasters, don't affect either number of posts or opinion of the Redditors. However, the Pearson Correlations disagrees! With Pearson correlations of p-value 0.023 and 0.16 respectively, we can't conclude any correlation based on the opinion (with 95% confidence) - but we **can** say that the severity is correlated with the number of posts!

Thinking of the kinds of data we have available, we can say with certainty that the number of posts, don't influence the severity of Natural Disasters - at least we hope not, as that would mean that posting on Reddit strengthens natural disasters :O)

But the opposite way of thinking is extremely interesting - do the severity of natural disasters directly affect the size of debate on Reddit or even the opinion? - The perfect candidate for a Granger causality test!

{% include fig6_Granger.html %}

The Granger test is a test which indicates whether one time-series can help forecasting another - and as such could also be thought of as incorporating some form of attention span into the causality test, as we know the two won't happen simultaneously! Thus, what we'd like to check is whether the data within the downmost subplot is affecting the trends in the two topmost subplots.

After making sure that the time-series are stationary - independent of time - through an ADF-test, we can perform the Granger causality test.

| Granger-test        | **P-value** |
|---------------------|-------------|
| Severity -> Posts   | 0.28        |
| Severity -> Opinion | 0.84        |

The p-values however suggests that Severity neither Granger-causes number of posts or the opinion of the Redditors. As such, though the two are correlated, we can't prove that one is affecting the other.




All our conclusions, however, heavily depend on whether we trust the decisions of our opinion classifier or not. And as such our conclusions would be more believable with a manually labeled opinion-dataset, however, this is simply not feasible when working with Big Data. 
