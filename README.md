# data-512-a2
Bias in Data Assignment UW MSDS Data 512

### Author: Grant Savage

# Project Structure

```
data-512-a2
│   README.md
│   LICENSE
│   .gitignore
|   hcds-a2-bias.ipynb
|
└───processed_data
│   │   articles_with_missing_revids.csv
|   |   wp_wpds_countries-no_match.csv
|   |   wp_wpds_politicians_by_country.csv
|
└───raw_data
    │   page_data.csv
    |   WPDS_2020_data.csv
    │   ...
```

# Goal of the project
To explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries.

# Data Sources

The data files stored in this project were accessed and saved on 10/14/2021.

1. 
The Wikipedia [politicians by country dataset](https://figshare.com/articles/Untitled_Item/5513449) can be found on Figshare. Read through the documentation for this repository, then download and unzip it to extract the data file, which is called page_data.csv.

|Column        | Description                                        |
|--------------|----------------------------------------------------|
|rev_id 	   | The unique identifier of the article (revision_id) |
|page          | The name of the article                            |
|country       | The country the article is attributed to           |



2. 
The population data is available in CSV format as [WPDS_2020_data.csv](https://docs.google.com/spreadsheets/d/1CFJO2zna2No5KqNm9rPK5PCACoXKzb-nycJFhV689Iw/edit?usp=sharing). This dataset is drawn from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau. 

|Column        | Description                                                          |
|--------------|----------------------------------------------------------------------|
|FIPS 	       | Federal Information Processing System (unique codes for geographies) |
|Name          | The name of the geography                                            |
|Type          | The type of Geography being referred to in the Name column           |
|TimeFrame     | Time data was collected                                              |
|Data (M)      | Population in millions                                               |
|Population    | Population                                                           |

3. 
The predicted quality scores for each article in the Wikipedia dataset is obtained by querying the ORES REST API. ORES is a machine learning system. This was originally an acronym for "Objective Revision Evaluation Service" but was simply renamed “ORES”. ORES is a machine learning tool that can provide estimates of Wikipedia article quality. The article quality estimates are, from best to worst:
FA - Featured article
GA - Good article
B - B-class article
C - C-class article
Start - Start-class article
Stub - Stub-class article

These were learned based on articles in Wikipedia that were peer-reviewed using the [Wikipedia content assessment](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment) procedures.These quality classes are a sub-set of quality assessment categories developed by Wikipedia editors. ORES will assign one of these 6 categories to any rev_id you send it.

In order to get article predictions for each article in the Wikipedia dataset, I read page_data.csv into Python and then use the values in the rev_id column to make API querys.


# Dataset

After the dataset is cleaned, merged, and predictions are aquired, the ouput csv [wp_wpds_politicians_by_country.csv](https://github.com/savageGrant/data-512-a2/blob/main/processed_data/wp_wpds_politicians_by_country.csv) has the schema:

|Column                  | Description                              |
|------------------------|------------------------------------------|
|revision_id 	         | The unique identifier of the article     |
|article_name            | The name of the article                  |
|country                 | The country the article is attributed to |
|article_quality_est     | ORES prediction of article quality       |
|population              | population of the country                |

# Writeup: Reflections and Implications

My first impression was that I was quite surprised to see North Korea at the top of the list of high quality politician articles as a percentage of the country's population. Before I started working with the data I was expecting European countries to have the largest percentage of high quality politician articles. I was expecting this because I view Wikipedia as a place primarily driven by english writing authors and because many European countries have open democratic socialist societies, I would expect there to be many high quality Wikipedia articles on their politicians.

I believe North Korea is the result of bias. North Korea is known to have tight control over its internal media and I highly suspect government officials monitor and update their politician's wikipedia pages to reflect the state's desired message.

I believe the results suggest that using the English Wikipedia would not be sufficient if you were attempting to create a bias-free global model. The lowest-ranked countries in terms of high quality politician articles relative to population are a handful of small countries, some of which do not have English as a common language. They all have 0 high quality articles. I suspect this is because the rest of the world is not impacted by their internal politics and does not write about them. The countries themselves may not have many English speaking individuals, or may not leverage Wikipedia as much as other countries.

