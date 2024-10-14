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



### Considerations:
- The no_score.csv file contains the names of articles that did not have ORES scores, some of these did not have ORES scores due to not having a last revision id returned from the pageinfo request so I was not able to call the ORES API for them and some did not have an ORES score due to the ORES API request not returning one. The breakdown of this can be seen in the lists in the notebook used to create the no_score.csv file
- There are politicians that are listed multiple times for different countries in the politicians_by_country_AUG.2024.csv, I am chosing to leave these in because after researching some, I found that they may have been a politician in a country that was made up of multiple of the current countries at the time they were serving thier term. A list of these are printed and noted in the HCD-HW2.ipynb file.
- All population values are in millions and articles per capita values are articles per million people.
- There are 66 countries with 0 high quality articles, therefore these all are the countries with the lowest high quality articles per capita, so instead of a table of 10, I printed all 66 of these countries since they all have 0 as a value for high quality articles per capita

