CS 424 - Visualization & Visual Analytics

Name: Chesta Dewangan  
Email: cdewa3@uic.edu

# Assignment 3

## Dataset

#### Overview
The dataset **[311 Service Requests](https://data.cityofchicago.org/Service-Requests/311-Service-Requests/v6vf-nfxy/about_data)** contains 311 service requests received in city of Chicago.

#### Libraries
Pandas, Geopandas, Vega-Altair & Shapely

#### Preprocessing

Since the original dataset is about ~4 GB with 11.3 million rows and 39 columns. So, I trimmed the original dataset and created a new dataset with requests made from 2021 onwards. The preprocessing is done in [csv_handler](csv_handler.ipynb) Jupyter notebook.

## Task 1 - Single view visualizations

### Visualization 1 - Heatmap of Requests

<img src="/viz_gifs/task1/viz1.gif" alt="viz1" height="500">\
    <ins>Image 1.1</ins>: Heatmap of Requests

#### Description

1. Attributes Visualized - CREATED_DATE (Month & Day)

2. Motivation and Rationale - This visualization is useful for understanding the data as different months and days of the month could factor in the 311 requests in Chicago. Some days or months could usually have higher averages than others; this visualization unfolds that.

#### Interaction Mechanism

The only mechanism is hovering or clicking over a block/tile in the heatmap, which reveals the exact day of month, month & average request count on that day & month.

#### Design Choices

1. Color schemes: Sequential Single-Hue Schemes - Blues
2. Encoding choices:
- X-axis maps day of the month and Y-axis maps month 
- Mapped the average REQUEST_COUNT to the heatmap’s color intensity to represent frequency.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Grid format for heatmap

#### Experimentation Process

The idea started with aggregating the data for the heatmap and plotting it. Initially, the color scheme was viridis, as I wanted to see how it looks in a sequential multi-hue scheme. Then, I changed the color scheme to red to depict the frequency correctly. The red scheme is usually used to depict urgency or danger, and since the heatmap is for representing request count, a calmer color would be better for the visualization, and that's how the visualization was finalized.

### Visualization 2 - Average time taken by a department to resolve a request (hours)

<img src="/viz_gifs/task1/viz2.gif" alt="viz2" height="500">\
    <ins>Image 1.2</ins>: Average time taken by a department to resolve a request (hours)

#### Description

1. Attributes Visualized - CREATED_DATE, CLOSED_DATE, OWNER_DEPARTMENT
2. Motivation and Rationale - This visualization helps understand the data as understanding different resolution times (closed date - created date) can help us, or domain experts, look at the visualization to determine which department is resolving a request faster than others and try to look for the reason to improve the resolution time.

#### Interaction Mechanism

The only mechanism is hovering or clicking over a piece of the donut, which reveals the department and the average resolution time of the department.

#### Design Choices

1. Color schemes: Categorical color scheme
2. Encoding choices:
- Theta encoding - Represents proportions of each department. The larger the value, the bigger the segment.
- Color encoding - Differentiates departments visually using distinct colors. Ensures clear distinction between segments.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Donut chart are a better version of pie chart which is informative and appealing.

#### Experimentation Process

The process started by thinking about the design that can be made for the purpose, I had pie chart, donut chart, radial chart and polar bar chart in consideration. I finalized donut chart as it is similar to pie chart but better informative and appealing and also since vega-lite allows hover and showing information, the middle hole helps in reading that information better. Now for color scheme the strategy was to use distinct colors that differentiates departments visually. So, I used categorical color scheme for that. 

### Visualization 3 - Request Dot Density by Department in Chicago

<img src="/viz_gifs/task1/viz3.gif" alt="viz3" height="500">\
    <ins>Image 1.3</ins>: Request Dot Density by Department in Chicago

#### Description

1. Attributes Visualized - LATITUDE, LONGITUDE & OWNER_DEPARTMENT
2. Motivation and Rationale - This visualization is useful for understanding the data as to which places in the Chicago land have different department requests and in what volume.

#### Interaction Mechanism

The only mechanism is hovering or clicking on a point in the map, which reveals the exact department, lat, long, and request count.

#### Design Choices

1. Color schemes: The same categorical color scheme as viz 2.
2. Encoding choices:
- Longitude & Latitude - Longitude is the X position of the dot & Latitude is the Y position of the dot.
- Color encoding - Categorical color palette ensures distinct colors and each color represents a department.
- Size encoding - Represents request count at a location.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Dot size /ange to balance dot visibility plus avoiding clutter. A gray base to improve readability of the dots.

#### Experimentation Process

Different visualizations can be considered to represent spatial attributes like dot density maps, choropleth maps, pie charts on maps & bar charts on maps among others. The best choice to represent what I wanted to be represented (spread) is dot density map. I tried various colors schemes, but the categorical one was much better as it was able to distinct departments. For the size of the dot, I again tried different sizes, some ranges resulted in too big dots covering up the others, while some were too small to read. I settled with range [40, 800]. The last issue was the big data even after making a subset so I had to choose sample of 5000 points to plot.

### Visualization 4 - Request Count by zip in Chicago

<img src="/viz_gifs/task1/viz4.gif" alt="viz4" height="500">\
    <ins>Image 1.4</ins>: Request Count by zip in Chicago

#### Description

1. Attributes Visualized - ZIP_CODE
2. Motivation and Rationale - This visualization is useful for understanding the data on zip granularity and see what is the request ount on average in different zip levels.

#### Interaction Mechanism

The only mechanism is hovering or clicking on a block in the map, which reveals the exact zip and request count.

#### Design Choices

1. Color schemes: Sequential Single-Hue Schemes - Blues
2. Encoding choices:
- Geographical encoding - Using zip code boundaries to have the shape to fill.
- Color encoding - Sequential color scheme for properly mapping high to low values
- Tooltip encoding - Hovering or clicking shows exact information

#### Experimentation Process

A choropleth map is a better & intuitive way to represent request count in zip codes. I say this because I started by considering a dot density map and a choropleth map. I tried visualizing a dot density map. With the method I used (aggregating) and the purpose I wanted to serve, which is showing the distribution was not well reflected by the dot density map as it will have only one dot per zip code, which doesn't tell much about **distribution** at first glance. Plus, the possible options have a sequential color scheme or a categorical; since it is categorical, using sequential could lead to misleading. Some points could be very light, and some could be dark. Visualizing the color over an area is better in that case. That's why I went ahead with choropleth map. The problem came in when a zip code (60612) had a very high number, and that could be because of various reasons, so I had to nullify the zip code value so that we could find the information. I needed a color scheme that could depict high to low instead of differentiating, so I chose blues as a color scheme similar to viz 1.

<img src="/experimentation_images/task1/1.jpeg" alt="viz" height="400">\
    <ins>Image 2.5.1</ins>: Dot Density Map of Requests by ZIP

<img src="/experimentation_images/task1/2.jpeg" alt="viz" height="400">\
    <ins>Image 2.5.2</ins>: Choropleth Map of Requests by ZIP

### Visualization 5 - Distribution of Requests by Owner Department

<img src="/viz_gifs/task1/viz5.gif" alt="viz5" height="500">\
    <ins>Image 1.5</ins>: Distribution of Requests by Owner Department

#### Description

1. Attributes Visualized - OWNER_DEPARTMENT
2. Motivation and Rationale - This visualization is useful for understanding the data as it can tell which departments get the highest requests and can help domain experts understand the reason behind it.

#### Interaction Mechanism

There is no interactive mechanism.

#### Design Choices

1. Color schemes: Categorical color scheme
2. Encoding choices:
- X axis & Y axis - Department name on x axis and total requests per department on y axis.
- Color encoding - Color by department for better differentiation.
3. Layout consideration: Vertical violin plots to represent the length of the requests or count of requests and its easier to compare each department.

#### Experimentation Process

The visualization options for distribution are violin plot, bar chart & packed circle chart. A packed circle chart could be ambiguous to see as two or more departments can have different request counts, but because of scaling with the other departments, it could be misleading. So, it narrowed down to a violin plot or bar chart. The violin chart is more appealing and informative, so I created a violin plot. The color scheme needed to be the categorical color scheme. The only problem the chart had was with the department since some of the requests were under 311 city services, and it was a huge portion; it was making the plot more complicated to read, so I removed that department and others, which was not adding to the information.

### Visualization 6 - Requests Per Department (Year 1 vs Year 2)

<img src="/viz_gifs/task1/viz6.gif" alt="viz6" height="350">\
    <ins>Image 1.6</ins>: Requests Per Department (Year 1 vs Year 2)

#### Description

1. Attributes Visualized - OWNER_DEPARTMENT, CREATED_DATE, CLOSED_DATE
2. Motivation and Rationale - This visualization is useful for understanding the data by comparing the department fraction in two years to analyze which department is growing and what's the reason behind it.

#### Interaction Mechanism

There are two interactions; the first is hovering or clicking over a portion of the pie, which reveals the exact department and the request count, but that's filtered by the second interaction, which is picking 2 years from the available year in the dataset from a dropdown. That filters each pie chart or aggregates the request count for that year and shows that.

#### Design Choices

1. Color schemes: Categorical color scheme
2. Encoding choices:
- Theta encoding - Represents the total requests per department.
- Color encoding - Having consistent categorical color scheme.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Side-by-side alignment to have a better comparison view. Titles for the correct pie chart on top of the pie chart to show which pie chart is for which year.

#### Experimentation Process

The simple visualization options for comparison are pie chart, stacked bar chart & nested proportional area chart. Since I wanted the filter option for the years and easy readable comparison, I decided to have two pie charts side-by-side. The side-by-side layout allowed for easy visualization of the comparison and a consistent color scheme to show that there are the same departments in both pie charts. However, they represent stats of different years, and that's how my visualization was complete.


## Task 2 - Linked view visualizations

For this task, I started by thinking of every possibility of combining two visualizations out of the six made in task 1. Out of these, I selected the four that could reveal different information about the data than before using different interaction mechanisms and methods.

### Visualization 1

<img src="/viz_gifs/task2/viz1.gif" alt="viz1" height="600">\
    <ins>Image 2.1</ins>: Heatmap of Requests + Average time taken by a department to resolve a request (hours)

#### Description

1. Attributes Visualized - CREATED_DATE, CLOSED_DATE, OWNER_DEPARTMENT
2. Motivation and Rationale - This visualization helps understand the data as it can provide an understanding of the volume of requests, the efficiency of different departments, and the relationship between them. The heatmap communicates the density of requests per department, and the donut chart helps the average time to resolve requests, which gives insights into performance and can help domain experts with that information.

#### Interaction Mechanism

Clicking one block/tile in the heatmap filters or manipulates the data on time; based on that, the donut chart updates. It still shows the same information but for the day and month clicked on the heatmap. Hovering over the heatmap or donut chart still works and reveals the information mentioned in task 1.

#### Methods Used

- Aggregate the request count by department and time.
- Filter the resolution time for the day and month.

#### Design Choices

1. Color schemes: For heatmap, blues & for the donut chart categorical color scheme
2. Encoding choices: 
- X-axis maps day of the month and Y-axis maps month (heatmap)
- Mapped the average REQUEST_COUNT to the heatmap’s color intensity to represent frequency.
- Theta encoding - Represents proportions of each department. The larger the value, the bigger the segment (donut).
- Color encoding (donut) - Differentiates departments visually using distinct colors. Ensures clear distinction between segments.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Since the heatmap is wide, both visualizations are stacked.

#### Experimentation Process

Since the motivation was clear, combining the heatmap and donut chart was a good option. I tried both layouts, having the heatmap and donut chart side-by-side and the heatmap stacked on top of the donut chart. The heatmap is broad, and making it too small was not a good option; even making it scroll a lot also blurs the purpose of seeing the change happen when the donut chart changes based on the selection in the heatmap. So, I went ahead and stacked them. As discussed before, the color scheme choice for both visualizations was clear. The interaction also showed that the donut chart will be manipulated based on the heatmap. That's how the visualization was created.

<img src="/experimentation_images/task2/1.gif" alt="viz1" height="300">\
    <ins>Image 2.1.2</ins>: Heatmap of Requests + Average time taken by a department to resolve a request (hours)

### Visualization 2

<img src="/viz_gifs/task2/viz2.gif" alt="viz2" height="600">\
    <ins>Image 2.2</ins>: Heatmap of Requests + Distribution of Requests by Owner Department

#### Description

1. Attributes Visualized - OWNER_DEPARTMENT & CREATED_DATE (MONTH & DAY)
2. Motivation and Rationale - This visualization is useful for understanding the data and to present the departmental variations in request distribution across different day and month.

#### Interaction Mechanism

Clicking one department in the violin chart shows a heatmap for the department by manipulating the data and using selection on the heatmap, one can see the details about the block/tile.

#### Methods Used

- Aggregate the request count by the department.
- Filter the day and month based on user selection of department.

#### Design Choices

1. Color schemes: categorical for the violin plot and blues for the heatmap
2. Encoding choices:
- X-axis maps day of the month and Y-axis maps month (heatmap)
- Mapped the average REQUEST_COUNT to the heatmap’s color intensity to represent frequency.
- X axis & Y axis - Department name on x axis and total requests per department on y axis (violin).
- Color encoding (violin) - Color by department for better differentiation.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Both visualizations are stacked since the heatmap & violin plot is wide.

#### Experimentation Process

Since I knew what I wanted, combining the heatmap and violin plot was a good option. I tried both layouts, having the heatmap and violin plot side-by-side and the heatmap stacked on top of the violin plot. Both of them are broad, and making it too small was not a good option; even making it scroll a lot, that too twice, also blurs the purpose of seeing the change happen when the heatmap changes based on the selection in the violin plot. So, I went ahead and stacked them. As discussed before, the color scheme choice for both visualizations was clear. The interaction also showed that the heatmap will be manipulated based on the violin plot. That's how the visualization was created.

### Visualization 3

<img src="/viz_gifs/task2/viz3.gif" alt="viz3" height="600">\
    <ins>Image 2.3</ins>: Distribution of Requests by Owner Department + Request Dot Density by Department in Chicago

#### Description

1. Attributes Visualized - OWNER_DEPARTMENT, LATITUDE, LONGITUDE
2. Motivation and Rationale - This visualization is helpful in understanding the data as it shows both categorical distribution plus geospatial distribution of requests by the department in Chicago.

#### Interaction Mechanism

Clicking on a department on the violin plot manipulates the data by filtering the department and then also manipulates the visual mapping by dynamically changing the color scheme on the geospatial map based on user selection. Users can click or hover over the dot on the map and see the stats clearly.

#### Methods Used

- Aggregate the request count by the department.
- Filter the points (long, lat) based on department selection.

#### Design Choices

1. Color schemes: Categorical color scheme for the violin plot and singular dynamically changing color for geospatial map
2. Encoding choices:
- X axis & Y axis - Department name on x axis and total requests per department on y axis (violin).
- Color encoding (violin) - Color by department for better differentiation.
- Longitude & Latitude (map) - Longitude is the X position of the dot & Latitude is the Y position of the dot.
- Color encoding (map) - Categorical color palette ensures distinct colors and each color represents a department.
- Size encoding (map) - Represents request count at a location.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Both visualizations are stacked since the violin plot is wide.

#### Experimentation Process

Since I knew what I wanted, combining the dot density map and violin plot was a good option. I tried both layouts, having the map and violin plot side-by-side and the map stacked on top of the violin plot. The violin plot is broad, and making it too small was not a good option; even making it scroll a lot also blurs the purpose of seeing the change happen when the map changes based on the selection in the violin plot. So, I went ahead and stacked them. As discussed before, the color scheme choice for both visualizations was clear. The interaction also showed that the map will be manipulated & change color dynamically based on the violin plot. That's how the visualization was created.

### Visualization 4

<img src="/viz_gifs/task2/viz4.gif" alt="viz4" height="600">\
    <ins>Image 2.4</ins>: Requests Per Department (Year) in a pie chart + Request Count by zip in Chicago

#### Description

1. Attributes Visualized - YEAR (CREATED_DATE), OWNER_DEPARTMENT, ZIP_CODE
2. Motivation and Rationale - This visualization is useful for understanding the data by showing the department's requests' distribution over the years and the geospatial variation of requests across Chicago's zip code. It can be duplicated and made for comparison, too, based on the use.

#### Interaction Mechanism

By selecting a year, the data is manipulated for the pie chart and by selecting a department, the geospatial map changes. Even though the color scheme stays the same for the map but based on the selection the map's visual mapping changes as it is a choropleth map. By hovering over a block on the map, user can see the details.

#### Methods Used

- Aggregate the request count by the department & zip code.
- Filter the pie chart by year and the points first by year and then zip code.

#### Design Choices

1. Color schemes: Categorical color scheme for the pie chart and blues color scheme for the map
2. Encoding choices:
- Geographical encoding (map) - Using zip code boundaries to have the shape to fill.
- Color encoding (map) - Sequential color scheme for properly mapping high to low values.
- Theta encoding (pie chart) - Represents the total requests per department.
- Color encoding (pie chart) - Having a consistent categorical color scheme.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Both visualizations are stacked.

#### Experimentation Process

Since I knew what I wanted, combining the pie chart and choropleth map was a good option. I went ahead and stacked them. As discussed before, the color scheme choice for both visualizations was clear. The interaction also showed that the year selection will be manipulating the pie chart & pie chart will manipulate the map. I tried both the slider and drop-down menu for the year selection. Since the year selection only manipulates the pie chart and not both, I decided to go ahead with the drop-down. I would have considered/used a slider if it manipulated both. That's how the visualization was created.

## Task 3 - Spatial visualization

Since, for the previous tasks, I used all of the geospatial attributes except COMMUNITY_AREA, I wanted to visualize that granularity for this task. But for that, I needed the geojson or dataset with community area shape, which was missing in chicago.geojson. For this, I looked up in the Chicago data portal and found the [community area boundaries](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Community-Areas-current-/cauq-8yn6) dataset, which I downloaded extra for this task and worked around with.

### Visualization - Choropleth Map of Requests by Community Area

<img src="/viz_gifs/task3/viz.gif" alt="viz" height="500">\
    <ins>Image 3.1</ins>: Choropleth Map of Requests by Community Area

#### Description

1. Attributes Visualized - COMMUNITY_AREA (number) & COMMUNITY (from the other dataset which is the name)
2. Motivation and Rationale - This visualization is useful for understanding the data at a different granularity than before. The visualization shows the distribution across different community areas. It also helps in comparing distribution across different zip codes or just places (long, lat) in Chicago.

#### Interaction Mechanism

The only mechanism that this visualization has is that by clicking or hovering over the blocks, one can see the community area name and the request count in that area.

#### Methods Used

- Aggregated request counts on the community area (number).
- Spatial arranged with the Chicago boundaries.

#### Design Choices

1. Color schemes: Sequential Single-Hue Schemes - Blues
2. Encoding choices:
- Geographical encoding - Using community area boundaries to have the shape to fill.
- Color encoding - Sequential color scheme for properly mapping high to low values
- Tooltip encoding - Hovering or clicking shows exact information

#### Experimentation Process

Since I wanted to look at the community area granularity, I started to think about the geospatial plot options that could best serve the purpose. The first option was to have a dot density map and show the distribution, which would mean having one dot in one area and having sequential color scheme that don't make sense as it's categorical. Now, using the multiple colors will work. Still, the map, in general, is not very informative. If I have dots, I could have the outline of the community area, but that could be misleading just at first glance as it could reflect that there is only one data point in that community area or one request; it doesn't show **distribution** which was the motivation of creating the map. I tried my second option of having a choropleth map, which I already had experimented with and used. So, I made it with the same color scheme as before to be consistent, and it nicely shows the distribution across community areas.

<img src="/experimentation_images/task3/1.jpeg" alt="viz" height="400">\
    <ins>Image 3.2.1</ins>: Dot Density Map of Requests by Community Area

<img src="/experimentation_images/task3/2.jpeg" alt="viz" height="400">\
    <ins>Image 3.2.2</ins>: Choropleth Map of Requests by Community Area

## Task 4 - Linked spatial visualization

### Visualization

<img src="/viz_gifs/task4/viz.gif" alt="viz" height="700">\
    <ins>Image 4.1</ins>: Requests Per Department (Year) in a pie chart + Request Dot Density by Department in Chicago + Choropleth Map of Requests by Community Area

#### Description

1. Attributes Visualized - YEAR (CREATED_DATE), OWNER_DEPARTMENT, LATITUDE, LONGITUDE, COMMUNITY_AREA (number) & COMMUNITY (name from the other dataset)
2. Motivation and Rationale - This visualization is useful for understanding the data as it integrates its spatial, categorical, and non-spatial attributes. It enables close work to visual analytics systems. The goal is to provide is having an interactive experience that helps people to explore. It helps in analyzing different patterns over time and by department.

#### Interaction Mechanism

- By using the over-the-year slider, the visualization manipulates the data for pie charts and choropleth maps. Department selection in the pie chart also manipulates the data for the dot density map.
- Selecting the department also manipulates the visual mapping by linking the categorical and spatial data.
- By hovering over the maps and pie chart, the view also changes and adds more details.

##### Meaningful interaction between Spatial and non-spatial atttributes

- Brushing & Linking: Selecting a department in a pie chart, the dot density map highlights only relevant request points correlating the volume of requests with location.
- Drill-down exploration: Clicking or sliding helps to filter data first by year, then department or just year, and by hovering/clicking to get more information.
- Time based analysis: Year slider helps temporal analysis and connecting between spatial and non-spatial attributes and the link or trends. 

#### Methods Used

- Aggregate request count at the community level and year plus department.
- Filtering by department happens for the dot density map.
- By hovering over the points in the dot density map, blocks in the choropleth map or the portions of the pie chart reveal more information.
- Drill-down exploration first by year, then department or just year, and by hovering/clicking to get more information.

#### Design Choices

1. Color schemes - Categorical color scheme for the pie chart, corresponding color for the dot density map, and blues for the choropleth map.
2. Encoding choices:
- Geographical encoding (choropleth map) - Using zip code boundaries to have the shape to fill.
- Color encoding (choropleth map) - Sequential color scheme for properly mapping high to low values.
- Theta encoding (pie chart) - Represents the total requests per department.
- Color encoding (pie chart) - Having a consistent categorical color scheme.
- Longitude & Latitude (dot density map) - Longitude is the X position of the dot & Latitude is the Y position of the dot.
- Color encoding (dot density map) - Categorical color palette ensures distinct colors, and each color represents a department.
- Size encoding (dot density map) - Represents request count at a location.
- Tooltip encoding - Hovering or clicking shows exact information
3. Layout considerations: Since year manipulates the pie chart and the choropleth map, it's stacked together as the slider appears at the bottom left. The dot density map is manipulated by the pie chart so next to it horizontally.

#### Experimentation Process

The motivation was clear, so I started with the same idea and experimentation for pie charts, dot density maps, and choropleth maps, as mentioned before. The main experiment was with the layout; I thought about different options, first having the pie chart on the top with the top maps at the bottom of it or vice versa, which didn't have any disadvantages I could think of. Then I thought about having the pie chart and choropleth map together horizontally, and dot density map at the bottom, which works well in terms of both the pie chart and choropleth map being manipulated together by the slider so they can be manipulated together. But then I thought about the last possibility of having the pie chart and the choropleth map stacked together and the dot density map next to the pie chart horizontally because the reason year slider, which appears at the bottom left, manipulates the pie chart and the choropleth map and the person could just look at that focus on it once the user wants to drill down on for the department could look at the first row and see the manipulation which I think would be visually better. So I went ahead with that, and the color scheme & encoding, as discussed before, got clear because of the experimentation in the previous task.

<img src="/experimentation_images/task4/1.png" alt="viz" height="400">\
    <ins>Image 4.2.1</ins>: Requests Per Department (Year) in a pie chart + Request Dot Density by Department in Chicago + Choropleth Map of Requests by Community Area

<img src="/experimentation_images/task4/2.png" alt="viz" height="700">\
    <ins>Image 4.2.2</ins>: Requests Per Department (Year) in a pie chart + Request Dot Density by Department in Chicago + Choropleth Map of Requests by Community Area    

## References 
1. [Color Schemes Vega](https://vega.github.io/vega/docs/schemes/)
2. [Input Selection Vega-Altair](https://altair-viz.github.io/altair-viz-v4/user_guide/interactions.html)
3. [Binding, Selections, Linking](https://altair-viz.github.io/altair-viz-v4/user_guide/interactions.html)
4. [Community Boundaries Dataset](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Community-Areas-current-/cauq-8yn6)
5. [Vega-Altair Examples](https://altair-viz.github.io/gallery/index.html)