


Forestry Analytics - Reproducible Pitch for Shiny Application
========================================================
author: Artem Braun
date: December 31st, 2017
transition: rotate
width: 1920
height: 1080

Summary
========================================================

This Assignment is part of Developing Data Products Course by Coursera. The goal is to make Shiny Application and Reproducible Pitch.

This application has applied value for Marketing Department of my employer company (forestry industry).

Web application was made using R and Shiny framework and could be accessed here:
https://abraun.shinyapps.io/Forestry_Analytics/ (please, wait for 1-2 munites while data is being downloaded).

Source code files are available on the GitHub: https://github.com/ArtemBraun/ShinyApp_Assignment

The application consists of 3 files: 
- ui.R (user Interface);
- server.R (back end);
- global.R (pre-precessing of loaded database and defining global objects for 'ui.R' and 'server.R').

Description of application
========================================================
<small>This application uses statistical data on Forestry Production and Trade, sourced from the web-repository of Food and Agriculture Organization of the United Nations: http://www.fao.org/faostat/en/#data/FO.

The main purpose of this application is to provide additional information which is unavailable on FAO Stat web-site.
Source dataframe has about 1.9mln rows and 9 variables (columns). The size of source CSV file is about 220Mb.</small>

There are 8 useful variables in source bulk dataframe:<small>  
1. Country / Group of countries (Area);  
2. Product / Group of products (Item);  
3. Year;  
4. Export Quantity (could be either 'm3' or 'ton');  
5. Export Value (USD);  
6. Import Quantity (could be either 'm3' or 'ton');  
7. Import Value (USD);  
8. Production Quantity (could be either 'm3' or 'ton')</small>  

***
Based on these variables we created 3 derivative ones:<small>  

9. Export Prices (= Export Value / Export Quantity);  
10. Import Prices (= Import Value / Import Quantity);  
11. Internal Consumption (= Production Quantity + Import Quantity - Export Quantity).  

Selecting country, product and time period, user can analyze dynamics of avarage prices in a single given country on interactive plot (**Tab: Dynamics of avarage prices in a country**).

Selecting country, product and time period, user can analyze dynamics of internal consuption in a single given country on interactive plot (**Tab: Dynamics of internal consuption in a country**).

Selecting year and product, user can analyze distribution of prices by all countries on interactive plot (**Tab: Avarage prices by countries**).</small>

Pre-processing calculations
========================================================
left: 50%
<small>Though, backend manipulations are conducted in both **'server.R'** and **'global.R'**, due to number of slides limit we provide only part of **'global.R'** contents.  
**'global.R'** is responsible for loading raw data, cleaning, transposing and creating global objects which are used in **server.R** and in **'ui.R'** as inputs.  
Source FAO Stat dataframe is not suitable for analysis. One of many problems is that the main column **Element** consists of 5 factors which should be transposed to saparate columns.</small>


|variable |classe    |first_values                                                                                                                                                                                                           |
|:--------|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Area     |character |Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afghanistan                                                                                                                                           |
|Item     |character |Wood fuel, coniferous (production), Wood fuel, coniferous (production), Wood fuel, coniferous (production), Wood fuel, coniferous (production), Wood fuel, coniferous (production), Wood fuel, coniferous (production) |
|Element  |character |Production, Production, Production, Production, Production, Production                                                                                                                                                 |
|Year     |character |1961, 1962, 1963, 1964, 1965, 1966                                                                                                                                                                                     |
|Unit     |character |m3, m3, m3, m3, m3, m3                                                                                                                                                                                                 |
|Value    |character |172596.000000, 174779.000000, 176990.000000, 179229.000000, 181496.000000, 183792.000000                                                                                                                               |

***
<small>We use function **spread** to make 5 saparate columns from one **Element**:</small>

```r
forestry <- spread(forestry, key = Element, value = Value, fill = 0)
```

<small>Now we have more effective data structure:</small>

|variable        |classe    |first_values                                                                 |
|:---------------|:---------|:----------------------------------------------------------------------------|
|Area            |character |Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afghanistan |
|Item            |character |Cartonboard, Cartonboard, Cartonboard, Cartonboard, Cartonboard, Cartonboard |
|Year            |character |1999, 1999, 2000, 2000, 2001, 2001                                           |
|Unit            |character |1000 US$, tonnes, 1000 US$, tonnes, 1000 US$, tonnes                         |
|Export Quantity |character |0, 0, 0, 0, 0, 0                                                             |
|Export Value    |character |0, 0, 0, 0, 0, 0                                                             |
|Import Quantity |character |0, 25.000000, 0, 3.000000, 0, 3.000000                                       |
|Import Value    |character |21.000000, 0, 2.000000, 0, 2.000000, 0                                       |
|Production      |character |0, 0, 0, 0, 0, 0                                                             |

<small>Except **spread**, we also used **aggregate**, **mutate** and **subset** for tidying this dataftame.</small>


Example of interactive plot
========================================================
<small>There are 3 types of plots available in this web-application. Following is the example of one of them.  
If user selects Canada as a country and Industrial Roundwood as a product, he can analyze dynamics of internal consuption of Industrial Roundwood in Canada on the following graph:</small>
![plot of chunk unnamed-chunk-5](./Plot2.png)