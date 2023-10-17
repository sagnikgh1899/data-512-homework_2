[![GitHub license](https://img.shields.io/github/license/sagnikgh1899/data-512-homework_2)](https://github.com/sagnikgh1899/data-512-homework_2/blob/main/LICENSE)
# DATA 512: Human Centered Data Science (Autumn 2023)

## Homework 2: Considering Bias in Data
## Goal

The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. We will focus on articles about cities in different US states. For this assignment, we will combine a dataset of Wikipedia articles with a dataset of state populations and use a machine learning service called ORES to estimate the quality of the articles about the cities.

The expectation is to perform  an analysis of how the coverage of US cities on Wikipedia and how the quality of articles about cities varies among states. The analysis will consist of a series of tables that show:  
- The states with the greatest and least coverage of cities on Wikipedia compared to their population.
- The states with the highest and lowest proportion of high quality articles about cities.
- A ranking of US geographic regions by articles-per-person and proportion of high quality articles.  

There is also a short reflection on the project that focuses on how both the findings from this analysis and the process to reach those findings help one understand the causes and consequences of biased data in large, complex data science projects.

## Data Sources
The following are the data sources used:
- [Category:Lists of cities in the United States by state](https://en.wikipedia.org/wiki/Category:Lists_of_cities_in_the_United_States_by_state): [us_cities_by_state_SEPT.2023.csv](https://drive.google.com/file/d/1khouDmMaZyKo0y5WkFj4lu7g8o35x_98/view?usp=sharing) - The articles were webcrawled to generate a list of Wikipedia article pages about US cities from each state.
- The US Census Bureau provides updated population estimates for every US state. We can find [State Population Totals and Components of Change: 2020-2022](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html) on their website. An Excel file, **population_by_states_2022.csv**, linked to that page contains estimated populations of all US states for 2022.
- [US States by Region - US Census Bureau.csv](https://docs.google.com/spreadsheets/d/14Sjfd_u_7N9SSyQ7bmxfebF_2XpR8QamvmNntKDIQB0/edit?usp=sharing) - Regional and divisional agglomerations as defined by the US Census Bureau and used for analysis in this notebook.

**NOTE**: While **us_cities_by_state_SEPT.2023.csv** was used as is without any modifications, we executed several steps for the **population_by_states_2022.csv** as described below:
- We read the **population_by_states_2022.csv** into a dataframe called **df_pop** and **US States by Region - US Census Bureau.csv** into a dataframe called **region_df**.
- We create a dictionary called state_to_region_division to map states to their corresponding regions and divisions based on data from the region_df. This mapping is used to add "Region" and "Division" columns to the df_pop DataFrame.
- We define a custom sorting key function called custom_sort_key that assigns a sorting order to each state based on its region and division. This function is used to sort the df_pop DataFrame in a customized order.
- We perform various data manipulations, including sorting the DataFrame using the custom sorting key, filtering out rows with 'NaN' Division, and converting the "Population Estimate" column to numeric format.
- Finally, we group the data by regional divisions and calculate the population sum for each division, storing the results in a DataFrame named df_pop_division.

For getting the articles quality predictions, [ORES](https://www.mediawiki.org/wiki/ORES) (Objective Revision Evaluation Service) has been used.
These were learned based on articles in Wikipedia that were peer-reviewed using the [Wikipedia content assessment](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment) procedures.

For using the ORES for making a page quality prediction, [API:Info](https://www.mediawiki.org/wiki/API:Info) is used.  

## API documentation sample document
The below sample codes were referenced for the following tasks and have been provided under the [Creative Commons](https://creativecommons.org/) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). 
- [Making a page info request](https://drive.google.com/file/d/15UoE16s-IccCTOXREjU3xDIz07tlpyrl/view?usp=sharing)
- [Making an ORES request](https://drive.google.com/file/d/17C9xsmR9U3lJeD52UTbAedlHDetwYsxs/view?usp=sharing)

## Data files

#### Repository tree
```
.
├── input files/
│   ├── population_by_states_2022.csv
│   ├── US States by Region - US Census Bureau.csv
│   └── us_cities_by_state_SEPT.2023.csv
├── intermediate files/
│   ├── API_request_error_log.txt
│   ├── request_ores_score_per_article_output.csv
│   └── request_pageinfo_per_article_output.csv
├── output files/
│   ├── wp_scored_city_articles_by_state.csv
│   └── wp_areas-no_match.txt
├── src/
│   └── Data_Acquisition_And_Analysis.ipynb
├── LICENSE
└── README.md

```
#### Description
- **input files** : This folder contains the input datasets (**population_by_states_2022.csv**, **US States by Region - US Census Bureau.csv**, **us_cities_by_state_SEPT.2023.csv**).
- **intermediate files** : This folder contains datasets and log text files generated during program execution. The **API_request_error_log.txt** is currently empty because we encountered zero failures while making API requests. However, we have retained it in case any failures are observed in the future. The **request_pageinfo_per_article_output.csv** file holds the API call responses, and the **request_ores_score_per_article_output.csv** file contains the ORES scores.
- **output files** : This folder contains the output datasets (**wp_areas-no_match.txt**, **wp_scored_city_articles_by_state.csv**).
- **src** : A folder containing the **Data_Acquisition_And_Analysis.ipynb** file. It is clearly documented to indicate what is to be done stepwise, containing code as well as information necessary to understand each processing step.
- **LICENSE** : a file that contains an MIT LICENSE for sagnikgh1899/data-512-homework_2 repo.
- **README.md** : a file that contains information to reproduce the analysis, including data descriptions, attributions and provenance information, and descriptions of all relevant resources and documentation (inside and outside the repo) and hyperlinks to those resources. This also contains the goal and learning reflections of the project.

#### Input files
- **us_cities_by_state_SEPT.2023.csv** : The Wikipedia [Category:Lists of cities in the United States by state](https://en.wikipedia.org/wiki/Category:Lists_of_cities_in_the_United_States_by_state) was crawled to generate a list of Wikipedia article pages about US cities from each state.
- **population_by_states_2022.csv** : This dataset is drawn from [State Population Totals and Components of Change: 2020-2022](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html). An Excel file linked to that page contains estimated populations of all US states for 2022.  

**NOTE**: We utilized the **population_by_states_2022.csv** data, creating a mapping of states to regions and divisions based on **US States by Region - US Census Bureau.csv**, which was used to augment the **df_pop** DataFrame. We applied custom sorting, data manipulations, and grouped the data by regional divisions, saving the results as **df_pop_division**. The details are described above in the "Data Sources" section. 

#### Output files
- **wp_areas-no_match.txt** : All areas for which there are no matches and output a list of those areas with each area on a separate line.
- **wp_scored_city_articles_by_state.csv** : Consolidating the remaining data as instructed into a single CSV file.

#### Intermediate files
- **API_request_error_log.txt** : This log file is currently empty since we observed 0 failures while making the API request, but we have kept it in case any failures are observed.
- **request_ores_score_per_article_output.csv** : Storing the output of the API call for the ores score in this .csv.
- **request_pageinfo_per_article_output.csv** : Storing the output of the API call for getting the page info for the articles.

## Special Considerations
- The population_by_states_2022.csv file was manually cleaned to remove the extraneous headers and footers before it was used for any analysis or preprocessing in the notebook.
- The page titles in the input .csv files have not been manipulated; thus, some articles had multiple commas (,) in between. However, this did not cause any issues during processing, so we did not include additional preprocessing for that.
- Cities with duplicate values have been eliminated.
- States are mapped based on the regional and divisional hierarchy mentioned in **US States by Region - US Census Bureau.csv**.
- Error log for page info request failing is handled using a .txt file, whereas for the ORES score, it has been printed in the notebook. There are 3 instances where ORES score wasn't captured (page_title; lastrevid):
	- **Kennebunk, Maine; 1172898961**
	- **Fraser, Michigan; 1162379459**
	- **Wildwood Crest, New Jersey; 1179887888**
- The absence of ORES scores for these three articles may be due to various reasons, such as missing data in the source, temporary unavailability of ORES data, or specific issues related to these articles.

## Reproducing the analysis

To reproduce this analysis, follow these steps:

1. Clone this repository to your local machine.
2. Ensure you have Python installed with the required libraries (pandas, numpy, tqdm).
3. Run Data_Acquisition_And_Analysis.ipynb to generate all the intermediate and final output files.
4. Analyze the results.

*Feel free to use, modify, or contribute to this project while adhering to the MIT License.*

## Research Implications

**Reflection**

It is essential to understand a data science project both quantitatively and qualitatively. This project provides a comprehensive walkthrough of qualitative analysis by addressing bias in the data. These biases stem from the raw data and are influenced by various factors, such as demographic region, religion, culture, gender, and more. Through this exercise, it becomes evident that the number of articles per capita may not be the best indicator for this analysis, as these values are not proportionate to the population of a state. Additionally, there is a bias in terms of articles per capita being higher in states with smaller populations like Vermont, Maine, and Alaska. This suggests that states with lower population density may exhibit a relatively higher number of articles per capita, possibly due to various factors such as a strong local community of contributors or more prolific speakers of the English language or a focus on local topics.

**What biases did you expect to find in the data (before you started working with it), and why?**

I expected the highest quality articles to come from states with the largest number of residents, such as California or Washington. However, after the exercise, it was surprising to discover that the opposite is true. In fact, states like California, Arizona, Florida, and Massachusetts have the lowest quality articles per capita.
A major bias that I've observed is that we are only accounting for articles in English, which is inherently biased. Populous regions like California have a much greater linguistic diversity, as more than half of their population speaks a language different from English, as mentioned on this [website](https://www.worldatlas.com/articles/the-most-spoken-languages-in-california.html).

**What might your results suggest about (English) Wikipedia as a data source?**

As mentioned above, I believe that using only English articles as a data source creates a significant bias. According to the [MPI Virginia](https://www.migrationpolicy.org/data/state-profiles/state/language/VA) website, only 29.4% of naturalized citizens and 50% of noncitizens speak English less than "very well." This data point alone explains why Virginia is at the top of the list for the lowest-quality articles. These data points also suggest that it is highly biased to rely solely on English articles for this exercise.
Another anomaly is relying on ORES for the quality of the article because it is more biased toward the structure of the article rather than the quality of the content within it.

**Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?**

The Medium article [Is GPT-3 Islamophobic?](https://towardsdatascience.com/is-gpt-3-islamophobic-be13c2c6954f) discusses how OpenAI's Western algorithm perpetuates orientalist power structures. The author discusses a similar context related to the limitations of text classifiers in online content. As examples, the author presents shocking revelations where GPT-3.5 generated disturbing texts when prompted with words such as "Muslims", "Islam", or "Middle East". The article concludes by emphasizing how powerful language models like GPT-3, trained on biased data, can exacerbate inequality within linguistic categories. It also highlights the potential for generating unjust results if deployed in an unfair world, considering the scale at which it currently operates and may operate in the future.

**How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?**

Numerous areas were found to be missing from the dataset, and it would be valuable to obtain data for these omitted regions. Moreover, these regions could not be seamlessly integrated into any pre-established regional or divisional categories. As an illustration, the District of Columbia represents one such "area" that lacks a specific region or division assignment and so we excluded it from our analysis. However, upon closer examination of the population figures at the regional level, comparing the original **population_by_states_2022.csv** dataset with the one generated by the code (**df_pop_division**), a notable discrepancy was identified in the population count attributed to the 'South' region. The incongruity stems from the adoption of a specific regional division convention in the analysis, whereas an alternative convention could potentially include the "District of Columbia" within the 'South' region. Therefore, researchers must exercise their judgment and expertise in selecting the most appropriate convention to apply.

## Best practices for documentation
- PEP 8 – Style Guide for Python Code. ([Reference link](https://peps.python.org/pep-0008/))
- Use of relative path addresses to help in reproducibility.
- Use of intuitive variable and function names to ease in understanding.
- Appropriate comments and documentation provided for the data aquisition, data processing and data analysis steps.
- Description of all data files present in the repository mentioned.
- Exception Handling taken care of in the code to avoid breaking the API loops.

## Author
[Sagnik Ghosal](https://github.com/sagnikgh1899) 