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
-
- [The Data](#data)
- [Intro](#intro)
- [Understanding branch size](#bsize)
- [A new wave of holdings](#wave)
- [A tale of 3 stories](#genres)
- [Diversity is in the small .. at first](#small)
- [Conclusion](#conclusion)


## <a id="data"> </a>
## **The data**
This is an overall exploration of the [Edmonton Public Library Dataset](https://data.edmonton.ca/Community-Centres/Most-Popular-Books-by-Branch-Edmonton-Public-Libra/qdgm-hex6). A brief description of the dataset from the official provider:
>_"A snapshot of the top ten books with the highest number of holds at Edmonton Public Library, each week from 9 AM Monday to 9 AM Monday, organized by customer home library branch."_

For this specific exploration, I added the **genre**, **publishing date** and **google books rating** features (which were missing in the original dataset) using the [Google Books API for python](https://github.com/hoffmann/googlebooks). 

It is very important to state that: once we didn't have the actual ISBN serial number of the books in the dataset - neither it was possible to obtain this information in the EPL website - **the script used to bring this data (using the API) was based in a voting system for the most similar API matches - using the book title and the author - for each book**.  _If you would like to see how this was done technically, please check the Github repo_.

This implies that these additional features are not 100% accurate, but it was the best possible, considering the resources I had. I would add on top of that, using some samples and manual check, that genre is by far the most reliable feature, with publishing dates and ratings being less accurate - considering that each book can have different editions and other aspects.

#### A snapshot of the dataset

<table border="1" width="200" class="dataframe">
    <thead>
        <tr style="text-align: center; height:50%; font-size:13px;">
            <th></th>      
            <th>row_id</th>      
            <th>branch_id</th>      
            <th>branch_name</th>      
            <th>holds</th>      
            <th>title</th>      
            <th>author</th>      
            <th>date</th>      
            <th>url</th>      
            <th>google_rating</th>      
            <th>publishing_Date</th>      
            <th>genre</th>      
            <th>hold_year</th>    
            </tr>  
        </thead>  
        <tbody >    
            <tr style="text-align: center; height:50%; font-size:12px;">      
                <th>0</th>      
                <td >EPLLON20150..</td>      
                <td >EPLLON</td>      
                <td>Londonderry Branch</td>      
                <td>36</td>      
                <td >The girl on ..</td>      
                <td>Hawkins Paula</td>      
                <td>2015-03-16</td>     
                 <td >http://w..</td>      
                 <td>3.5</td>      
                 <td>2015.0</td>      
                 <td>Fiction</td>      
                 <td>2015</td>    
            </tr>  
        </tbody>
</table>

<p align="center"> <em> table 1: Dataset snapshot </em> </p>

The features:

- **row_id**: a unique identifier for each row (the join of title, data, author and branch id)
- **branch_id**: a unique identifier for each branch
- **branch_name**: the complete name of each branch
- **holds**: the number of holds for the title in that week (date)
- **title**: the title of the book
- **author**: the author of the book
- **date**: the first day of the week being featured
- **url**: url for the search in the EPL website using the book title as a parameter
- **google_rating**: the google books rating for that title
- **publishing_Date**: the year of publication of the book 
- **genre**: the genre of the book
- **hold_year**: the year component of the **date** feature


#### A brief description of the data available

<table border="1" class="dataframe" style="width:50px;">  
    <thead>    
        <tr style="text-align: right;">      
            <th></th>      
            <th>row_id</th>      
            <th>branch_id</th>      
            <th>branch_name</th>      
            <th>holds</th>      
            <th>title</th>      
            <th>author</th>      
            <th>url</th>      
            <th>google_rating</th>      
            <th>publishing_Date</th>     
            <th>genre</th>      
            <th>hold_year</th>    
        </tr>  
     </thead>  
     <tbody>    
        <tr style="text-align: center; height:50%; font-size:13px;">      
            <th>count</th>     
            <td>32300</td>      
            <td>32300</td>      
            <td>32300</td>      
            <td>32300.0</td>      
            <td>32300</td>      
            <td style="background-color:yellow;">32134</td>     
            <td>32300</td>      
            <td style="background-color:yellow;">23963.0</td>      
            <td>32300.0</td>      
            <td style="background-color:yellow;">32289</td>      
            <td>32300.0</td>    
            </tr>    
        <tr style="text-align: center; height:50%; font-size:13px;">      
            <th>unique</th>      
            <td>32300</td>      
            <td>22</td>      
            <td>22</td>      
            <td>0.0</td>      
            <td>3312</td>      
            <td>2375</td>      
            <td>3414</td>      
            <td>0.0</td>      
            <td>0.0</td>     
            <td>63</td>      
            <td>0.0</td>    
        </tr>  
    </tbody>
</table>

<p align="center"> <em> table 2: Data summary </em> </p>

Data uniqueness info:

- We have 22 different branches being analyzed;
- 3312 different titles and 2375 different authors were featured in the dataset;
- We had 63 different genres being represented in the dataset;

Data availability:

- We have few lines without **author** information;
- For the **google_rating** feature, in approximately 25% of the data, it was not possible to get this information;
- We had a few lines without the genre information (actually just 3 books);

One very important detail about data availability: 

<table border="1" class="dataframe">  
    <thead>    
        <tr style="text-align: center;">      
        <th></th>      
        <th>hold_year</th>      
        <th>total_data_points</th>     
        <th>number_of_months_available</th>    
        </tr>  
    </thead> 
    <tbody style="text-align: center;">   
        <tr>      
            <th>0</th>      
            <td>2015</td>      
            <td>8644</td>      
            <td>11</td>    
        </tr>    
        <tr>      
            <th>1</th>      
            <td>2016</td>      
            <td>8709</td>      
            <td>11</td>    
        </tr>    
        <tr>      
            <th>2</th>      
            <td>2017</td>      
            <td style="background-color: yellow;">6481</td>      
            <td style="background-color: yellow;">9</td>    
        </tr>    
        <tr>      
        <th>3</th>      
        <td>2018</td>     
            <td style="background-color: yellow;">8256</td>      
            <td style="background-color: yellow;">10</td>    
            </tr>    
            <tr>      
            <th>4</th>      
            <td>2019</td>      
            <td>210</td>      
            <td>1</td>    
            </tr>  
    </tbody>
    </table>

<p align="center"> <em> table 3: Data availability by year </em> </p>

Here we have a clear situation where **2015 and 2016 have more data available than 2017 and 2018** - 2019 is a very limited year concerning data entries, so for further analysis it will be just used in some specific cases. This implies that we should be very cautious when considering some year changes between analysis.

## <a id="intro"> </a>
## **Intro**

There could be numerous different ways to approach this dataset. My intention here with this analysis is to try finding meaningful patterns with a macro look to the questions. 

What are the important trends we can explore with confidence using the data we have - and knowing our limitations? 

What are some practical conclusions we could take from the final thoughts? 

This is my guideline for the investigation you are about to read.

# <a id="bsize"> </a>
## **Understanding branch size**

Before we jump into any type of question or conclusion, there is some elementary understanding we need to have:

**How are the branches different when concerning the distribution of the holds? is there a high concentration that would give any further analysis a bias?**

Answering these questions we can create a clear guideline to further understand the dynamics of the book holdings.

<table border="1" class="dataframe">  <thead>    <tr>      <th></th>      <th>total_sum</th>      <th>total_mean</th>      <th>total_median</th>      <th>size_index</th>      <th>%holds</th>    </tr>    <tr>      <th>hold_year</th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>    </tr>    <tr>      <th>branch_name</th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>    </tr>  </thead>  <tbody>    <tr>      <th>Whitemud Crossing Branch</th>      <td>141714.0</td>      <td>47238.000000</td>      <td>40650.000000</td>      <td>22</td>      <td>9.681155</td>    </tr>    <tr>      <th>Riverbend Branch</th>      <td>123651.0</td>      <td>41217.000000</td>      <td>35626.000000</td>      <td>21</td>      <td>8.447186</td>    </tr>    <tr>      <th>Lois Hole Library</th>      <td>105351.0</td>      <td>35117.000000</td>      <td>32087.000000</td>      <td>20</td>      <td>7.197026</td>    </tr>    <tr>      <th>Mill Woods Branch</th>      <td>105079.0</td>      <td>35026.333333</td>      <td>32192.000000</td>      <td>19</td>      <td>7.178444</td>    </tr>    <tr>      <th>Enterprise Square (Downtown)</th>      <td>96716.0</td>      <td>32238.666667</td>      <td>26129.000000</td>      <td>18</td>      <td>6.607128</td>    </tr>    <tr>      <th>Woodcroft Branch</th>      <td>84589.0</td>      <td>28196.333333</td>      <td>27392.000000</td>      <td>17</td>      <td>5.778675</td>    </tr>    <tr>      <th>Strathcona Branch</th>      <td>83448.0</td>      <td>27816.000000</td>      <td>24838.000000</td>      <td>16</td>      <td>5.700728</td>    </tr>    <tr>      <th>Idylwylde Branch</th>      <td>82838.0</td>      <td>27612.666667</td>      <td>25215.000000</td>      <td>15</td>      <td>5.659056</td>    </tr>    <tr>      <th>Castle Downs Branch</th>      <td>82262.0</td>      <td>27420.666667</td>      <td>25342.000000</td>      <td>14</td>      <td>5.619707</td>    </tr>    <tr>      <th>Jasper Place Branch</th>      <td>77425.0</td>      <td>25808.333333</td>      <td>22878.000000</td>      <td>13</td>      <td>5.289269</td>    </tr>    <tr>      <th>Meadows Branch</th>      <td>68167.0</td>      <td>22722.333333</td>      <td>22528.000000</td>      <td>12</td>      <td>4.656811</td>    </tr>    <tr>      <th>Londonderry Branch</th>      <td>59963.0</td>      <td>19987.666667</td>      <td>19813.000000</td>      <td>11</td>      <td>4.096357</td>    </tr>    <tr>      <th>Capilano Branch</th>      <td>55716.0</td>      <td>18572.000000</td>      <td>17419.000000</td>      <td>10</td>      <td>3.806224</td>    </tr>    <tr>      <th>Clareview Branch</th>      <td>51032.0</td>      <td>17010.666667</td>      <td>17010.666667</td>      <td>9</td>      <td>3.486238</td>    </tr>    <tr>      <th>Calder Branch</th>      <td>43110.0</td>      <td>14370.000000</td>      <td>13457.000000</td>      <td>8</td>      <td>2.945048</td>    </tr>    <tr>      <th>West Henday Promenad (Lewis Estates) Branch</th>      <td>42235.0</td>      <td>14078.333333</td>      <td>14078.333333</td>      <td>7</td>      <td>2.885273</td>    </tr>    <tr>      <th>Highlands Branch</th>      <td>39771.0</td>      <td>13257.000000</td>      <td>13257.000000</td>      <td>6</td>      <td>2.716945</td>    </tr>    <tr>      <th>Sprucewood Branch</th>      <td>33821.0</td>      <td>11273.666667</td>      <td>11273.666667</td>      <td>5</td>      <td>2.310473</td>    </tr>    <tr>      <th>Abbottsfield - Penny McKee Branch</th>      <td>32878.0</td>      <td>10959.333333</td>      <td>10959.333333</td>      <td>4</td>      <td>2.246052</td>    </tr>    <tr>      <th>McConachie Branch</th>      <td>30391.0</td>      <td>10130.333333</td>      <td>10130.333333</td>      <td>3</td>      <td>2.076153</td>    </tr>    <tr>      <th>Heritage Valley</th>      <td>21159.0</td>      <td>7053.000000</td>      <td>765.000000</td>      <td>2</td>      <td>1.445472</td>    </tr>    <tr>      <th>MacEwan University</th>      <td>2497.0</td>      <td>832.333333</td>      <td>784.000000</td>      <td>1</td>      <td>0.170582</td>    </tr>  </tbody></table>

<p align="center"> <em> table 4: holding numbers and size index by branch </em></p>

Here we have some information concerning the whole period of 2015 to 2019 for the holdings numbers of the branches - ordered by the total holdings. 

Some interesting things:

- Whitemud (9%) and Riverbend (8%) are isolated in the higher positions for the % of total holdings.
- ***I created the _size index_ for us to use in further analysis and to have the notion of the branch sizes. The bigger the index, the more holding power the branch has.**

When we look to the distribution of the holds by branch we have :

|![alou alou](/assets/posts/library/hold_hist.png)
|:--:|
|*Image 1: Histogram: distribution of the number of total holdings by branch.*

This is not a normal distribution for sure (p-value of 79% for the kurtosis test assuming a normal distribution).

Using *table 4* and *image 1* we can see clearly the concentration by some few branches in the top - although I would argue not a very serious one, but an important to keep an eye on - and a good part of the branches are situated in the middle still.

This is even more clear when we look to the means and medians for the whole period (2015 to 2018):

|![alou alou](/assets/posts/library/b_means.png)
|:--:|
|*Image 2: Yearly Means (right) and yearly medians (left) for holdings by branch (2015-2018) - ordered by size index*

We have the top 4 branches with the higher means and medians, but still a lot of branches in the middle. 

**For further analysis we should, every time we can, look to the size index and investigate the weigh of the top 4 branches in each effect we try to understand.**


## <a id="wave"> </a>
## **A new wave of book holdings**

So, our starting point here would be the following question:

**Is the book holdings for the libraries top 10's increasing/decreasing?** 

Let's plot the graph and see what happens: 

|![alou alou](/assets/posts/library/data_avail_holds.png)
|:--:|
|*Image 3: Months of data available by year (left), Total holds by year (right)*

Looking to *Image 3* we can see clearly that, even though we have fewer data in 2017 and 2018, these years have way more holdings than the previous ones. This is a **strong argument** in favor that the holdings indeed had a big spike, once that, logically, we would expect the following relation:

More data = More holdings -> **not our case here**.

The first question here would be: what about if this is a result of _only_ big branches rises in holds?

|![alou alou](/assets/posts/library/branch_avail.png)
|:--:|
|*Image 4: Data availability vs total holds by year, by branch*

Here we can have the overall picture: all branches are experiencing the same effect. This could make us more secure in thinking that, all the reasons and consequences of the great rise in holdings are probably happening in all branches.

|*Important consideration*: you may have noted that we had 3 new branches entering in 2017 - and that could be an important driver for these holdings ,right? But in effect, they are the smallest branches in all aspects even when we normalize it by means and medians (_see Image 2_). And for the whole period, these 3 branches corresponds to less than 3% of the data (_see table 4_). So I would discard this possibility.

#### **Where can we go from here?** 

We do not have the demographics and user data to investigate why exactly the holdings had such a rise in 2017, so we could check important questions like:

- Are the number of holdings per user growing? 
- Is the overall number of users bigger? 
- What is the main different driver by the user to show this difference?

But, once this effect happened, **what patterns can we learn looking inside of it**? **what is the path ahead**? 

These are the questions we are going to focus on in this analysis.



## <a id="genres"> </a>
## **A tale of 3 stories**

Considering all the years, what would be the most popular genres?

<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>total_holds</th>      <th>%total</th>    </tr>    <tr>      <th>genre</th>      <th></th>      <th></th>    </tr>  </thead>  <tbody>    <tr>      <th>Fiction</th>      <td>932804</td>      <td>63.750478</td>    </tr>    <tr>      <th>Biography &amp; Autobiography</th>      <td>100739</td>      <td>6.884790</td>    </tr>    <tr>      <th>Cooking</th>      <td>70164</td>      <td>4.795207</td>    </tr>    <tr>      <th>Self-Help</th>      <td>56262</td>      <td>3.845105</td>    </tr>    <tr>      <th>Psychology</th>      <td>34363</td>      <td>2.348465</td>    </tr>    <tr>      <th>House &amp; Home</th>      <td>27254</td>      <td>1.862616</td>    </tr>    <tr>      <th>Business</th>      <td>25324</td>      <td>1.730714</td>    </tr>    <tr>      <th>Health &amp; Fitness</th>      <td>25217</td>      <td>1.723401</td>    </tr>    <tr>      <th>Foreign Language Study</th>      <td>23838</td>      <td>1.629157</td>    </tr>    <tr>      <th>Political Science</th>      <td>19295</td>      <td>1.318675</td>    </tr>  </tbody></table>

<p align="center"> <em> table 5: Top 10 genres in book holdings </em></p>


Fiction it is! 63% of the total holdings and it is way ahead of the second place, Biographies. 

Ok, but would this top 10 be a result of a long term state or are some of these genres growing in recent years? 

|![alou alou](/assets/posts/library/genre_3stories.png)
|:--:|
|*Image 5: top 10 genres evolution in the number of holdings by year*

So here, on purpose, I  divided the top 10 into 3 distinct categories:

- **High Growth**: genres that have very expressive holdings evolution from 2015 and above.
- **Normal Growth**: genres that had growth, but nothing outstanding (comparing to the above).
- **Decaying** : genres with decaying number of holds from 2015 and above.

Concerning the **Normal Growth** category we could state that it has a high standard of consistent demand for all the years (not different the genres in this category are top 1 and 2). But for further investigation, we are gonna focus on the other 2 categories.

#### **High growth but different circumstances**

Excuse me, but before anything, I would like to zoom in the High Growth category and explore something more closely:

|![alou alou](/assets/posts/library/genre_hgrowth.png)
|:--:|
|*Image 6: high growth evolution in the number of holdings by year*

If you remember the content discussed in _Image 3_ you will also remember that we had a very intense spike in the number of holdings from 2016 to 2017. This had an obvious impact on all the genres we are studying in this section. 

But take a closer look in the 2 red circles on _image 6_:

On the left, on what I called _constant high growth_, we can see that these genres were **accelerating even before the spike in holdings**. On the other hand, this is not clear in the right part of the graph: **these genres were static or in some cases even decaying before the bump**. It seems we could have something going on here.

My main mission now is identifying real successes and real rising genres for this group. That said, I would like to see in the desired genre:

- 1) Wide adoption
- 2) (_optional but good to have_) Variety of titles

**1) Adoption check**

To do this analysis, we will be using the following type of graph (heatmap):

- **On the Y(vertical) axis:** the branches ordered by the higher branch (top) in size, using size index, to the smaller one (bottom).
- **On the X(horizontal) axis:** the years of analysis.
- **Metric inside the squares**: % of adoption of that genre in the corresponding year.

|![alou alou](/assets/posts/library/one_heat.png)
|:--:|
|*Image 7: Fiction adoption by branch - Heatmap*

_Image 7_ is just a training for us to look for the genres in high growth. I used the Fiction genre just to show the interpretation:

- Darker colors represent more adoption
- The superior part of the graph represents the bigger branches

For Fiction, we can notice the crescent predominance of lighter colors with the passing of the years, which would imply that the adoption rate of fiction is decaying - with this effect being more effective on the bottom part, which states that fiction is less adopted in smaller branches and this is intensifying during the years.

Now we are trained to look to the High Growth genres and answer: is there wide adoption for all these genres?

|![alou alou](/assets/posts/library/high_heat1.png)
|:--:|
|*Image 8: Constant High Growth genres adoption by branch - Heatmap*

|![alou alou](/assets/posts/library/high_heat2.png)
|:--:|
|*Image 9: Eventual High Growth genres adoption by branch - Heatmap*

When comparing _image 8_ and _image 9_, it is clear we are seeing two different patterns here:

- **Constant High Growth** genres (image 8) shows us a more widespread adoption, mainly if you look to self-help and cooking, you can see the evolution of adoption happening in all branches. With psychology too, but we would say that is more adopted in the very top and bottom - not as much in the middle.

- **Eventual High Growth** genres (image 9) confirm what would be our more logical guess: they 'surfed' in the spike and are way more concentred events. If we take a look to health & fitness and political science, they are bottom and top concentrations respectively.

For the adoption it would be reasonable to assume that the _constant high growth_ group is a spread phenomenon, when the _eventual_ group is not.

**2) Variety check**

Before the verdict, let's just take a look at the possibility of an individual title (or a small group of them), is influencing this results by genre:

|![alou alou](/assets/posts/library/scatt_1.png)
|:--:|
|*Image 10: Constant Growth % of adoption by title by year, by genre*

|![alou alou](/assets/posts/library/scatt_2.png)
|:--:|
|*Image 11: Eventual Growth % of adoption by title by year, by genre*

The annotated titles with their correspondent % of adoption represent the title with more adoption by year.

That said, we can see that the picture for both *constant* and *eventual* growth genres have a clear pattern:

**Concentration for the more adopted titles for each genre is decaying with the pass of time**. This means that besides being adopted in many branches, these genres are experiencing a more variety of titles being demanded, which could imply in a more solid and long term trend for these genres getting even more traction.

> Considering these 2 points explored above, I would say that the **Constant Growth Genres are the safest picks for the real growth genres**. The eventual - as the name suggests - show genres that are surfing the higher number of holds but do not show any characteristic to continue performing in the long term.

#### **Constant High Growth Domination**

Let's take a look into the dissemination of the Constant High Growth genres geographically:

|Cooking linear adoption (%) by years
|:--:|
|![](/assets/posts/library/cooking_gif.gif)

|Self-Help linear adoption (%) by years
|:--:|
|![](/assets/posts/library/self_gif.gif)

|Psychology linear adoption (%) by years
|:--:|
|![](/assets/posts/library/psy_gif.gif)


#### **Marie Kondo and English hustlers**

|![alou alou](/assets/posts/library/low_heat.png)
|:--:|
|*Image 12: Decaying High Growth genres adoption by branch - Heatmap*

So we could see that House & Home was a past widespread event in all branches, but Foreign Language study was very Millwoods and Meadows effect. 

What about the titles?

|![alou alou](/assets/posts/library/scatt_3.png)
|:--:|
|*Image 13: House & Home% of adoption by title by year, by genre*

Here we have Marie Kondo effect: this genre probably is featured in the top 10 ranks because of Marie Kondos's book and is leaving it because of the same reason.

For Foreign Language Study, Cambridge IELTS just dominated all the years. 

## <a id="small"> </a>
## **Diversity is in the small .. at first**

Once we have already explored a lot of diversity concerning genres, here is the final graph that would be enlightening for this purpose:

|![alou alou](/assets/posts/library/diver_heat.png)
|:--:|
|*Image 14: Genre diversity on the Top 10's by branch heatmap*

Here we can see a spread and crescent diversification happening in all the top 10's in the branches, with a more strong effect happening **first on the smallest branches and coming to bigger ones (in the top)**.

So, diversification - with more holding numbers - is the name of the scene here.

## <a id="fail"> </a>
## **Failed attempts**

Here I am going to reveal some failed attempts to explore some aspects of the data.

#### Google Books Rating Score

One of the data I brought using the Google Books API was the Google Books Rating ( metric: from 0 to 5, the higher the better). 

Even though this implies in an interesting feature to help to explore this dataset, here we had 2 problems: 

- 25% of all the data didn't have a valid google rating (for the specific titles).
- Is was not very conclusive when applied to the data.

|![alou alou](/assets/posts/library/google.png)
|:--:|
|*Image 14: google Books rating vs total number of holds, by genre type*

If we do not consider the outliers, it would make some sense until rating 4 - the number of holds is growing with the rating. But after that, it just drops and goes the other way around. 

Another point here is that the majority of holding successes are located in the 3.0 to 4.0 - which is good - but why not in the 5.0's? This only showed to me that there was another important feature working around here than the rating.

#### Publishing Year

Another point that I tried to investigate was the publishing year, mainly with this question: Are newer books helping the book holdings grow? Are there preferences for older books or brand new published books?

Here is the graph:

|![alou alou](/assets/posts/library/pubs.png)
|:--:|
|*Image 14: total number of holds divided by books published in that same year or prior*

Although we can identify some yeas where new books were preferred, and others where it was not, the difference is very sutil.

When considering all the years is almost impossible to find a year where the most preferred scenario - older books or newer ones - had more than 55% of the preference. Which is the majority but not very satisfying for a clear choice right? 

I have to mention too that this graph - image 14 - helped me to see that the holding number spike was not caused by brand new books too.

## <a id="conclusion"> </a>
## **Conclusion**

We did explore a lot of things here, so we could point some highlights for the conclusion:

1. **The number of holdings exploded**: the data is clear that the number of holdings had a great spike - even though with this data we are not capable of exploring exactly why.

2. **Diversification is the name of the game**: we could see in numerous ways that the diversification movement is happening all along the branches.

3. **In the small is the seed**: we could also see that it is in the smallest branches that diversity is happening faster.

4. **Is all about the mind, and then food**: exploring the most prominent genres, we could identify that self-help, psychology, and cooking are the safest candidates to pick on fiction - maybe someday - being widely adopted and with consistent growth and diversification during the years.


That's all for now!
Thank you.<br>
**Fernando**
