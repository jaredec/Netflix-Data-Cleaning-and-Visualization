## Netflix Data Cleaning and Visualization

**Project description:** This project involves the analysis of Netflix movie and TV show data from 2008 to 2021. Leveraging SQL for data cleaning and transformation, the dataset is then prepared for interactive visualization using Tableau, enabling the exploration of content trends, genres, and other valuable insights.


### Dataset
The dataset was obtained from [Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows){:target="_blank"}, comprising listings of all the movies and TV shows available on Netflix from 2008 to 2021. The dataset consists of 12 columns and 8807 rows of data in a structured CSV format. These columns include:

- **Show ID:** Unique identifier.
- **Type:** Movie or a TV show.
- **Title:** Content title.
- **Director:** Director(s).
- **Cast:** Movie/TV show cast.
- **Country:**  Production country/countries.
- **Release Year:** The year when the movie or TV show was released.
- **Rating:** Content rating (e.g., PG, TV-MA).
- **Duration:** The duration in minutes.
- **Listed in:** Genre(s) on Netflix.
- **Description:** Brief description or synopsis.
- **Date Added:** Date when added to Netflix.

### Cleaning the data with SQL

While this dataset is considerably clean, we still need to make a few adjustments such as removing duplicates, populating null values, and preparing the data for visualization. I have included the first step in the process below:

*Check for duplicates*
```
SELECT show_id, Occurence= COUNT(*)
FROM NetflixData
GROUP BY show_id
HAVING COUNT(*) > 1
```
*No duplicates*


*Double check using the description column:*
```
SELECT description, description_count = COUNT(*)
FROM NetflixData
GROUP BY description
HAVING COUNT(*) > 1
```
*There are instances where the movie is the same but dubbed in another language. These will be removed as it will inflate results for that title*


*Delete the duplicates:*
```
WITH DupeCTE as
(
	SELECT *, ROW_NUMBER()
	OVER (PARTITION BY description
	ORDER BY description) 
	AS RowNumber FROM NetflixData
) 

DELETE
FROM DupeCTE
WHERE RowNumber > 1
```

View the full SQL code [here](https://github.com/jaredec/jaredec.github.io/blob/master/projects/net/NetflixData.sql){:target="_blank"}.


### Visualizing the Data in Tableau

View the dashboard in Tableau Public [here](https://public.tableau.com/views/NetflixInteractiveDashboard_16881575833210/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link){:target="_blank"}.

#### Dashboard Features

- **Filters:** Users can utilize filters located in the upper right corner of the dashboard to sort and filter content based on specific criteria such as genre, release year, country, and type (movie or TV show).
- **Interactive Charts:** The dashboard includes interactive charts and visualizations that dynamically update based on user selections. These charts provide insights into content distribution, trends over time, and other relevant metrics.
- **Geographical Analysis:** Users can explore content availability and preferences across different countries using interactive maps and geographical filters.

#### Dashboard Navigation

Users can navigate through the dashboard by clicking directly on the visualizations or using the interactive filters to refine their selection criteria. In case the "Reset Filters" button experiences difficulty caching, refresh your page to ensure the filters reset properly. 

#### Example Use Cases

- Filtering by TV Shows in India:
<img src="images/net_demo1.gif?raw=true"/>

- Alternatively, you can also filter by clicking directly on the dashboard; filtering by content in Japan:
<img src="images/net_demo2.gif?raw=true"/>

> Data from [Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows{% raw %}:target="_blank"{% endraw %})