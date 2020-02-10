---
layout: post
title:  "[Data Analysis] Exploring Edmonton Library Data"
date:   2020-02-07 10:00:00 -0700
categories: jekyll update
---

**Github repository for the project:** [click here](https://github.com/FernandoMeiraFilho/Edmonton-Library-EDA).

**Jupyter Notebook fast link:** [click here](https://github.com/FernandoMeiraFilho/Edmonton-Library-EDA/blob/master/Edmonton%20Library%20EDA.ipynb).


>_I got the idea to explore this dataset on the [BetaCityYEG Meetup](https://www.meetup.com/startupedmonton/events/vppzhrybcdbjc/) Community Bulletin Board of Tech Projects. If you are in Edmonton and want to start working in some interesting projects for the city, you should definetly check it out._

---
_
- [Intro](#intro)
- [The Data](#data)
- [The LRT and the Edmonton Library Scene](#success)
<!-- - [Publication year](#publication) -->
- [We are all about the Fiction here](#fiction)
- [The self-help effect and genre evolution](#evolution)
- [(Sort of) Conclusion](#conclusion)



## <a id="intro"> </a>
## **Intro**

So, this was a free exploration project. It means that no specific goal was demanded other than look into the data and try to find meaningful patterns. Due to the small amount of available features, I opted to make a data exploration focusing more on visualizing than applying statistical models, and try to answers some questions I already had :

- What are the all time successes?
- Are the successes consistent in all branches?
- **Is there any meaningful patterns concerning the genres?**
- Geographical exploration of genre distribution in the branches


## <a id="data"> </a>
## **The data**
This is an overall exploration of the [Edmonton Public Library Dataset](https://data.edmonton.ca/Community-Centres/Most-Popular-Books-by-Branch-Edmonton-Public-Libra/qdgm-hex6). A brief description of the dataset from the official provider:
>_"A snapshot of the top ten books with the highest number of holds at Edmonton Public Library, each week from 9 AM Monday to 9 AM Monday, organized by customer home library branch."_

For this specific exploration I added the **genre** and **publishing date** features (which was missing in the original dataset) using the [Google Books API for python](https://github.com/hoffmann/googlebooks). If you would like to see how this was done technically, please check the Github repo.

## <a id="success"> </a>
## **The LRT and the Edmonton Library Scene**

So, guess what this two images have in common?

![train](/assets/posts/library/train_girl.png)

It seems that they say a lot when we look for the all time successes in holdings:

![train](/assets/posts/library/allTimeHolds.png)

So, **The girl on the train is the all time favorite**!

The first thing that comes into mind is: is this result the same in the majority of branches or is this the influence of specific (and more crowded) branches?

One way to figure it out is to plot the number of holdings together with the _number of appearances in the weekly top 10 (that way we could weigh down crowded libraries and make their participation equal to others)_ :

 ![train](/assets/posts/library/hold_appear.png)

We could say that, for the top 3, this seems to make sense (more holds equals more appearances). But, when we expand this for the subsequent titles like _The subtle art of not giving a f*ck_, _Into the water_ and _12 rules for life_ this argument does not hold. This could be for 2 reasons:

1. These titles (with less appearances) could be fruit of a momentary trend.
2. The hold numbers for these titles could be concentrated in branches with natural elevated hold numbers.

Let's check for both hypothesis:

#### 1. Momentary trend hypothesis:

![train](/assets/posts/library/moment_trend.png)

Above we can see the top 4-6 books from our rank of the all time favorites. We can see that the only title here that could not configure for a momentary trend is the _The subtle art of not giving a f*ck_ - which was able to maintain the demand for years after it's launch (which was Sep 2016). But when we take a look at the other two, we can see that it was a spike effect in the year of their launch: 2017, for _Into the water_ and 2018 for _12 rules for life_.

#### 2. Concentration hypothesis:

To check this hypothesis, we need to do 2 things:

1. Check if the number of holds correlates somehow with more appearances in the top ranks of individual branches.
2. Check if the branch with the highest numbers for the individual title represents too much of the total.

![train](/assets/posts/library/concentration.png)

**What is the IDEAL column**: this is a suggestion of what would be the more logical guess (in an IDEAL world) of what the number of appearances **should** be. TL,DR: Before looking to any data, the natural guess here would be that the highest positions in the rank have more appearances in all branches - and it degrades linearly - the further you go in the rank.

**Whats is concentration in the chart**: The % of which the branch with greater holding numbers for that title, represents in the total number.

Here we can see that, it seems to exist a correlation between number of appearances and the position in the rank of holds, once that the blue bars (REAL) are, in the most part, close to the orange bars (IDEAL). Concerning concentration, 2 things stand out: first, the high number for the third book in the rank _Cambridge IELTs_, with more than 30% concentration in the Millwoods branch (people are trying hard here), and the fact that the top 4-6 has relative greater values than the rest. And, interesting to see that, the last positions in our rank rely a lot more than others in the concentration values to stand out.

**In summary**: These books have a lot of appearances, and although they rely more in concentration, the picture here is clear that these books are all time favorites in the majority of the branches.

**Conclusion on the top 15 of all time**: Even though the correlation between number of appearances seems to stand, after all of the above, we could add that the first books in the rank (top 6) rely not just on the participation on the majority of the branches but in **record demand** - only such a thing would explain why books with peaks in appearances like _Harry Potter and the curse child_ and _All the light you cannot see_ do not figure higher in the ranks.

<!-- ---
_

Before we close the successes part of this analysis: **do you think that, after all of the above, the title _The girl on the train_ represents a perennial and spread phenomena?**

I would't rush on this:

![train](/assets/posts/library/moment_trend2.png)

Wait a minute. The top 3, when we take off the equation the title _Cambridge IELTS_ , is all about the moment trend too! _The girl on train_ was launched in 2015 and had it's spike in the same year - almost the same situation for _The life changing magic of tidying up_ which launched in Oct 2014.
Does it say anything relevant about publication year vs hold numbers?

## <a id="publication"> </a>
## **Publication**

Indeed, there are some trends coming and going here:

![train](/assets/posts/library/pubyear.png)

So here we can see that in 2015, the majority of holds went for books published in previous years. This trend is reversed for the following years - 2016 and 2017. In 2018 the trend is back to books published in previous years. **So, we cannot say that there is a perennial trend here, but something that come as goes - and probably fluctuate with the books that are getting more attention**.

What about the this information concerning our top 15:

![train](/assets/posts/library/pubyear_title.png)

Here there is the answer we were looking for in the previous section: **the most holden books of all time had the most of their holdings in the years after the publishing.
 -->

## <a id="fiction"> </a>
## **We are all about the fiction here**

So, what could we discover looking to the preferred genres?

![train](/assets/posts/library/genre.png)

**Edmonton is all about the fiction**. Almost 7x the number of the second place! But here is where we start with the questions:

1. Is this a trend that probably will continue?
2. Is this true for every branch?

#### 1. Is this a trend that probably will continue?

![train](/assets/posts/library/genre_evolution.png)

Seems as an easy answer: **fiction is doing good, and it will keep going**.

> **Side signal**: there seems to be a good trend on the **variety of genres being featured during the years**. When in 2015 and 2016 we had few bars, in 2018 we seen a lot of different ones, so fiction is great, but the variety is coming, which implies different books being explored.

#### 2. Is this true for every branch?

There is one good way to explore this questions: trying to see the evolution - by branch - of the most chosen genre.

![train](/assets/posts/library/fiction-gif.gif)

Yes, it is a total fiction domination.

But what about other genres? Let's take off fiction and see what else is dominating the branches:

![train](/assets/posts/library/non-fiction-gif.gif)

Here we have more interesting patterns:

1. **2015**: We have drama domination due to _The girl on the train_ success, and only 2 branches picked different genres.
2. **2016**: The seed of change started growing and we can see Biographies and Juvenile Fiction taking in.
3. **2017**: Here we have total change with Juvenile Fiction taking almost all branches, with central areas sticking to Biographies.
4. **2018**: Self-help starts to pop in some different branches - is this the start of something new? (more below).

## <a id="evolution"> </a>
## **The self-help effect and genre evolution**

Here we are going to dedicate some space to explore some heatmaps on the evolution of adoption of certain genres (the most popular ones) during time.

The self-help effect - spreading strong:
![train](/assets/posts/library/self-help-gif.gif)

The drama is gone:
![train](/assets/posts/library/drama-gif.gif)

Fiction doesn't change:
![train](/assets/posts/library/fiction2-gif.gif)

Biographies too:
![train](/assets/posts/library/bio-gif.gif)

## <a id="conclusion"> </a>
## **(Sort of) Conclusion**

We did explore a lot of things here, so we could point some highlights for conclusion:

1. **Trends come from momentary high demand** : we could see that the top books have high number of holds but they change quickly during the years - _The girl on the train_ was the motive drama was so hot in 2015, but after that the genre just tanked.

2. **Fiction Domination** : fiction is dominating the genre dispute by far. And the trend is continuing - in all branches. And better: the move is spread and unpredictable.

3. **Genre diversification** : we could see a good trend developing in the last years that the number of genres being featured in the ranks are growing.

4. **Self-help effect** : when we take fiction off the genre analysis, we could see juvenile fiction and biographies dominating the branches during 2016 and 2017. But the same trend that made this is giving self-help a good momentum and it seems that the future is all about being a better human being - rather than reading fiction.


That's all by now!
Thank you.
**Fernando**
