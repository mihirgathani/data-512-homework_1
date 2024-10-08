# **Data 512: HW 1 - Professionalism & Reproducibility**

## Project Goal
The goal of this assignment is to construct, analyze, and publish a dataset of monthly article traffic for desktop and mobile access for a select set of pages from English Wikipedia from July 1, 2015 through September 30, 2024. The purpose of the assignment is to develop and follow best practices for open scientific research as exemplified by the repository. We are specifically focusing on a subset of the English Wikipedia articles that represents a large number of rear diseases which were collected from the [National Organization for Rare Diseases (NORD)](https://rarediseases.org). The analysis involves data acquisition from the [Wikimedia Analytics API](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html) and subsequent processing to gain insights into article views.  The main objectives include identifying articles with the highest and lowest pageviews, determining trends over time, and discovering the articles with the fewest months of available data. 

## Licenses

**Source Data**

- The data used in this project is sourced from the Wikimedia Foundation using [Wikimedia Analytics API](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html). It is under the [MIT license](https://opensource.org/license/mit-0) for code and [creative commons](https://creativecommons.org/licenses/by-sa/4.0/deed.en) otherwise. It is also subject to the [Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use). In accordance with these terms, the dataset created and used in this project is for research and analytical purposes. I have ensured this data should comply with the Wikimedia Foundation's policies regarding data usage, privacy, and attribution.

- In order to get the specific subset of articles for our dataset, we use data from [National Organization for Rare Diseases (NORD)](https://rarediseases.org) to get a list of rear diseases as provided in [rare-disease_cleaned.AUG.2024.csv](rare-disease_cleaned.AUG.2024.csv). It is important to ensure that their [Terms and Conditions](https://rarediseases.org/terms-conditions/) are followed.


**Data Acquisition Code**

- Parts of the code in the ["Wikipedia_Article_Traffic_DataAcquisition.ipynb"](./Wikipedia_Article_Traffic_DataAcquisition.ipynb") file were taken as-is or with minimal changes from the [example code](https://drive.google.com/file/d/1fYTIX79t9jk-Jske8IwysV-rbRkD4_dc/view?usp=drive_link) that was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024


## API Documentation
This project uses the following Wikimedia APIs for data collection:

- **Wikimedia Analytics API**: This is the primary API used to retrieve pageview statistics for the specified articles. More details can be found in the official documentation: [Wikimedia Analytics API Documentation](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/). We gather data from July 1, 2015 to September 30, 2024.

## Steps to run the code
In order to make the graphs, you would have to run the following steps which involves running two files for modularization the ease of understanding.

- **Pre-run Information:** In order to successfully run the code, you need to clone/ download the repository and upload it to a Jupyter Environment (a place where .ipynb files can run). After this, you will need to ensure that the required Python packages are installed. Instructions to do so can be found in the individual code files.

Now that you have the entire repository cloned and environment setup, you can follow the steps:

1. First, you need to run the ["Wikipedia_Traffic_Analysis_Part1_DataAcquisition.ipynb"](./Wikipedia_Traffic_Analysis_Part1_DataAcquisition.ipynb) file. In order to do this, you need to click on run all cells or restart kernal and run all cells button in the jupyter notebook. 
**Note-** Since the data acquisition process calls the API to get the required data, this file would take about 45 minutes to run and store the results into JSON files. To aid in speeding up the process, you can find the JSON files in the [JSON Data Files](./JSON%20Data%20Files/) folder. 

2. The final step is to run the ["Wikipedia_Traffic_Analysis_Part2_DataAnalysis.ipynb"](./Wikipedia_Traffic_Analysis_Part2_DataAnalysis.ipynb)
file. In order to do this, you need to click on run all cells or restart kernal and run all cells button in the jupyter notebook.


## Data and Final Output Files
During the execution of the project, several data and final output files are created. Below is a list of these files and their descriptions:

- **JSON Data Files**: We created three JSON files using the data gathered from the Pageviews API.

   1. [`rare-disease_monthly_desktop_201507-202409.json`](./JSON%20Data%20Files/rare-disease_monthly_desktop_201507-202409.json): Contains monthly pageview data for rare disease articles accessed via desktop from July 2015 to September 2024.

   2. [`rare-disease_monthly_mobile_201507-202409.json`](./JSON%20Data%20Files/rare-disease_monthly_mobile_201507-202409.json): Contains monthly pageview data for rare disease articles accessed via mobile platforms (both app and web) for the same time period.

   3. [`rare-disease_monthly_cumulative_201507-202409.json`](./JSON%20Data%20Files/rare-disease_monthly_cumulative_201507-202409.json): Aggregates the desktop and mobile views to show the cumulative pageviews.

**Data Schema**: The three JSON files created store data with the following structure.  

```json
{
    "article_title": [
        {
            "project": "string",  # Wikipedia project (e.g., en.wikipedia)
            "article": "string",  # Name of the Wikipedia article
            "granularity": "string",  # Data granularity (monthly in our case)
            "timestamp": "string",  # Month in format YYYYMM
            "agent": "string", #Type of user agent
            "views": "int"  # Number of page views
        },
        ...
    ]
}
```

- **Final Output Files:** We performed a visual analysis on the JSON files and created three timeseries graphs.

   1. [`max_min_avg_analysis.png`](./Generated%20Graphs/max_min_avg_analysis.png): This graph shows a visualization of the articles with the highest and lowest average pageviews for both desktop and mobile platforms over the entire time series.

   2. [`top_10_peak_views_analysis.png`](./Generated%20Graphs/top_10_peak_views_analysis.png): This graph shows the top 10 article pages by largest (peak) page views over the entire time series on both mobile and desktop platforms. The graph contains the top 10 for desktop and top 10 for mobile access (20 lines).

   3.  [`fewest_months_of_data_analysis.png`](./Generated%20Graphs/fewest_months_of_data_analysis.png): This graph shows pages that have the fewest months of available data. The graph shows the 10 articles with the fewest months of data for desktop access and the 10 articles with the fewest months of data for mobile access.


## Issues:
One major issue that I encountered was that the code for data acquistion failed when it came across a `/` in the article title. This was becuase the `/` was triggering an encoding error (Ex: Article title - `Sulfadoxine/pyrimethamine`). The original code was taken from Dr. David's work and then modified to fix the issue.

Original Code:
 Original code:
``` python
urllib.parse.quote(request_template['article'].replace(' ','_'))
```

Updated Code with solution:
```python
urllib.parse.quote(request_template['article'].replace(' ','_'), safe='')
```
The reason that the solution works is because the safe='' parameter makes sure that the special characters are encoded correctly.

## Potential Considerations: 
The following considerations are important to consider when using this code:

- **Incomplete Time Series**: Some rare disease articles may have incomplete pageview data due to either the creation date of the article or periods of inactivity (no pageviews recorded). Inavailability of these articles could also play a role.

- **API Rate Limits**: Although the code accounts for API rate limits, there might be a possibility that it may affect data acquistion, especially in case the API changes.

- **API Changes:** There is a possibility that the Wikimedia API may undergo changes over time causing inconsistency in data, and potentially different results. Users of this project should check for any updates to the API that might affect data acquisition or analysis methods.

- **Article Title Changed:** In case an article is renamed, or updated in a wrong way, it may lead to data inconsistencies. This is possible since Wikipedia relies on it's users for the data.

- **Bias in Data Representation**: Wikipedia data is user-generated and pageview statistics can be influenced by many external factors unrelated to the relevance of the disease. Popular media coverage or social media trends can cause sudden spikes in views, which do not necessarily correlate with the actual importance of the topic.

- **Monthly Mobile Data JSON File**: Since the API separates mobile access into app and web, it is important to remember that the views in the file[`rare-disease_monthly_mobile_201507-202409.json`](./JSON%20Data%20Files/rare-disease_monthly_mobile_201507-202409.json) is a sum of both app and web based views. 



