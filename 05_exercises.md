---
title: 'Weekly Exercises #5'
author: "Ben Wagner"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
veggie_harvest_graph <-garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(variety2 = fct_reorder(variety, date, min)) %>% 
  group_by(variety2) %>% 
  mutate(weight_lbs= weight*0.00220462)%>% 
  summarize(total_harvest= sum(weight_lbs)) %>%   
  ggplot()+
  geom_col(aes(x=total_harvest, y=variety2, fill = variety2))+
  labs(x="", 
       y="", 
       title = "Total harvest weights in pounds by variety(oldest harvest to most recent)")
ggplotly(veggie_harvest_graph)
```

<!--html_preserve--><div id="htmlwidget-42422329aecc03aec96b" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-42422329aecc03aec96b">{"x":{"data":[{"orientation":"v","width":32.39468628,"base":0.55,"x":[16.19734314],"y":[0.9],"text":"total_harvest: 32.39469<br />variety2: grape<br />variety2: grape","type":"bar","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"grape","legendgroup":"grape","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":24.99377694,"base":1.55,"x":[12.49688847],"y":[0.9],"text":"total_harvest: 24.99378<br />variety2: Big Beef<br />variety2: Big Beef","type":"bar","marker":{"autocolorscale":false,"color":"rgba(222,140,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Big Beef","legendgroup":"Big Beef","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":24.9232291,"base":2.55,"x":[12.46161455],"y":[0.9],"text":"total_harvest: 24.92323<br />variety2: Bonny Best<br />variety2: Bonny Best","type":"bar","marker":{"autocolorscale":false,"color":"rgba(183,159,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Bonny Best","legendgroup":"Bonny Best","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":34.00846812,"base":3.55,"x":[17.00423406],"y":[0.9],"text":"total_harvest: 34.00847<br />variety2: Better Boy<br />variety2: Better Boy","type":"bar","marker":{"autocolorscale":false,"color":"rgba(124,174,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Better Boy","legendgroup":"Better Boy","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":15.71232674,"base":4.55,"x":[7.85616337],"y":[0.9],"text":"total_harvest: 15.71233<br />variety2: Cherokee Purple<br />variety2: Cherokee Purple","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,186,56,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Cherokee Purple","legendgroup":"Cherokee Purple","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":65.67342518,"base":5.55,"x":[32.83671259],"y":[0.9],"text":"total_harvest: 65.67343<br />variety2: Amish Paste<br />variety2: Amish Paste","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,139,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Amish Paste","legendgroup":"Amish Paste","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":26.32536742,"base":6.55,"x":[13.16268371],"y":[0.9],"text":"total_harvest: 26.32537<br />variety2: Mortgage Lifter<br />variety2: Mortgage Lifter","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Mortgage Lifter","legendgroup":"Mortgage Lifter","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":15.0244853,"base":7.55,"x":[7.51224265],"y":[0.899999999999999],"text":"total_harvest: 15.02449<br />variety2: Jet Star<br />variety2: Jet Star","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,180,240,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Jet Star","legendgroup":"Jet Star","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":26.71778978,"base":8.55,"x":[13.35889489],"y":[0.899999999999999],"text":"total_harvest: 26.71779<br />variety2: Old German<br />variety2: Old German","type":"bar","marker":{"autocolorscale":false,"color":"rgba(97,156,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Old German","legendgroup":"Old German","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":15.8071254,"base":9.55,"x":[7.9035627],"y":[0.899999999999999],"text":"total_harvest: 15.80713<br />variety2: Black Krim<br />variety2: Black Krim","type":"bar","marker":{"autocolorscale":false,"color":"rgba(199,124,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Black Krim","legendgroup":"Black Krim","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":15.64618814,"base":10.55,"x":[7.82309407],"y":[0.899999999999999],"text":"total_harvest: 15.64619<br />variety2: Brandywine<br />variety2: Brandywine","type":"bar","marker":{"autocolorscale":false,"color":"rgba(245,100,227,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Brandywine","legendgroup":"Brandywine","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":51.61235882,"base":11.55,"x":[25.80617941],"y":[0.899999999999999],"text":"total_harvest: 51.61236<br />variety2: volunteers<br />variety2: volunteers","type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,100,176,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"volunteers","legendgroup":"volunteers","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":98.6301369863014},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total harvest weights in pounds by variety(oldest harvest to most recent)","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-3.283671259,68.957096439],"tickmode":"array","ticktext":["0","20","40","60"],"tickvals":[0,20,40,60],"categoryorder":"array","categoryarray":["0","20","40","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,12.6],"tickmode":"array","ticktext":["grape","Big Beef","Bonny Best","Better Boy","Cherokee Purple","Amish Paste","Mortgage Lifter","Jet Star","Old German","Black Krim","Brandywine","volunteers"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12],"categoryorder":"array","categoryarray":["grape","Big Beef","Bonny Best","Better Boy","Cherokee Purple","Amish Paste","Mortgage Lifter","Jet Star","Old German","Black Krim","Brandywine","volunteers"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"variety2","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"5dcc35a222b7":{"x":{},"y":{},"fill":{},"type":"bar"}},"cur_data":"5dcc35a222b7","visdat":{"5dcc35a222b7":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
Cum_veggie_weight_graph <-garden_harvest%>% 
  filter(vegetable== "beets") %>%
  group_by(date, variety) %>% 
  summarize(total_weight= sum(weight)) %>% 
  mutate(weight_lbs=total_weight*0.00220462,
         cum_wt_lbs=cumsum(weight_lbs)) %>% 
  ggplot(aes(x=date, y=cum_wt_lbs, color=variety))+
  geom_line()+
  labs(title = "Cumulative weight harvested in pounds by day for each variety of beet")
ggplotly(Cum_veggie_weight_graph)
```

<!--html_preserve--><div id="htmlwidget-c5d7b450aa4329ef58d2" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c5d7b450aa4329ef58d2">{"x":{"data":[{"x":[18450,18451,18463,18470,18487],"y":[0.13668644,0.18298346,0.23589434,0.32848838,6.13766208],"text":["date: 2020-07-07<br />cum_wt_lbs:  0.13668644<br />variety: Gourmet Golden","date: 2020-07-08<br />cum_wt_lbs:  0.18298346<br />variety: Gourmet Golden","date: 2020-07-20<br />cum_wt_lbs:  0.23589434<br />variety: Gourmet Golden","date: 2020-07-27<br />cum_wt_lbs:  0.32848838<br />variety: Gourmet Golden","date: 2020-08-13<br />cum_wt_lbs:  6.13766208<br />variety: Gourmet Golden"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Gourmet Golden","legendgroup":"Gourmet Golden","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434],"y":[0.01763696,0.0551155,0.02425082,0.12566334],"text":["date: 2020-06-11<br />cum_wt_lbs:  0.01763696<br />variety: leaves","date: 2020-06-18<br />cum_wt_lbs:  0.05511550<br />variety: leaves","date: 2020-06-19<br />cum_wt_lbs:  0.02425082<br />variety: leaves","date: 2020-06-21<br />cum_wt_lbs:  0.12566334<br />variety: leaves"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","name":"leaves","legendgroup":"leaves","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18450,18452,18455,18461,18470,18473,18487],"y":[0.15873264,0.15211878,0.19621118,0.37919464,0.43651476,0.22266662,11.44418242],"text":["date: 2020-07-07<br />cum_wt_lbs:  0.15873264<br />variety: Sweet Merlin","date: 2020-07-09<br />cum_wt_lbs:  0.15211878<br />variety: Sweet Merlin","date: 2020-07-12<br />cum_wt_lbs:  0.19621118<br />variety: Sweet Merlin","date: 2020-07-18<br />cum_wt_lbs:  0.37919464<br />variety: Sweet Merlin","date: 2020-07-27<br />cum_wt_lbs:  0.43651476<br />variety: Sweet Merlin","date: 2020-07-30<br />cum_wt_lbs:  0.22266662<br />variety: Sweet Merlin","date: 2020-08-13<br />cum_wt_lbs: 11.44418242<br />variety: Sweet Merlin"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","name":"Sweet Merlin","legendgroup":"Sweet Merlin","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":37.2602739726027},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Cumulative weight harvested in pounds by day for each variety of beet","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18420.85,18490.15],"tickmode":"array","ticktext":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"tickvals":[18428,18444,18458,18475,18489],"categoryorder":"array","categoryarray":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.553690313,12.015509693],"tickmode":"array","ticktext":["0","3","6","9","12"],"tickvals":[0,3,6,9,12],"categoryorder":"array","categoryarray":["0","3","6","9","12"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"cum_wt_lbs","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"variety","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"5dcc51492b1d":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"5dcc51492b1d","visdat":{"5dcc51492b1d":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>% 
  filter(year=="2017",
         departure_station== "PARIS EST") %>%
  group_by(departure_station, month) %>% 
  mutate(n= n()) %>% 
  summarize(avg_delay_departing = max(cumsum(avg_delay_all_departing)/n)) %>%
  ggplot(aes(x=month,
             y= avg_delay_departing))+
  geom_point()+
  labs(title= "Average Total Departure Delay(minutes) from Paris Est during 2017",
       subtitle = "Month: {frame_time}",
       x="",
       y="")+
  scale_x_continuous(breaks = seq(0,12,1))+
  theme(legend.position = "side",
        legend.title = element_blank())+
  transition_time(month)+
   exit_fade()
```

```r
anim_save("delay.gif")
```

```
## Error: The animation object does not specify a save_animation method
```

```r
knitr::include_graphics("delay.gif")
```

![](delay.gif)<!-- -->



## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>% 
  group_by(variety) %>% 
  mutate(cum_harvest= cumsum(daily_harvest_lb)) %>% 
  ggplot()+
  geom_area(aes(x= date, 
                y= cum_harvest, 
                fill = fct_reorder2(variety, variety, cum_harvest, .desc= FALSE)))+
  labs(title = "Cumulative Tomatoe Harvest Weight in lbs. by variety",
       x="",
       y="",
       fill= "Variety")+
  transition_reveal(date)
```

![](05_exercises_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("Harvest_weight.gif")
```

```r
knitr::include_graphics("Harvest_weight.gif")
```

![](Harvest_weight.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.28, bottom = 39.41, right = 3.03, top = 39.8), 
    maptype = "terrain",
    zoom = 11)

mallorca_gganim <- ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele),
             size = .5) +
  geom_point(data = mallorca_bike_day7,
             aes(x=lon, y=lat),
             color="red",
             size= 4)+
  labs(title= "Mallorca Bike Ride Path",
       subtitle = "Time: {frame_along}",
       x="",
       y="")+
  scale_color_viridis_c(option = "magma") +
  theme_map() +
  theme(legend.background = element_blank())+
  transition_reveal(time)

animate(mallorca_gganim, duration = 20)
```

![](05_exercises_files/figure-html/unnamed-chunk-9-1.gif)<!-- -->

```r
anim_save("mallorca_bike_ride.gif")
```

```r
knitr::include_graphics("mallorca_bike_ride.gif")
```

![](mallorca_bike_ride.gif)<!-- -->
I like this map rather than the static one because you can see the path as you progress through the times rather than seeing the path all at once. Making the duration longer also helped visualize the path taken.


  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_map <- get_stamenmap(
    bbox = c(left = -79.6, bottom = 8.9, right = -79.45, top = 9.0), 
    maptype = "terrain",
    zoom = 14)

ironman<- bind_rows(panama_bike, panama_run, panama_swim)

  ggmap(panama_map) +
  geom_path(data = ironman, 
             aes(x = lon, y = lat, color = event),
             size = 1) +
  geom_point(data = ironman,
             aes(x=lon, y=lat, color= event),
             size= 4)+
  labs(title= "Heather's Ironman Path",
       subtitle = "Time: {frame_along}",
       x="",
       y="",
       color= "Event")+
  theme_map() +
  theme(legend.background = element_blank())+
  transition_reveal(time)
```

![](05_exercises_files/figure-html/unnamed-chunk-12-1.gif)<!-- -->

```r
anim_save("Heather_ironman.gif")
```

```r
knitr::include_graphics("Heather_ironman.gif")
```

![](Heather_ironman.gif)<!-- -->

  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
covid_anim <- covid19 %>% 
  group_by(state) %>% 
  mutate(one_day_lag= lag(cases, n=1),
         seven_day_lag= lag(cases, n=1)) %>% 
  replace_na(list(one_day_lag=0, seven_day_lag=0)) %>% 
  mutate(new_cases_1day= cases-one_day_lag) %>%
  mutate(new_cases_7day=cases-seven_day_lag) %>% 
  filter(cases>20) %>% 
  ggplot()+
  geom_path(aes(x=cases,
                y= new_cases_7day,
                group = state))+
  geom_point(aes(x=cases,
                 y= new_cases_7day,
                 group = state))+
  geom_text(aes(x=cases,
                y= new_cases_7day,
                group= state,
                label = state),
                check_overlap= TRUE)+
  scale_y_log10(labels= scales::comma)+
  scale_x_log10(labels= scales::comma)+
  labs(title = "New 7 day covid cases vs. total cases in each state",
       x="",
       y="",
       subtitle = "Date: {frame_along}")+
  transition_reveal(date)

animate(covid_anim, nframes=200, duration = 30)
```

![](05_exercises_files/figure-html/unnamed-chunk-15-1.gif)<!-- -->

```r
anim_save("new_covid_cases.gif")
```

```r
knitr::include_graphics("new_covid_cases.gif")
```

![](new_covid_cases.gif)<!-- -->

I can see from the animation, when the pandemic initially struck, both cumulative cases increased and new cases also increased because the number of cases was increasing exponentially. However, the number of new cases each 7 days began to slow down because the government began regulating human contact with social distancing.

  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  



```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
cases_anim <- covid19 %>%
  mutate(state_name = str_to_lower(state)) %>% 
  arrange(desc(date)) %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% 
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000,
         day_of_week = wday(date, label = TRUE)) %>%
  filter(day_of_week == "Fri") %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = cases_per_10000,
               group=date)) +
  #This assures the map looks decently nice:
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  labs(title = "Most recent number of cases per 10,000 people in each state by color",
       fill = "Cases Per 10k",
       subtitle = "Date: {frame_time}")+
  transition_time(date)

animate(cases_anim, nframes= 200, end_pause=10)
```

![](05_exercises_files/figure-html/unnamed-chunk-18-1.gif)<!-- -->

```r
anim_save("Recent_covid_cases.gif")
```

```r
knitr::include_graphics("Recent_covid_cases.gif")
```

![](Recent_covid_cases.gif)<!-- -->

You can see from the animation that states in the middle of the country tended to change color quicker because their overall population is a lot less than the Northwest or Sothern states. However, those states soon came to follow and they even got lighter meaning the number of covid cases in those states rose quickly. 


## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
