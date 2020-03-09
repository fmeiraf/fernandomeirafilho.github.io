---
layout: post
title:  "[Data Analysis] A multidimensional approach to understanding cities growth - Sao Paulo Province"
date:   2020-03-07 10:00:00 -0700
categories: jekyll update
---

**Github repository for the project:** [click here](https://github.com/FernandoMeiraFilho/Capstone-Final).

**Jupyter Notebook fast link:** [click here](https://github.com/FernandoMeiraFilho/Capstone-Final/blob/master/Final%20Code%20v2.ipynb).


- [Intro](#intro)
- [The Data](#data)
- [Results](#results)
- [Conclusion](#conclusion)
- [References](#refs)


## <a id="intro"> </a>
## **Intro**

In this section, we are going to cover the foundations of this study. First, we are going to frame the actual debate on how to measure growth and development in countries, which also encompasses the measurement of related cities within the same country. Second, we are going to introduce the main method used for this project, called I-Distance, explaining the method workflow and its particularities. Finally, we are going to discuss the relevance of this study in the context of Brazil and thereby identifying the specific client for this project, for whom recommendations are suggested at the end of this document.

#### **The challenges associated with measuring growth**

What methodologies to use to measure growth is a topic that is amply debated in Economics and Social Sciences. Most of the approaches used in this type of study, for many years, have been focused on just economic aspects. The most used parameter for these has been the GDP (Gross Domestic Product), which is a monetary account for all the wealth generated in a country. Although this measure can be used to rank countries by their wealth, it has been very poor to capture socio-economic gaps within the subjects of study.

The first attempt for a ‘multidimensional’ approach started with the creation of the HDI (Human Development Index), by The World Bank in 1990. This index consists of grouping 3 major indicators: education, income, and longevity. Given its simplicity, this index was very popular during a long period but, according to many researchers, it has been criticized because of the large correlation among the indicators and its small number. This oversimplification raised the concern that this index would not capture nuances of the country's development when looking to different countries, and as a consequence, different scenarios.

As a response to the concerns raised above, some other versions of the index have been created to try to fix these issues and to try to get more accurate modeling of growth. The most relevant include the following: CDI (Calibrated Human Development Index) is very similar to HDI but it gives more weight to life expectancy than to education. Another similar is the resource–infrastructure-environment (RIE) index, which attempts to capture structural concerns that are relevant to the characterization of development, including ICT infrastructure variables. Other variables could be added such as internet access and similar indicators associated with technology use.

This is an indication that the world nowadays is getting multi-dimensional fast and the addition of different features to growth models is going to get more usual than ever. Because of that, approaches that could deal with this multidimensionality more naturally should be of enormous importance within this study field.

#### **The I-Distance Method**

Considering all the discussion above, the focus of this study was the choice of a multidimensional method that could capture the interaction of different variables on growth, allowing us to check the relative relevance of each of them. The I-Distance method used in this present study was introduced by Milenkovic and others in [[1]]( http://www.sciencedirect.com/science/journal/02649993/38).

The main idea of this method is to use hyperplanes, composed by a given group of chosen variables, to then compare growth between countries (and in our case, cities) by using the distance between different planes. Entities of interest would be represented by the values of chosen variables, through the hyperplane representation, and they would be compared to a unique hyperplane referred to by the authors as the “reference entity”. This reference entity could represent different types of benchmarks that would serve as a baseline for the comparison among the entities. The reference entity could be, for example, the hyperplane computed with the values that can be summarized by using minimum, mean or maximum values.

Given the selected features X<sup>T</sup> = (X<sub>1</sub>, X<sub>2</sub>, ... , X<sub>k</sub>) the I-distance between e<sub>r</sub><sup>T</sup> = (X<sub>1r</sub>, X<sub>2r</sub>, ... , X<sub>kr</sub> and e<sub>s</sub><sup>T</sup> = (X<sub>1s</sub>, X<sub>2s</sub>, ... , X<sub>ks</sub>), defined as follows:

![alou alou](/assets/posts/city_growth/Idistancenorm.png)

Where:

- d<sub>i</sub>(r, s) is the distance between the values of variable X<sub>i</sub> for e<sub>r</sub> and e<sub>s</sub> e.g. the discriminate effect, d(r,s) = x<sub>ir</sub> - x<sub>is</sub>, 𝑖 𝜖 {1,...,𝑘}.
- σ<sub>i</sub> the standard deviation of X<sub>i</sub>
- r<sub>ji.12 ... j − 1</sub> is a partial coefficient of the correlation between X<sub>i</sub> and X<sub>j</sub>, (j < i).

The I-Distance between the observations and the reference would be given by the discriminate effect divided by the standard deviation of that variable, multiplied by 1 minus the correlation between those variables (which would imply in the “pure” effect of that variable).
In some sets, negative correlation effects and negative partial correlation effects could occur, mainly in scenarios with a reduced number of variables, it would be interesting the use of the I- Distance squared, which is given by:

![alou alou](/assets/posts/city_growth/Idistance.png)

There is a second important part of the I-Distance method, which is one of the main points for this project: the possibility of measure the relevance of each variable for the index calculation.

After the calculation of the I-distance, the correlation effect of each variable with the I-distance is calculated. This correlation should be interpreted as the relevance of that variable for the distance index: the bigger the correlation, the most representative that feature is.

Then, after the rank is computed, we choose the smallest correlation value that is not significant. This variable will be excluded from the model, and the calculation (the distance index and the following correlation between features and distance) will be computed again. This process should be repeated until just significant variables remain. By the end, a rank of the most relevant features should remain, indicating only the variables that are relevant for measuring growth for that set.

#### **K-Means clustering**

The K-Means is, in summary, a method to classify data into K different clusters, where the value of K is defined a priori. K initial centroids are set far as possible from each other and the data points are distributed to each nearest centroid.

After this first arrangement, each cluster has its centroids recalibrated to assume the very barycenter of that centroid, and these K centers are relocated and the data points distributed again. This process is done until no more moves by the centroids are done, and the clustering is given by the last iteration.

When considering the K-Means method in the context of this project, the main approach is to use the clustering method, also with the I-Distance ranking values, to look for patterns in growth behaving within the province of São Paulo. This should assume the form of different groups of cities that should share some similarities when considering growth as a whole and would allow as to make some comparisons, not just in individual level of characteristics, but at a group level too.


#### **Measuring growth of cities**

All of the methods discussed above were conceptualized using countries as entities to be compared. The concern in considering all of the important aspects related to the development of nations so they can be compared by using a common metric.

This elementary question is possible because, as we know, each country has it’s own particularities, and it`s own data that describe its situation. This is very similar when we move to the issue of discussing the growth of cities within the same country. It is very common, mainly in medium-size to big countries (like the case of Brazil), to select two cities that belong to the same province but have very different situations concerning culture, economic growth, healthcare, etc.

So, in our case, once we have all the data that describe these particularities, it is reasonable to extend these methods used for countries' growth, to model growth of cities within the same country.

#### **The relevance of the study and the client**

Currently, Brazil seems to be drowning in corruption scandals at various levels. One of the main concerns is the management of public resources and the effect of what is promised and what is built on these promises.

One of the main questions that this study aims to answer is the relevance of investment variables (public and private) in the growth of the cities of the São Paulo province, which might also offer some insights into aspects related to corruption.

The main client for this study is the Office of the Public Governor of the province which could, using the results from this project, measure the relevance of the investment as a policy for growth and balance the distribution of this incentive. We could say that the population of the cities and the province could be an additional stakeholder, who could use the results of this project as an argument for requesting a fairer distribution of resources.


## <a id="data"> </a>
## **The data**

#### **Data Presentation**

The data set used for this study was merged from two sources: Investment (public and private) values by year:

[http://dados.gov.br/dataset](http://dados.gov.br/dataset)

The rest of the features (demographics, social, economic, etc.):

[http://www.imp.seade.gov.br/](http://www.imp.seade.gov.br/)

The data ranged from the years 2000 to 2016 for all the 645 cities of the province of São Paulo, Brazil. The features of choice were based on the main pillars of growth that are: education, economics, demographics, and social. The complete list of features follows:

|![alou alou](/assets/posts/city_growth/datatable.png)
|:--:|
|*Image 1: features description.*

#### **Data Exploration**

The main question when exploring the data was to set up the best scenario to apply the I- Distance method. This scenario would be the year with the most data available, so the distance could be more accurate concerning all the features.
The focus for this analysis was on the three main variables for the study: GDP per capita, public and private investment.
When looking to GDP per capita missing values we could see:

|![alou alou](/assets/posts/city_growth/gdp-table.png)
|:--:|
|*Image 2: Number of entries for GDP per capita variable, per year.*

Here we could conclude that 2000, 2001 and 2015 were years that should not be used to this analysis, once there were no entries for anyone of them.
Going a little deeper and trying to see the year with the best combination for the three variables mentioned above, we could see a clear choice as the plot below shows:

|![alou alou](/assets/posts/city_growth/city-opport.png)
|:--:|
|*Image 3: Number of entries for the 3 main features, by year.*

Looking at this plot the scenario is set. For GDP per capita, we can choose from 2001 to 2014, as we saw above. But for investment, our best chance is located in 2013, once that private investment is at its top level, and public investment reaches its peek too.
Since the year of 2013 is a clear choice, our final check would be in the availability of the values for the other features in the data set.

|![alou alou](/assets/posts/city_growth/2013features.png)
|:--:|
|*Image 4: Data entries for all the features in the year of 2013.*

When looking to Image 4 we could see that 3 features need to be dropped: HDI, educ_superior and tax_revenue, once that any one of them has values. For the other variables, we can see that there are plenty of values to maintain them, even the private investment that, besides of the fact that it has a small number of values, it is a crucial variable to the project,

After this cleaning and dropping the 3 features above, the data set was ready for further analysis.


# <a id="results"> </a>
## **Results**

#### **Defining the Reference Entity**

As defined above, the reference entity is the foundation for the calculation of the I-Distance index. Among the many possibilities for this element, in this project, the focus was entirely on the indicator GDP per capita. This was the case because the GDP per capita, besides the fact that it is a very used parameter in other growth studies, resumes a decent part of wealth distribution because of its ‘normalization’ by the number of inhabitants.

Another important fact was that, for this project, the reference entity was chosen to be a real city, which could assume a more realistic point of comparison than taking the mean or median of all indicators. Following this thought, the city of Itapirapuã Paulista was chosen because it has the smallest number for GDP per capita among all the cities in the studied period. This implies that cities with great values of I-Distance will reflect, by the definition of this metric, cities far from the reference and as a consequence, cities with more growth.

As a result, the reference entity vector would assume this form:

|![alou alou](/assets/posts/city_growth/gdpreference.png)
|:--:|
|*Image 5: Values of the reference for each indicator.*

#### **Choosing between two versions of the I-Distance method**

As defined above, the I-Distance method was the main tool for this analysis. Recall there are two ways of using the I-Distance method: the squared version, and the not squared one.

The not squared version is more sensitive, to big differences in variable values and a small number of data (or features), which could affect the final results. To check for the difference in the results, the two versions were applied and the ‘sensitivity’ of the not squared method is clear as seen in the table below:

|![alou alou](/assets/posts/city_growth/itable.png)
|:--:|
|*Image 6: Number of iterations and excluded features for the I-Distance calculation.*

As we can see, the not squared version eliminated a big part of the features that, in the squared version, remained. This is taken as a negative point for this model, once that the main interest of the study is to check for the features that contribute more to growth and how they contribute to it. In addition to this, for the rank of the features of the not squared version, we can see:

|![alou alou](/assets/posts/city_growth/frelevance.png)
|:--:|
|*Image 7: Rank of features relevance for the not squared I-Distance.*

Since the ‘r’ column reflects the correlation of that feature with the I-Distance calculation (as defined in the introduction section), hence the contribution of that variable, for “density_pop” and “urbing” features we have a negative ‘r’, which makes the interpretation difficult, once that these variables are still significant. It is important to say here that, the p-value in this context represents the probability of finding the value on the table above if in fact, the value of ‘r’ were 0.

Then we can conclude here that we should focus more on the squared version.

#### **The results of the squared I-Distance method**

As mentioned before, for this version of the method, 7 iterations were needed and the following variables were excluded as non-significant, in this order: olding, agriculture_add, gdp_growth, hosp_rooms_per, primary_enrolls, private_inv, and urbing.

Taking the first look at the features, we can see the rank of relevance as follows:

|![alou alou](/assets/posts/city_growth/frelevanceS.png)
|:--:|
|*Image 8: Rank of features relevance for the squared I-Distance.*

One of the most interesting points that we are seeking in this project is to check the relevance of investment variables in the calculations. As we can see, the private investment was dropped in the sixth iteration. This implies that, for this scenario, the private investment was not important for the growth of the cities. On the other hand, the public investment figured as a relevant feature, located in the middle of the rank. Another interesting result concerning the features is that, although we have a lot of economic features at the top of the rank (jobs, service_add, value_add) the top feature was the violence indicator. This is a very interesting finding, given that violence is one of the main weak points of public management in many cities of all the country.

For the rank of cities we have:

|![alou alou](/assets/posts/city_growth/rcity.png)
|:--:|
|*Image 9: Rank of cities for the squared I-Distance (best-positioned cities).*

For the cities, we can see that a very good reality check for the index is that the city of São Paulo is on the top of the rank. São Paulo is a very big city, it has a lot of global companies installed, it is a crucial point for the financial industry of the country, and it is an outlier in the province. Other cities that are natural candidates to be placed in the best positions of the rank are there, like Campinas, São José dos Campos, Santos, Guarulhos, and Osasco. These cities are relatively big, well-positioned in the logistics scheme for industries and core technology development centers within the province and even within the country. Other cities of the province that are not that relevant when looking to common parameters like São Sebastião, Ilha Comprida, and Louveira, are big surprises here and should be a very interesting start point when looking through the implications of their position and the parameters they have.

#### **Merging the I-Distance method with clustering**

When looking for growth contributors and ranking cities, one of the most common questions would be if there are patterns or related groups of cities that would make sense geographically, economically or even demographically.

To tackle this point, the purpose here was to use clustering methods (K-means) to combine the index explored above and look for patterns within the province.

The number of centroids (K) was chosen using the Elbow method. The number chosen was 5, and this choice is clear when looking at the following plot:

|![alou alou](/assets/posts/city_growth/elbow.png)
|:--:|
|*Image 10: Variance explained versus the number of centroids chosen.*

With the optimal number of centroids chosen, the clusters are shown in the following figure:

|![alou alou](/assets/posts/city_growth/clusters.png)
|:--:|
|*Image 11: The distribution of the clusters using all the features.*

For the visualization of Figure 8 to be possible in a two-dimension plot, a Principal Component Analysis (PCA) was performed. In a brief explanation, PCA is an algorithm to reduce the dimensionality of a data set. The main idea is to reduce dimension using variables that are correlated (heavily or lightly), transforming the correlated variables in a new set of variables (called principal components) and preserving the variation of the data as maximum as possible. For Figure 8, a PCA, consisting of 2 principal components, was computed, reducing the dimension of the dataset to 2 and making possible the two-dimension visualization of the data.

Looking at the figure above it's clear why the number of 5 clusters is the optimal solution. There are 5 different groups and the calculations for most representative cities for the centroids shows:

|![alou alou](/assets/posts/city_growth/cluster-table.png)
|:--:|
|*Image 12: Cluster most representative cities.*

The ‘one-city’ clusters are represented by São Paulo (for cluster 1) and São Sebastião (cluster 3). This seems to be natural for the case of the city of São Paulo, that is a clear outlier. When looking to the case of the cluster represented by the city of São Sebastião, this is not so clear, considering that it is a medium city, located on the coast of the province and far from the port infrastructure that is in the south part of the coast. However, these two cities are ranked as the best two cities when considering the index, even with a big edge separating them. This implies that, despite the territory size or infrastructure, the complete set of features should be considered, and even more, this single fact helps to corroborate the assumption that growth must take into account multiple metrics.

|![alou alou](/assets/posts/city_growth/cluster0.png)
|:--:|
|*Image 13: Rank of cities for the squared I-Distance (best-positioned cities) for cluster 0 with cluster centroid in highlight.*

|![alou alou](/assets/posts/city_growth/cluster1.png)
|:--:|
|*Image 14: Rank of cities for the squared I-Distance (best-positioned cities) for cluster 1 with cluster centroid in highlight.*

|![alou alou](/assets/posts/city_growth/cluster2.png)
|:--:|
|*Image 15: Rank of cities for the squared I-Distance (best-positioned cities) for cluster 2 with cluster centroid in highlight.*

|![alou alou](/assets/posts/city_growth/cluster3.png)
|:--:|
|*Image 16: Rank of cities for the squared I-Distance (best-positioned cities) for cluster 3 with cluster centroid in highlight.*

|![alou alou](/assets/posts/city_growth/cluster4.png)
|:--:|
|*Image 17: Rank of cities for the squared I-Distance (best-positioned cities) for cluster 4 with cluster centroid in highlight.*

On the other hand, when looking to the question on whether investment (in this case the public, that was the only that remained in the model) is relevant to the growth of this cities, we can have very interesting insights grouping the cities with the cluster labels and comparing these as different groups.

|![alou alou](/assets/posts/city_growth/scattSum.png)
|:--:|
|*Image 18: Scatter plot of the sum of I-Distance and public investment by the cluster.*

Even though cluster 0 groups the largest number of cities, there is a clear visual relationship where the clusters with a more absolute value of investment have a better index. However, we should admit that, when using the mean or median (as shown in Figure 10), to normalize this distribution effect, the relationship is way subtler.

|![alou alou](/assets/posts/city_growth/scattMean.png)
|:--:|
|*Image 19: Scatter plot of the mean of I-Distance and public investment by the cluster.*



## <a id="conclusion"> </a>
## **Conclusion**

This project explored the question of the extent to which investment is relevant to city growth, and as a consequence, how growth differs from city to city (or group of cities).

Using the I-Distance as a unifying metric, it seems that private investment was not relevant in any of the scenarios we explored. However, public investment is shown to be relatively important. Moreover, when looking at the clusterings, the public investment seems to be one good approach to differentiate city growth, when considering different types of cities within the province (according to the features used in this project).

The models we built using the concept of I-Distance and its combination with clustering were relatively good, positioning cities with theoretical growth in expected places and also showing cities that are not on the radar of many public agents but should be studied at a deeper level.

#### **Recommendations**

As part of the recommendations for the client, we would like to highlight the following:

- Better management of public investment funding, since it seems to have relative relevance for cities' growth.
- Look for benchmarks outside the most known cities, considering different features in the analysis.
- Take a closer look at violence indicators, since it was classified as the most important features on the subset studied.

## <a id="refs"> </a>
## **References**

 [1] N. Milenkovic, et alia: "A multivariate approach in measuring socio-economic development of
 MENA countries". In Economic Modeling 38 (2014) 604-608. Available from
 [http://www.sciencedirect.com/science/journal/02649993/38](http://www.sciencedirect.com/science/journal/02649993/38)

 [2] J. Bang, et alia: "New Tools for Predicting Economic Growth Using Machine Learning: A Guide for
 Theory and Policy".. Available from:

 [https://www.researchgate.net/publication/291827961_New_Tools_for_Predicting_Economic_Growth_Using_Machine_Learning_A_Guide_for_Theory_and_Policy](https://www.researchgate.net/publication/291827961_New_Tools_for_Predicting_Economic_Growth_Using_Machine_Learning_A_Guide_for_Theory_and_Policy)

 [3] N. Adriansson, et alia: "Forecasting GDP Growth or How Can Random Forests to Improve
 Predictions in Economics? ". Available from:
 [https://pdfs.semanticscholar.org/d402/473ba628b67bcd0d3a8cf39799ae6efbdc66.pdf](https://pdfs.semanticscholar.org/d402/473ba628b67bcd0d3a8cf39799ae6efbdc66.pdf)
 

That's all for now!
Thank you.<br>
**Fernando**

