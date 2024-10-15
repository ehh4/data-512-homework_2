# Data 512 Homework 2

### Project goal
The goal of this project is to look at bias in data using Wilipedia articles. This project looks at polticians from countries across the world and their pages quality from ORES score on Wikipedia and analyzes coverage of politicians on Wikipedia and coverage quality. The specified politicians, a URL to their Wikipedia page, and their country are given by the politicians_by_country_AUG.2024.csv. The project collects last revision id data using the MediaWiki REST API for the EN Wikipedia, which is documentated [here](https://www.mediawiki.org/wiki/API:Info). Using the last revision id returned by the pageinfo API, the predicted quality score for the articles is collected using the ORES machine learning system. The predicted ORES score is collected by using the ORES LiftWing API, the document can be found [here](https://ores.wikimedia.org/docs) and [here](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing/Usage). NOTE: To use the LiftWing API service, you need a access token, intructions for this can be found [here](https://api.wikimedia.org/wiki/Authentication) and in the wp_ores_liftwing_example.ipynb notebook. The article quality estimates from ORES score are, from best to worst:
- FA - Featured article
- GA - Good article (also known as A-Class)
- B - B-Class article
- C - C-Class article
- Start - Start-class article
- Stub - Stub-class article

For this project, a "high quality article" is one that has a score of FA or GA.
After the data is collected for each given politician article, the data is joined with region and population data from the population_by_country_AUG.2024.csv file. Analysis is done and 6 tables are created showing the following:
- The 10 countries with the highest total articles per capita (in descending order)
- The 10 countries with the lowest total articles per capita (in ascending order)
- The 10 countries with the highest high quality articles per capita (in descending order)
- The 10 countries with the lowest high quality articles per capita (in ascending order)
- A rank ordered list of geographic regions (in descending order) by total articles per capita
- Rank ordered list of geographic regions (in descending order) by high quality articles per capita


### Licensing
The MediaWiki Action Pageinfo API source code and the Wikimedia LiftWing API source code is licensed by [Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use).
The starter code is adpated from Dr. McDonald's example notebooks, wp_page_info_example.ipynb and wp_ores_liftwing_example.ipynb which are licensed [CC-BY](https://creativecommons.org/licenses/by/4.0/). The following changes were made:
- Added my own email
- Added my own Wikimedia username and access token

All other code not designated as starter code from Dr. McDonald is created by Elizabeth Holden and licensed [CC-BY](https://creativecommons.org/licenses/by/4.0/).

### Files
HCD-HW2.ipynb is the notebook containing all project code and produces the following files:
- no_score.csv: This is a file containing a columm 'name' (type str) that has the name of articles that I was not able to get a ORES score for. Some did not get a score because I was not able to get a last revision id for them and some I was not able to get an ORES score for them.
- wp_politicians_by_country.csv: This is a file containing the final merged dataset that was used for the analysis. The file contains the following columns:
    - country (type str)
    - region (type str)
    - population (type float64)
    - article_title (type str)
    - revision_id (type int64)
    - article_quality (type str)
- wp_countries-no_match.txt: This is a file containing a list of countries, each on a seperate line, that correspond to entries that could not be merged, either the population dataset does not have an entry for the equivalent Wikipedia country, or vice-versa



### Research Implications
Reflection:

This assignment was an interesting way to look at bias because, while there is bias here in the data that we found, I think that there is bias in the way that we are looking at the data also. When looking at the countries with the highest articles per capita and the countries with the least amount of articles per capita, the countries with the highest articles are the countries with some of the smallest populations and the lowest articles per capita are the countries with some of the highest populations. I am not sure if we can say yes or no there is bias based on this because obviously larger population countries are going to have less articles per capita when the population is so large. Another thought I had during this was that some countries are a lot older than other countries and each country has very different govenment structures that have different numbers of people in politics, so all these factors combined can give very different results for countries. Additionally, There were a lot of countries with 0 high quality articles, and after looking at this list, many of these countries are third would countries, so I do think that the high quality articles are more biased towards more western and developed countries.


What biases did you expect to find in the data (before you started working with it), and why?

I expected for countries that are less developed, like third world countries, to have less articles available. Not only did I expect for there to be less articles overall, but also less high quality articles due to less information being readily available. Not all third world countries have large broadcasts of information available which is why I thought this. Additionally, if a country is not well developed and does not have a well developed govenment system then there may be less politicians in general and therefore less articles about these politicans available on Wikipedia. One last bias I thought there would be is a bias towards English speaking countries since we were looking at English Wikioedia.

What (potential) sources of bias did you discover in the course of your data processing and analysis?

I found that from the data that we were given, the data is bias towards underdeveloped countries. The countries that had 0 high quality articles were a majority underdeveloped countries, a lot from Africa.Additonally, not all countries have a representative amount of politicians represented in the dataset. Many large countries have a large government system because they have a large population, yet this is not represented in the politician dataset. We can see this in China, it is one of the largest population countries in the world with a very large number of politicians yet there are only 16 politician articles in the given dataset. This small number of articles divided by the large population makes China the country with the lowest articles per capita.

Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?

If someone was trying to make a business decision around what country would be best to start a political magazine in, using this data would result in very misleading results. The results would be misleading because if the business decision makers were looking at articles per capita to base their decision on, they would see really small countries with the highest articles per capita which is misleading since these countries most likely have a smaller number of government representatives and therefore a smaller number of polticians to contribute to their magazine.


### Considerations
- The no_score.csv file contains the names of articles that did not have ORES scores, some of these did not have ORES scores due to not having a last revision id returned from the pageinfo request so I was not able to call the ORES API for them and some did not have an ORES score due to the ORES API request not returning one. The breakdown of this can be seen in the lists in the notebook used to create the no_score.csv file
- There are politicians that are listed multiple times for different countries in the politicians_by_country_AUG.2024.csv, I am chosing to leave these in because after researching some, I found that they may have been a politician in a country that was made up of multiple of the current countries at the time they were serving their term. A list of these are printed and noted in the HCD-HW2.ipynb file.
- All population values are in millions and articles per capita values are articles per million people.
- There are 66 countries with 0 high quality articles, therefore these all are the countries with the lowest high quality articles per capita, so instead of a table of 10, I printed all 66 of these countries since they all have 0 as a value for high quality articles per capita
- When calculating the articles per capita, there are countries with 0.0 population due to the population being in millions, I chose to fill these with NaN for the sake of calculations.
- At first GuineaBissue and South Korea were in the no match list due to being formatted differently in the 2 datasets, so I renamed the countries so that these could match.

