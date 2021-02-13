# big-cities
    Our project sought to compare the quality of life in 26 major U.S. cities using data from the U.S. Centers for Disease Control and Prevention. The data set includes variables that pertain to demographics and indicators of health, income, and education level. 

    We intended to do this by producing interactive plots, making linear models of key predictors, and comparing cities based on their “Livability”, a variable we created representing relative levels of positive and negative predictors of quality of life. 

Our hypothesis was that the “Livability” of Detroit, MI would be lowest
because it is known for its weak economy, high levels of poverty, and
high crime rates. Meanwhile, we predicted Boston, MA and Portland, OR
would be relatively high on the list as they are known to be cities with
thriving economies and rich local cultures.

    The main dataset we used was the U.S. Centers for Disease Control and Prevention’s Big Cities Health Inventory Data. The R language and RStudio were used for all data wrangling, analyses, and visualizations. We researched previous work done to summarize the health, finances, and education levels of Americans. The Social Science Research Council has a comprehensive interactive map of the U.S. depicting a range of statistics regarding human wellbeing by state. Their map informed our approach to analysis and visualisation. Inspired by this resource, we decided presenting data on life expectancy, income, and education level by various demographics was crucial to summarizing quality of life in big cities. 

References

R Core Team (2017). R: A language and environment for statistical
computing. R Foundation for Statistical Computing,Vienna, Austria. URL
<a href="https://www.R-project.org/" class="uri">https://www.R-project.org/</a>.

RStudio Team (2015). RStudio: Integrated Development for R. RStudio,
Inc., Boston, MA URL
<a href="http://www.rstudio.com/" class="uri">http://www.rstudio.com/</a>.

Big Cities Health Data:
<a href="https://data.world/health/big-cities-health" class="uri">https://data.world/health/big-cities-health</a>

Measure of America Interactive Map:
<a href="https://measureofamerica.org/maps/?state%5Ehdi%5Eall_all%5EHDI%5Ehdi" class="uri">https://measureofamerica.org/maps/?state^hdi^all_all^HDI^hdi</a>

Measure of America Data Set:
<a href="http://measureofamerica.org/download-agreement/" class="uri">http://measureofamerica.org/download-agreement/</a>

First, our process involved finding a mean of the values for each city
and indicator. Each city has multiple years of data for each indicator
so obtaining a mean across these years is important for further
analysis. We then calculated the sum of mean values for a given
indicator across all cities. The city’s mean for each indicator was then
divided by the sum of means in the indicator, we’ll call this value the
city’s relative value. Since we have a relative value for every city in
each indicator we can calculate totals of the relative value for the
good and bad indicator categories. This is done by summing the relative
values for the good indicators and for the bad indicators. Then to get a
final ranking of the cities we subtracted the sum of the good indicators
from the sum of the bad indicators for each city. The result decides the
final ranking of the city. Finally, we plotted Livability by City.

After this, we made linear models to see if life expectancy and race and
life expectancy and city had statistically significant associations.

Subsequently, we produced an interactive faceted barplot depicting life
expectancy by city and race. Then, we made a series of static barplots
showing median household income, all-cause mortality rate, and percent
of adults who meet CDC recommended levels of physical activity by city
in order to visualize the relative health and income levels of the
cities. Finally, we produced three interesting barplots depicting
firearms-related mortality rate, homicide rate, and suicide rate. It was
surprising to find that Sacremento, CA has the highest suicide rate. It
was unsurprising to find that Detroit, MI had both the highest homicide
rate and the highest firearms-related mortality rate.

In conclusion, we found that the city that has the lowest quality of
health, education, and income was Detroit, Michigan and the city with
the best of these qualities was San Jose, California. One of the largest
challenges in this project was finding a way to correctly quantify
quality of life in a city. To get a reasonable result we had to try out
a few different iterations of the final method we came to and we feel
that this method is the most accurate way to rank these cities based on
the data we had.

Detroit, MI matched our hypothesis, while Boston, MA and Portland, OR
differed from it. One reason Boston, MA and Portland, OR were lower on
the list than hypothesized was because the data set incorporated a
limited number of predictors that do not fully encompass quality of
life. Also, we did not weigh the variables differently in our
calculation of “Livability”, when in reality some factors are more
important than others.

Import data set into workspace; define vectors of variables representing
variables unfit to be indicators of quality of life, and variables that
are positive indicators of quality of life

    library(ggplot2)
    library(dplyr)
    library(plotly)
    #setwd("C:/Users/robin/OneDrive/Documents")
    data <- read.csv("Big_Cities_Health_Data_Inventory.csv")


    unrelated = c("Percent Foreign Born", "Race/Ethnicity", "Opioid-Related Mortality Rate (Age-Adjusted; Per 100,000 people) *These data should not be compared across cities as they have different definitions", "Opioid-Related Mortality Rate (Age-Adjusted; Per 100,000 people) *These data should not be compared across cities as they have different definitions", "Opioid-Related Mortality Rate (Crude Rate; Per 100,000 people) *These data should not be compared across cities as they have different definitions", "Opioid-Related Mortality Rate (Age-Adjusted; Per 100,000 people) *These data should not be compared across cities as they have different definitions.", "Drug Abuse-Related Hospitalization Rate (Per 100,000 people) *Comparisons of these data are difficult as definitions can vary.", "HIV Diagnoses Rate (Per 100,000 people)", "AIDS Diagnoses Rate (Per 100,000 people)", "All-Cause Mortality Rate (Age-Adjusted; Per 100,000 people)", "Percent of Adults Who Are Obese", "Percent of Adults Who Binge Drank", "Percent of Adults Who Currently Smoke", "Percent of High School Students Who Binge Drank", "Percent of High School Students Who Currently Smoke", "Rate of Laboratory Confirmed Infections Caused by Shiga Toxin-Producing E-Coli (Per 100,000 people)", "Life Expectancy at Birth (Years)", "Percent of Adults Over Age 65 Who Received Pneumonia Vaccine", "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", "Rate of Laboratory Confirmed Infections Caused by Salmonella (Per 100,000 people)", "Percent of High School Students Who Are Obese", "Percent of High School Students Who Meet CDC-Recommended Physical Activity Levels", "Percent of Adults Who Received Seasonal Flu Shot", "Percent of Children Who Received Seasonal Flu Shot", "Life Expectancy at Birth (Years)", "Percent of Adults Over Age 65 Who Received Pneumonia Vaccine", "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", "Rate of Laboratory Confirmed Infections Caused by Salmonella (Per 100,000 people)", "Percent of High School Students Who Are Obese", "Percent of High School Students Who Meet CDC-Recommended Physical Activity Levels", "Percent of Adults Who Received Seasonal Flu Shot", "Percent of Children Who Received Seasonal Flu Shot", "Life Expectancy at Birth (Years)", "Percent of Adults Over Age 65 Who Received Pneumonia Vaccine", "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", "Rate of Laboratory Confirmed Infections Caused by Salmonella (Per 100,000 people)", "Percent of High School Students Who Are Obese", "Percent of High School Students Who Meet CDC-Recommended Physical Activity Levels", "Percent of Adults Who Received Seasonal Flu Shot", "Percent of Children Who Received Seasonal Flu Shot")

    good_indicators = c("Life Expectancy at Birth (Years)", "Median Household Income (Dollars)", "Percent 18+ High School Graduates", "Percent of Adults Over Age 65 Who Received Pneumonia Vaccine", "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", "Percent of High School Students Who Meet CDC-Recommended Physical Activity Levels", "Percent of Children Who Received Seasonal Flu Shot", "Percent of Adults Who Received Seasonal Flu Shot")

    places <- unique(data$Place)
    indicators <- unique(data$Indicator)

Create a variable “Mean\_by\_city\_and\_ind” equal to the mean of values
for a given place and indicator

    data <- data %>% 
      group_by(Indicator, Place) %>% 
      mutate(Mean_by_city_and_ind=mean(na.exclude(Value)))
    data <- ungroup(data)

Create variable “Sum” equal to the sum of mean values for a given
indicator across all places

    data <- data %>% 
      group_by(Indicator) %>% 
      mutate(Sum=sum(unique(Mean_by_city_and_ind)))
    data <- ungroup(data)

Create variable “Relative” equal to the quotient of the mean value for a
given indicator and place divided by the sum of all mean values for that
indicator across all places

    data <- data %>% 
      mutate(Relative=Mean_by_city_and_ind/Sum)

Create a variable “Good” equal to the sum of the “Relative” values
corresponding to good indicators for a given place

    data <- data %>% 
      group_by(Place) %>% 
      mutate(Good=sum(unique(Relative[Indicator %in% good_indicators])))

    data <- ungroup(data)

Create a variable “Bad” equal to the sum of the “Relative” values
corresponding to a bad indicators for a given place

    data <- data %>% 
      group_by(Place) %>% 
      mutate(Bad=sum(unique(Relative[!Indicator %in% good_indicators & !Indicator %in% unrelated])))
    data<-ungroup(data)

Create a variable “Livability” equal to Good minus Bad values

    data <- data %>%
      mutate(Livability = Good-Bad)

Center Livability around 0

    data$Livability <- scale(data$Livability)

Visualize livability by city

    data1 <- data %>%
      group_by(Place, Livability) %>% 
      summarize()
    data1 <- ungroup(data1)
    data1 <- data1[order(data1$Livability),]
    ggplot(data1)+aes(reorder(Place, Livability), Livability)+geom_point(stat="identity")+theme(axis.text.x=element_text(angle=90, hjust=1, vjust=0.5))+labs(x="City")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-9-1.png)

Create a variable representing mean life expectancy by race and city

    data2 <- data[data$Indicator == "Life Expectancy at Birth (Years)" & data$Race..Ethnicity != "All",]
    data2 <- data2 %>% 
      group_by(Race..Ethnicity, Place) %>% 
      mutate(Mean_Life_Exp_by_Race_and_Place=mean(na.exclude(Value))) 
    data2 <- ungroup(data2)

    life_exp.df<- summarise(group_by(data2, Race..Ethnicity, Place, Mean_Life_Exp_by_Race_and_Place))
    life_exp.df.ordered <- life_exp.df[order(life_exp.df$Mean_Life_Exp_by_Race_and_Place),]

Linear model of life expectancy and city

    summary(lm(Mean_Life_Exp_by_Race_and_Place ~ Place, life_exp.df))

    ## 
    ## Call:
    ## lm(formula = Mean_Life_Exp_by_Race_and_Place ~ Place, data = life_exp.df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -10.225  -3.031   1.000   3.775   9.125 
    ## 
    ## Coefficients:
    ##                                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                           82.5250     2.6428  31.227   <2e-16 ***
    ## PlaceCleveland, OH                    -3.2500     3.7375  -0.870    0.390    
    ## PlaceFort Worth (Tarrant County), TX  -7.1250     4.5774  -1.557    0.128    
    ## PlaceKansas City, MO                  -6.3750     4.5774  -1.393    0.172    
    ## PlaceLas Vegas (Clark County), NV     -0.9500     3.7375  -0.254    0.801    
    ## PlaceLong Beach, CA                   -1.5000     3.4118  -0.440    0.663    
    ## PlaceLos Angeles, CA                  -1.0250     3.7375  -0.274    0.785    
    ## PlaceNew York, NY                     -2.2917     4.0369  -0.568    0.574    
    ## PlaceOakland, CA                      -2.6917     4.0369  -0.667    0.509    
    ## PlacePortland (Multnomah County), OR  -2.2050     3.5457  -0.622    0.538    
    ## PlaceSan Antonio, TX                  -4.1583     4.0369  -1.030    0.309    
    ## PlaceSan Diego County, CA              0.1667     3.7375   0.045    0.965    
    ## PlaceSan Francisco, CA                -3.1000     3.7375  -0.829    0.412    
    ## PlaceSeattle, WA                       1.1083     4.0369   0.275    0.785    
    ## PlaceWashington, DC                   -0.7583     4.0369  -0.188    0.852    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.286 on 39 degrees of freedom
    ## Multiple R-squared:  0.1523, Adjusted R-squared:  -0.152 
    ## F-statistic: 0.5006 on 14 and 39 DF,  p-value: 0.9185

> The results show that life expectancy is not correlated with city in a
> statistically significant way.

Linear model for mean life expectancy and race

    summary(lm(Mean_Life_Exp_by_Race_and_Place ~ Race..Ethnicity, life_exp.df))

    ## 
    ## Call:
    ## lm(formula = Mean_Life_Exp_by_Race_and_Place ~ Race..Ethnicity, 
    ##     data = life_exp.df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -6.4956 -1.2864  0.0022  1.2381  6.8410 
    ## 
    ## Coefficients:
    ##                                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                      76.400      2.535  30.139  < 2e-16 ***
    ## Race..EthnicityAsian/PI          10.071      2.689   3.746 0.000491 ***
    ## Race..EthnicityBlack             -1.323      2.618  -0.505 0.615595    
    ## Race..EthnicityHispanic           8.259      2.631   3.140 0.002923 ** 
    ## Race..EthnicityMultiracial        3.100      3.585   0.865 0.391577    
    ## Race..EthnicityNative American    1.100      3.585   0.307 0.760320    
    ## Race..EthnicityWhite              3.496      2.618   1.335 0.188251    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.535 on 47 degrees of freedom
    ## Multiple R-squared:  0.765,  Adjusted R-squared:  0.735 
    ## F-statistic: 25.51 on 6 and 47 DF,  p-value: 3.101e-13

> The linear model shows that Asian and Hispanic races or ethnicities
> are have a statistically significant association with life expectancy.
> The resulting model is: life expectancy = 76.4 + 10.071 x Asian +
> 8.259 x Hispanic

Visualize mean life expectancy by race and city

    gg  <- ggplot(life_exp.df.ordered) + aes(x=reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place) ,y=Mean_Life_Exp_by_Race_and_Place, fill = Race..Ethnicity) +
      geom_bar(position = "dodge", stat="identity") + facet_wrap(~Place) + labs(title = "Life Expectancy by Race and City") + theme(axis.text.x=element_text(angle=90, hjust=1, vjust=0.5))

    ggp_build <- plotly_build(gg)
    ggp_build$layout$height = 1000
    ggp_build$layout$width = 1000

    ggp_build

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-e563461f765ad2ebf724">{"x":{"data":[{"orientation":"v","width":0.9,"base":0,"x":[2],"y":[76.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): American Indian/Alaska Native<br />Mean_Life_Exp_by_Race_and_Place: 76.40000<br />Race..Ethnicity: American Indian/Alaska Native","type":"bar","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"American Indian/Alaska Native","legendgroup":"American Indian/Alaska Native","showlegend":true,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[87.2],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 87.20000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[88.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 88.40000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[86.8],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 86.80000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[84.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 84.50000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[85.8],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 85.80000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x3","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[86.2666666666667],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 86.26667<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x2","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[87.7],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 87.70000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x4","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[7],"y":[85.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Asian/PI<br />Mean_Life_Exp_by_Race_and_Place: 85.10000<br />Race..Ethnicity: Asian/PI","type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Asian/PI","legendgroup":"Asian/PI","showlegend":false,"xaxis":"x","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[77],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 77.00000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[72.8],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 72.80000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[74.3],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 74.30000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x3","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[73.7],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 73.70000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x4","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[76.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 76.10000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[78.95],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 78.95000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[75.7],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 75.70000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x3","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[77.2],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 77.20000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x4","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[74.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 74.50000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[74.2],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 74.20000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x2","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[75.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 75.90000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x3","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[76.7],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 76.70000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x4","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[69.2],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 69.20000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[77.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 77.10000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x2","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[1],"y":[72.8],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Black<br />Mean_Life_Exp_by_Race_and_Place: 72.80000<br />Race..Ethnicity: Black","type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black","legendgroup":"Black","showlegend":false,"xaxis":"x3","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[86.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 86.40000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[82.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 82.50000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[85.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 85.90000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[84.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 84.90000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[83.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 83.40000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x3","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[82.2],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 82.20000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x4","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[83.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 83.90000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[84.4666666666667],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 84.46667<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x2","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[79.3],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 79.30000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x3","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[85.3],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 85.30000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x4","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[82.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 82.40000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[91.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 91.50000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x2","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[6],"y":[88.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Hispanic<br />Mean_Life_Exp_by_Race_and_Place: 88.40000<br />Race..Ethnicity: Hispanic","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Hispanic","legendgroup":"Hispanic","showlegend":false,"xaxis":"x3","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[4],"y":[79.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Multiracial<br />Mean_Life_Exp_by_Race_and_Place: 79.50000<br />Race..Ethnicity: Multiracial","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,182,235,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Multiracial","legendgroup":"Multiracial","showlegend":true,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[3],"y":[77.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): Native American<br />Mean_Life_Exp_by_Race_and_Place: 77.50000<br />Race..Ethnicity: Native American","type":"bar","marker":{"autocolorscale":false,"color":"rgba(165,138,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Native American","legendgroup":"Native American","showlegend":true,"xaxis":"x2","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[79.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 79.50000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[73.4],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 73.40000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[76.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 76.50000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x3","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[78.6],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 78.60000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x4","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[77.5],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 77.50000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.90000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.10000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x3","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81.3],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.30000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x4","yaxis":"y2","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.10000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[79.1666666666667],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 79.16667<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x2","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[79.9],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 79.90000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x3","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81.0666666666667],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.06667<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x4","yaxis":"y3","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[81],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 81.00000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[82.3],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 82.30000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x2","yaxis":"y4","hoverinfo":"text","frame":null},{"orientation":"v","width":0.9,"base":0,"x":[5],"y":[84.1],"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place): White<br />Mean_Life_Exp_by_Race_and_Place: 84.10000<br />Race..Ethnicity: White","type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"White","legendgroup":"White","showlegend":false,"xaxis":"x3","yaxis":"y4","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":55.4520547945205,"r":7.30593607305936,"b":197.990867579909,"l":37.2602739726027},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Life Expectancy by Race and City","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,0.241846053489889],"automargin":true,"type":"linear","autorange":false,"range":[0.4,7.6],"tickmode":"array","ticktext":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"tickvals":[1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-90,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y4","title":"","hoverformat":".2f"},"annotations":[{"text":"reorder(Race..Ethnicity, Mean_Life_Exp_by_Race_and_Place)","x":0.5,"y":-0.281963470319635,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"top","annotationType":"axis"},{"text":"Mean_Life_Exp_by_Race_and_Place","x":-0.0252772341813438,"y":0.5,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-90,"xanchor":"right","yanchor":"center","annotationType":"axis"},{"text":"Boston, MA","x":0.120923026744945,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Cleveland, OH","x":0.375,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Fort Worth (Tarrant County), TX","x":0.625,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Kansas City, MO","x":0.879076973255055,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Las Vegas (Clark County), NV","x":0.120923026744945,"y":0.720319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Long Beach, CA","x":0.375,"y":0.720319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Los Angeles, CA","x":0.625,"y":0.720319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"New York, NY","x":0.879076973255055,"y":0.720319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Oakland, CA","x":0.120923026744945,"y":0.470319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Portland (Multnomah County), OR","x":0.375,"y":0.470319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"San Antonio, TX","x":0.625,"y":0.470319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"San Diego County, CA","x":0.879076973255055,"y":0.470319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"San Francisco, CA","x":0.120923026744945,"y":0.220319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Seattle, WA","x":0.375,"y":0.220319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Washington, DC","x":0.625,"y":0.220319634703196,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":11.689497716895},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Race..Ethnicity","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"yaxis":{"domain":[0.779680365296804,1],"automargin":true,"type":"linear","autorange":false,"range":[-4.575,96.075],"tickmode":"array","ticktext":["0","25","50","75"],"tickvals":[0,25,50,75],"categoryorder":"array","categoryarray":["0","25","50","75"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0.779680365296804,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0.779680365296804,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0.779680365296804,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0.779680365296804,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0,"y1":23.37899543379,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0.529680365296804,"y1":0.720319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.720319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0.529680365296804,"y1":0.720319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.720319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0.529680365296804,"y1":0.720319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.720319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0.529680365296804,"y1":0.720319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0,"y1":23.37899543379,"yanchor":0.720319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0.279680365296804,"y1":0.470319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.470319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0.279680365296804,"y1":0.470319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.470319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0.279680365296804,"y1":0.470319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.470319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0.279680365296804,"y1":0.470319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.758153946510111,"x1":1,"y0":0,"y1":23.37899543379,"yanchor":0.470319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0,"y1":0.220319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.241846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.220319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0,"y1":0.220319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.258153946510111,"x1":0.491846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.220319634703196,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0,"y1":0.220319634703196},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"transparent","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.508153946510111,"x1":0.741846053489889,"y0":0,"y1":23.37899543379,"yanchor":0.220319634703196,"ysizemode":"pixel"}],"xaxis2":{"type":"linear","autorange":false,"range":[0.4,7.6],"tickmode":"array","ticktext":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"tickvals":[1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-90,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.258153946510111,0.491846053489889],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y4","title":"","hoverformat":".2f"},"xaxis3":{"type":"linear","autorange":false,"range":[0.4,7.6],"tickmode":"array","ticktext":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"tickvals":[1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-90,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.508153946510111,0.741846053489889],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y4","title":"","hoverformat":".2f"},"xaxis4":{"type":"linear","autorange":false,"range":[0.4,7.6],"tickmode":"array","ticktext":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"tickvals":[1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["Black","American Indian/Alaska Native","Native American","Multiracial","White","Hispanic","Asian/PI"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-90,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.758153946510111,1],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y3","title":"","hoverformat":".2f"},"yaxis2":{"type":"linear","autorange":false,"range":[-4.575,96.075],"tickmode":"array","ticktext":["0","25","50","75"],"tickvals":[0,25,50,75],"categoryorder":"array","categoryarray":["0","25","50","75"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.529680365296804,0.720319634703196],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"yaxis3":{"type":"linear","autorange":false,"range":[-4.575,96.075],"tickmode":"array","ticktext":["0","25","50","75"],"tickvals":[0,25,50,75],"categoryorder":"array","categoryarray":["0","25","50","75"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0.279680365296804,0.470319634703196],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"yaxis4":{"type":"linear","autorange":false,"range":[-4.575,96.075],"tickmode":"array","ticktext":["0","25","50","75"],"tickvals":[0,25,50,75],"categoryorder":"array","categoryarray":["0","25","50","75"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0,0.220319634703196],"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"31307d0053b5":{"x":{},"y":{},"fill":{},"type":"bar"}},"cur_data":"31307d0053b5","visdat":{"31307d0053b5":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->

> Above is an interactive plot showing the mean life expectancy for
> cities by race.

Visualize median household income by city

    income <- subset(data, Indicator == "Median Household Income (Dollars)", select = colnames(data))
    income_val <- aggregate(Value ~ Place, income, mean)
    ggplot(income_val, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "Median Household Income (Dollars)", x="City")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-14-1.png)

> San Jose, CA has the highest Median Household Income, while Detroit,
> MI has the lowest.

Visualize Mortality Rate by city and race

    all_cause <- subset(data, Indicator == "All-Cause Mortality Rate (Age-Adjusted; Per 100,000 people)", select = colnames(data))
    all_cause2 <- aggregate( Value ~ Place, all_cause, mean )
    ggplot(all_cause2, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "All-Cause Mortality Rate (Age-Adjusted; Per 100,000 people)", x="City")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-15-1.png) &gt;
Cleveland, OH has the lowest All-Cause Mortality Rate, while Phoenix, AZ
has the lowest.

Visualize percent of adults who meet Physical Activity Requirements

    pa <- subset(data, Indicator == "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", select = colnames(data))
    pa_data <- aggregate( Value ~ Place, pa, mean)
    ggplot(pa_data, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "Percent of Adults Who Meet CDC-Recommended Physical Activity Levels", x="City")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-16-1.png) &gt;
Denver, CO has the highest percentage of adults who meet CDC-recommend
physical activity levels, while Detroit, MI has the lowest.

Interesting plots

    fa <- subset(data, Indicator == "Firearm Related Mortality Rate (Age-Adjusted; Per 100,000 people)", select = colnames(data))
    fa_data <- aggregate( Value ~ Place, fa, mean)
    ggplot(fa_data, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "Firearm Related Mortality Rate (Age-Adjusted; Per 100,000 people)")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-17-1.png) &gt;
Detroit, MI has the highest firearm-related mortality rate, while New
York, NY has the lowest.

    hr <- subset(data, Indicator == "Homicide Rate (Age-Adjusted; Per 100,000 people)", select = colnames(data))
    hr_data <- aggregate( Value ~ Place, hr, mean)
    ggplot(hr_data, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "Homicide Rate (Age-Adjusted; Per 100,000 people)", x="City")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-18-1.png) &gt;
Detroit, MI has the highest homicide rate, while San Diego, CA has the
lowest

    sr <- subset(data, Indicator == "Suicide Rate  (Age-Adjusted; Per 100,000 people)", select = colnames(data))
    sr_data <- aggregate( Value ~ Place, sr, mean)
    ggplot(sr_data, aes(x=reorder(Place, Value), y=Value)) + geom_col() + coord_flip() + labs(title = "Suicide Rate  (Age-Adjusted; Per 100,000 people)")

![](finalproj_files/figure-markdown_strict/unnamed-chunk-19-1.png) &gt;
Sacramento, CA has the highest suicide rate, while Los Angeles, CA has
the lowest.
