<h1> Human-made Climate Change discussion on Reddit </h1>

This blog-post investigates the phenomena of Climate Change, more specifically the "man-made or not"-discussion and which factors influence the opinion of Redditors regarding the Climate Change discussion.

The investigations are based on submissions and comments posted on Reddit, in the period of Fall 2014 to Spring 2022 - the period between the release of the 5th and 6th edition of the IPCC assessment report. The data consists of ??? comments and ??? submissions about climate change, from a total of ??? Redditors from a multitude of subreddits. In addition, real world data, in the shape of 1704 natural disasters in that same period, and the severity of these based on the amount of people affected has been downloaded from The international disasters database, EM-DAT. The investigation will take us through the evolution of the opinion of Redditors through time, as well as look at some of the multiple different factors that could affect the opinion of the Redditors - namely the amount of engagement on the relevant parts of the social network, the severity of natural disasters in the real world, as well as whether the opinion of Redditors are affected by what is known as echo-chambers, or specific authorities in the social network.

So let's get started!

Firstly, we'd like to see how our data evolves on a yearly basis. The following plot shows how the size of the network evolves through time, scattered with regards to the average yearly opinion score, ranging from -1 (Anti-) to 1 (Pro- climate change opinion).

{% include Fig0sizePerYear.html %}

The plot clearly shows that the discussion of climate on Reddit has been growing throughout the specific period, note the tiny dot present in 2014, compared to the moderately sized point of 2022 - based on only 4 months of data. Furthermore it seems as though the general opinion is upward-trending, though minimally so. The average yearly opinion of the entire period is on the anti-side of the scale, very close to neutral however.

Diving further into the data, we can statistically conclude whether the opinion of specific authors change year to year. We do so by applying a paired statistical test on all authors overlapping in any two subsequent years, throughout the entire period. Since the opinion score distributions from year to year do not follow a Gaussian distribution, the statistical test of choice is the Wilcoxon Signed Rank-Sum test (aka. Wilcoxon paired t-test).

|                         | **2014-2015** | **2015-2016** | **2016-2017** | **2017-2018** | **2018-2019** | **2019-2020** | **2020-2021** | **2021-2022** |
|-------------------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|
| **Overlapping Authors** | 589           | 3066          | 3591          | 4048          | 8257          | 9763          | 7359          | 3976          |
| **P-value**             | 0.78          | 0.17          | 0.88          | 0.019         | 0.00036       | 0.00061       | 0.00037       | 0.50          |
| **Changed Opinion**     | False         | False         | False         | True          | True          | True          | True          | False         |


considering the authors overlapping in the networks of any two subsequent years, applying a paired statistical test on all authors

Hvis vi kigger på de specifikke authors der overlapper år til år, kan vi måske se om det er fordi disse skiifter holdning

Diving further into the data,

{% include Fig1hypeVsOpinion.html %}
