---
title: 'Weekly Exercises #3'
author: "Caleb Williams"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  select(vegetable, date, weight) %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  group_by(vegetable, day) %>% 
  summarize(total_weight_day = sum(weight)) %>% 
  pivot_wider(id_col = vegetable,
              names_from = day,
              values_from = total_weight_day)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"156","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"20","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"186","3":"30","4":"50","5":"12","6":"212","7":"NA","8":"NA"},{"1":"beans","2":"2136","3":"2952","4":"1990","5":"1539","6":"692","7":"868","8":"1852"},{"1":"beets","2":"172","3":"305","4":"72","5":"5394","6":"11","7":"146","8":"83"},{"1":"broccoli","2":"NA","3":"372","4":"NA","5":"NA","6":"75","7":"571","8":"321"},{"1":"carrots","2":"1057","3":"395","4":"160","5":"1213","6":"970","7":"1332","8":"2523"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"8"},{"1":"cilantro","2":"17","3":"NA","4":"2","5":"NA","6":"33","7":"NA","8":"NA"},{"1":"corn","2":"597","3":"344","4":"330","5":"NA","6":"1564","7":"661","8":"2405"},{"1":"cucumbers","2":"4373","3":"2166","4":"4557","5":"1500","6":"3370","7":"1408","8":"2407"},{"1":"edamame","2":"2127","3":"NA","4":"636","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"571","4":"64","5":"NA","6":"NA","7":"NA","8":"31"},{"1":"jalapeño","2":"684","3":"2519","4":"249","5":"102","6":"587","7":"119","8":"218"},{"1":"kale","2":"676","3":"938","4":"128","5":"127","6":"173","7":"375","8":"280"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"191","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"597","3":"1115","4":"416","5":"1112","6":"817","7":"665","8":"538"},{"1":"onions","2":"868","3":"231","4":"321","5":"273","6":"33","7":"118","8":"NA"},{"1":"peas","2":"1294","3":"2102","4":"938","5":"1541","6":"425","7":"933","8":"490"},{"1":"peppers","2":"627","3":"1146","4":"655","5":"322","6":"152","7":"228","8":"1108"},{"1":"potatoes","2":"1271","3":"440","4":"NA","5":"5376","6":"1697","7":"NA","8":"2073"},{"1":"pumpkins","2":"42043","3":"13662","4":"14450","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"105","3":"89","4":"43","5":"67","6":"88","7":"37","8":"NA"},{"1":"raspberries","2":"242","3":"59","4":"152","5":"131","6":"259","7":"NA","8":"NA"},{"1":"rutabaga","2":"3129","3":"NA","4":"NA","5":"NA","6":"1623","7":"8738","8":"NA"},{"1":"spinach","2":"118","3":"67","4":"225","5":"106","6":"89","7":"221","8":"97"},{"1":"squash","2":"25502","3":"11038","4":"8377","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"77","3":"217","4":"NA","5":"40","6":"221","7":"37","8":"NA"},{"1":"Swiss chard","2":"333","3":"487","4":"32","5":"1012","6":"280","7":"566","8":"412"},{"1":"tomatoes","2":"15933","3":"5213","4":"22113","5":"15657","6":"38590","7":"34296","8":"26429"},{"1":"zucchini","2":"1549","3":"5532","4":"7470","5":"15708","6":"8492","7":"5550","8":"926"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?

I began by simply summarizing the original "garden_harvest" data set to calculate the total harvest in lbs for each vegetable variety. I renamed this garden_harvest2. I then selected only the essential columns from the garden_planting data set and named it "selected_garden_planting", I then did a full join to by vegetable and variety to add plot to the original garden_harvest2. Problems may arise if varieties were planted in multiple plots.When this happens, the harvest value may be attributed to multiple different plots, instead of the proportionate value from each. 


```r
garden_harvest2 <- garden_harvest %>% 
  group_by(variety, vegetable) %>% 
  summarize(total_weight = sum(weight)) %>% 
  mutate(total_weight_lbs = total_weight*0.00220462)

selected_garden_planting <- garden_planting %>% 
  select(vegetable, variety, plot)

garden_harvest2 %>% 
  full_join(selected_garden_planting,
            by = c("vegetable", "variety"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_weight"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["total_weight_lbs"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[5],"type":["chr"],"align":["left"]}],"data":[{"1":"Amish Paste","2":"tomatoes","3":"29789","4":"65.67342518","5":"J"},{"1":"Amish Paste","2":"tomatoes","3":"29789","4":"65.67342518","5":"N"},{"1":"asparagus","2":"asparagus","3":"20","4":"0.04409240","5":"NA"},{"1":"Better Boy","2":"tomatoes","3":"15426","4":"34.00846812","5":"J"},{"1":"Better Boy","2":"tomatoes","3":"15426","4":"34.00846812","5":"N"},{"1":"Big Beef","2":"tomatoes","3":"11337","4":"24.99377694","5":"N"},{"1":"Black Krim","2":"tomatoes","3":"7170","4":"15.80712540","5":"N"},{"1":"Blue (saved)","2":"squash","3":"18835","4":"41.52401770","5":"A"},{"1":"Blue (saved)","2":"squash","3":"18835","4":"41.52401770","5":"B"},{"1":"Bolero","2":"carrots","3":"3761","4":"8.29157582","5":"H"},{"1":"Bolero","2":"carrots","3":"3761","4":"8.29157582","5":"L"},{"1":"Bonny Best","2":"tomatoes","3":"11305","4":"24.92322910","5":"J"},{"1":"Brandywine","2":"tomatoes","3":"7097","4":"15.64618814","5":"J"},{"1":"Bush Bush Slender","2":"beans","3":"10038","4":"22.12997556","5":"M"},{"1":"Bush Bush Slender","2":"beans","3":"10038","4":"22.12997556","5":"D"},{"1":"Catalina","2":"spinach","3":"923","4":"2.03486426","5":"H"},{"1":"Catalina","2":"spinach","3":"923","4":"2.03486426","5":"E"},{"1":"Cherokee Purple","2":"tomatoes","3":"7127","4":"15.71232674","5":"J"},{"1":"Chinese Red Noodle","2":"beans","3":"356","4":"0.78484472","5":"K"},{"1":"Chinese Red Noodle","2":"beans","3":"356","4":"0.78484472","5":"L"},{"1":"cilantro","2":"cilantro","3":"52","4":"0.11464024","5":"potD"},{"1":"cilantro","2":"cilantro","3":"52","4":"0.11464024","5":"E"},{"1":"Cinderella's Carraige","2":"pumpkins","3":"14911","4":"32.87308882","5":"B"},{"1":"Classic Slenderette","2":"beans","3":"1635","4":"3.60455370","5":"E"},{"1":"Crispy Colors Duo","2":"kohlrabi","3":"191","4":"0.42108242","5":"front"},{"1":"delicata","2":"squash","3":"4762","4":"10.49840044","5":"K"},{"1":"Delicious Duo","2":"onions","3":"342","4":"0.75398004","5":"P"},{"1":"Dorinny Sweet","2":"corn","3":"5174","4":"11.40670388","5":"A"},{"1":"Dragon","2":"carrots","3":"1862","4":"4.10500244","5":"H"},{"1":"Dragon","2":"carrots","3":"1862","4":"4.10500244","5":"L"},{"1":"edamame","2":"edamame","3":"2763","4":"6.09136506","5":"O"},{"1":"Farmer's Market Blend","2":"lettuce","3":"1725","4":"3.80296950","5":"C"},{"1":"Farmer's Market Blend","2":"lettuce","3":"1725","4":"3.80296950","5":"L"},{"1":"Garden Party Mix","2":"radish","3":"429","4":"0.94578198","5":"C"},{"1":"Garden Party Mix","2":"radish","3":"429","4":"0.94578198","5":"G"},{"1":"Garden Party Mix","2":"radish","3":"429","4":"0.94578198","5":"H"},{"1":"giant","2":"jalapeño","3":"4478","4":"9.87228836","5":"L"},{"1":"Golden Bantam","2":"corn","3":"727","4":"1.60275874","5":"B"},{"1":"Gourmet Golden","2":"beets","3":"3185","4":"7.02171470","5":"H"},{"1":"grape","2":"tomatoes","3":"14694","4":"32.39468628","5":"O"},{"1":"green","2":"peppers","3":"2582","4":"5.69232884","5":"K"},{"1":"green","2":"peppers","3":"2582","4":"5.69232884","5":"O"},{"1":"greens","2":"carrots","3":"169","4":"0.37258078","5":"NA"},{"1":"Heirloom Lacinto","2":"kale","3":"2697","4":"5.94586014","5":"P"},{"1":"Heirloom Lacinto","2":"kale","3":"2697","4":"5.94586014","5":"front"},{"1":"Improved Helenor","2":"rutabaga","3":"13490","4":"29.74032380","5":"NA"},{"1":"Isle of Naxos","2":"basil","3":"490","4":"1.08026380","5":"potB"},{"1":"Jet Star","2":"tomatoes","3":"6815","4":"15.02448530","5":"N"},{"1":"King Midas","2":"carrots","3":"1858","4":"4.09618396","5":"H"},{"1":"King Midas","2":"carrots","3":"1858","4":"4.09618396","5":"L"},{"1":"leaves","2":"beets","3":"101","4":"0.22266662","5":"NA"},{"1":"Lettuce Mixture","2":"lettuce","3":"2154","4":"4.74875148","5":"G"},{"1":"Long Keeping Rainbow","2":"onions","3":"1502","4":"3.31133924","5":"H"},{"1":"Magnolia Blossom","2":"peas","3":"3383","4":"7.45822946","5":"B"},{"1":"Main Crop Bravado","2":"broccoli","3":"967","4":"2.13186754","5":"D"},{"1":"Main Crop Bravado","2":"broccoli","3":"967","4":"2.13186754","5":"I"},{"1":"Mortgage Lifter","2":"tomatoes","3":"11941","4":"26.32536742","5":"J"},{"1":"Mortgage Lifter","2":"tomatoes","3":"11941","4":"26.32536742","5":"N"},{"1":"mustard greens","2":"lettuce","3":"23","4":"0.05070626","5":"NA"},{"1":"Neon Glow","2":"Swiss chard","3":"3122","4":"6.88282364","5":"M"},{"1":"New England Sugar","2":"pumpkins","3":"20348","4":"44.85960776","5":"K"},{"1":"Old German","2":"tomatoes","3":"12119","4":"26.71778978","5":"J"},{"1":"perrenial","2":"chives","3":"8","4":"0.01763696","5":"NA"},{"1":"perrenial","2":"raspberries","3":"843","4":"1.85849466","5":"NA"},{"1":"perrenial","2":"strawberries","3":"592","4":"1.30513504","5":"NA"},{"1":"pickling","2":"cucumbers","3":"19781","4":"43.60958822","5":"L"},{"1":"purple","2":"potatoes","3":"1365","4":"3.00930630","5":"D"},{"1":"red","2":"potatoes","3":"2011","4":"4.43349082","5":"I"},{"1":"Red Kuri","2":"squash","3":"10311","4":"22.73183682","5":"A"},{"1":"Red Kuri","2":"squash","3":"10311","4":"22.73183682","5":"B"},{"1":"Red Kuri","2":"squash","3":"10311","4":"22.73183682","5":"side"},{"1":"reseed","2":"lettuce","3":"45","4":"0.09920790","5":"NA"},{"1":"Romanesco","2":"zucchini","3":"45227","4":"99.70834874","5":"D"},{"1":"Russet","2":"potatoes","3":"4124","4":"9.09185288","5":"D"},{"1":"saved","2":"pumpkins","3":"34896","4":"76.93241952","5":"B"},{"1":"Super Sugar Snap","2":"peas","3":"4340","4":"9.56805080","5":"A"},{"1":"Sweet Merlin","2":"beets","3":"2897","4":"6.38678414","5":"H"},{"1":"Tatsoi","2":"lettuce","3":"1313","4":"2.89466606","5":"P"},{"1":"thai","2":"hot peppers","3":"67","4":"0.14770954","5":"potB"},{"1":"unknown","2":"apple","3":"156","4":"0.34392072","5":"NA"},{"1":"variety","2":"hot peppers","3":"599","4":"1.32056738","5":"potC"},{"1":"variety","2":"peppers","3":"1656","4":"3.65085072","5":"potA"},{"1":"variety","2":"peppers","3":"1656","4":"3.65085072","5":"potA"},{"1":"variety","2":"peppers","3":"1656","4":"3.65085072","5":"potD"},{"1":"volunteers","2":"tomatoes","3":"23411","4":"51.61235882","5":"N"},{"1":"volunteers","2":"tomatoes","3":"23411","4":"51.61235882","5":"J"},{"1":"volunteers","2":"tomatoes","3":"23411","4":"51.61235882","5":"front"},{"1":"volunteers","2":"tomatoes","3":"23411","4":"51.61235882","5":"O"},{"1":"Waltham Butternut","2":"squash","3":"11009","4":"24.27066158","5":"A"},{"1":"Waltham Butternut","2":"squash","3":"11009","4":"24.27066158","5":"K"},{"1":"yellow","2":"potatoes","3":"3357","4":"7.40090934","5":"I"},{"1":"yellow","2":"potatoes","3":"3357","4":"7.40090934","5":"I"},{"1":"Yod Fah","2":"broccoli","3":"372","4":"0.82011864","5":"P"},{"1":"Butternut (saved)","2":"squash","3":"NA","4":"NA","5":"A"},{"1":"Cinderalla's Carraige","2":"pumpkins","3":"NA","4":"NA","5":"A"},{"1":"Grandma Einck's","2":"dill","3":"NA","4":"NA","5":"wagon"},{"1":"Long Island","2":"brussels sprouts","3":"NA","4":"NA","5":"D"},{"1":"Big Max","2":"pumpkins","3":"NA","4":"NA","5":"side"},{"1":"Cinderalla's Carraige","2":"pumpkins","3":"NA","4":"NA","5":"side"},{"1":"Doll Babies","2":"watermelon","3":"NA","4":"NA","5":"side"},{"1":"honeydew","2":"melon","3":"NA","4":"NA","5":"side"},{"1":"Improved Helenor","2":"rudabaga","3":"NA","4":"NA","5":"E"},{"1":"perennial","2":"strawberries","3":"NA","4":"NA","5":"F"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.

The first step I would take would be to modify, and simplify some of the garden_harvest data. Presumably by finding the total accumulated harvest of each individual vegetable and variety combination. Once this has been done, and there is only one row for each vegetable and variety combo in the new garden_harvest data set. I would attempt to add another variable using mutate(), a new variable that would do its best to approximate how much it would cost to buy the amount of produce that Lisa grew and collected herself. Next I would do an inner_join from that data to the data set garden_spending by the variables "vegetable" and "variety". Finally, using this new joined dataset I would again mutate a new variable, one that takes the already found estimated price of Lisa's produce by vegetable variety, and subtract what Lisa paid for the seeds. This should give us the difference between what it cost Lisa originally, and what it would cost the average person now, evaluating Lisa's savings. 


  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order. 


```r
plant_min_date <- garden_planting %>% 
  group_by(vegetable, variety) %>% 
  summarize(earliest_planting = min(date))

days_of_planting_and_harvest <- garden_harvest %>% 
  left_join(plant_min_date,
            by = c("vegetable", "variety")) %>% 
  na.omit()
  
days_to_harvest <- days_of_planting_and_harvest %>% 
  group_by(vegetable, variety) %>% 
  mutate(day_diff = date - earliest_planting) %>% 
  summarize(plant_to_harvest = min(day_diff)) %>% 
  arrange(plant_to_harvest)
  
days_to_harvest
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["plant_to_harvest"],"name":[3],"type":["drtn"],"align":["right"]}],"data":[{"1":"spinach","2":"Catalina","3":"26 days"},{"1":"lettuce","2":"Lettuce Mixture","3":"32 days"},{"1":"radish","2":"Garden Party Mix","3":"35 days"},{"1":"basil","2":"Isle of Naxos","3":"38 days"},{"1":"cilantro","2":"cilantro","3":"38 days"},{"1":"lettuce","2":"Farmer's Market Blend","3":"40 days"},{"1":"kale","2":"Heirloom Lacinto","3":"42 days"},{"1":"cucumbers","2":"pickling","3":"44 days"},{"1":"beans","2":"Classic Slenderette","3":"46 days"},{"1":"zucchini","2":"Romanesco","3":"46 days"},{"1":"lettuce","2":"Tatsoi","3":"49 days"},{"1":"Swiss chard","2":"Neon Glow","3":"50 days"},{"1":"beans","2":"Bush Bush Slender","3":"51 days"},{"1":"tomatoes","2":"grape","3":"52 days"},{"1":"jalapeño","2":"giant","3":"57 days"},{"1":"peas","2":"Magnolia Blossom","3":"59 days"},{"1":"peas","2":"Super Sugar Snap","3":"59 days"},{"1":"hot peppers","2":"thai","3":"60 days"},{"1":"hot peppers","2":"variety","3":"60 days"},{"1":"tomatoes","2":"Big Beef","3":"62 days"},{"1":"tomatoes","2":"Bonny Best","3":"62 days"},{"1":"tomatoes","2":"volunteers","3":"62 days"},{"1":"peppers","2":"variety","3":"64 days"},{"1":"tomatoes","2":"Better Boy","3":"65 days"},{"1":"tomatoes","2":"Cherokee Purple","3":"65 days"},{"1":"beets","2":"Gourmet Golden","3":"66 days"},{"1":"beets","2":"Sweet Merlin","3":"66 days"},{"1":"tomatoes","2":"Amish Paste","3":"66 days"},{"1":"tomatoes","2":"Mortgage Lifter","3":"68 days"},{"1":"tomatoes","2":"Jet Star","3":"69 days"},{"1":"tomatoes","2":"Old German","3":"69 days"},{"1":"broccoli","2":"Yod Fah","3":"72 days"},{"1":"tomatoes","2":"Black Krim","3":"73 days"},{"1":"tomatoes","2":"Brandywine","3":"73 days"},{"1":"beans","2":"Chinese Red Noodle","3":"75 days"},{"1":"peppers","2":"green","3":"75 days"},{"1":"corn","2":"Dorinny Sweet","3":"78 days"},{"1":"onions","2":"Delicious Duo","3":"81 days"},{"1":"carrots","2":"King Midas","3":"82 days"},{"1":"corn","2":"Golden Bantam","3":"82 days"},{"1":"carrots","2":"Dragon","3":"83 days"},{"1":"onions","2":"Long Keeping Rainbow","3":"85 days"},{"1":"edamame","2":"edamame","3":"87 days"},{"1":"carrots","2":"Bolero","3":"89 days"},{"1":"potatoes","2":"purple","3":"96 days"},{"1":"potatoes","2":"yellow","3":"96 days"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"104 days"},{"1":"pumpkins","2":"saved","3":"104 days"},{"1":"squash","2":"Blue (saved)","3":"104 days"},{"1":"broccoli","2":"Main Crop Bravado","3":"110 days"},{"1":"pumpkins","2":"New England Sugar","3":"117 days"},{"1":"squash","2":"delicata","3":"117 days"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"120 days"},{"1":"squash","2":"Red Kuri","3":"122 days"},{"1":"squash","2":"Waltham Butternut","3":"122 days"},{"1":"potatoes","2":"Russet","3":"137 days"},{"1":"potatoes","2":"red","3":"146 days"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  select(vegetable, variety) %>% 
  mutate(variety_length = str_length(variety)) %>% 
  group_by(vegetable, variety) %>% 
  summarize(variety_length_final = min(variety_length)) %>% 
  mutate(variety_lower = str_to_lower(variety)) %>% 
  arrange(vegetable, variety_length_final)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["variety_length_final"],"name":[3],"type":["int"],"align":["right"]},{"label":["variety_lower"],"name":[4],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"7","4":"unknown"},{"1":"asparagus","2":"asparagus","3":"9","4":"asparagus"},{"1":"basil","2":"Isle of Naxos","3":"13","4":"isle of naxos"},{"1":"beans","2":"Bush Bush Slender","3":"17","4":"bush bush slender"},{"1":"beans","2":"Chinese Red Noodle","3":"18","4":"chinese red noodle"},{"1":"beans","2":"Classic Slenderette","3":"19","4":"classic slenderette"},{"1":"beets","2":"leaves","3":"6","4":"leaves"},{"1":"beets","2":"Sweet Merlin","3":"12","4":"sweet merlin"},{"1":"beets","2":"Gourmet Golden","3":"14","4":"gourmet golden"},{"1":"broccoli","2":"Yod Fah","3":"7","4":"yod fah"},{"1":"broccoli","2":"Main Crop Bravado","3":"17","4":"main crop bravado"},{"1":"carrots","2":"Bolero","3":"6","4":"bolero"},{"1":"carrots","2":"Dragon","3":"6","4":"dragon"},{"1":"carrots","2":"greens","3":"6","4":"greens"},{"1":"carrots","2":"King Midas","3":"10","4":"king midas"},{"1":"chives","2":"perrenial","3":"9","4":"perrenial"},{"1":"cilantro","2":"cilantro","3":"8","4":"cilantro"},{"1":"corn","2":"Dorinny Sweet","3":"13","4":"dorinny sweet"},{"1":"corn","2":"Golden Bantam","3":"13","4":"golden bantam"},{"1":"cucumbers","2":"pickling","3":"8","4":"pickling"},{"1":"edamame","2":"edamame","3":"7","4":"edamame"},{"1":"hot peppers","2":"thai","3":"4","4":"thai"},{"1":"hot peppers","2":"variety","3":"7","4":"variety"},{"1":"jalapeño","2":"giant","3":"5","4":"giant"},{"1":"kale","2":"Heirloom Lacinto","3":"16","4":"heirloom lacinto"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"17","4":"crispy colors duo"},{"1":"lettuce","2":"reseed","3":"6","4":"reseed"},{"1":"lettuce","2":"Tatsoi","3":"6","4":"tatsoi"},{"1":"lettuce","2":"mustard greens","3":"14","4":"mustard greens"},{"1":"lettuce","2":"Lettuce Mixture","3":"15","4":"lettuce mixture"},{"1":"lettuce","2":"Farmer's Market Blend","3":"21","4":"farmer's market blend"},{"1":"onions","2":"Delicious Duo","3":"13","4":"delicious duo"},{"1":"onions","2":"Long Keeping Rainbow","3":"20","4":"long keeping rainbow"},{"1":"peas","2":"Magnolia Blossom","3":"16","4":"magnolia blossom"},{"1":"peas","2":"Super Sugar Snap","3":"16","4":"super sugar snap"},{"1":"peppers","2":"green","3":"5","4":"green"},{"1":"peppers","2":"variety","3":"7","4":"variety"},{"1":"potatoes","2":"red","3":"3","4":"red"},{"1":"potatoes","2":"purple","3":"6","4":"purple"},{"1":"potatoes","2":"Russet","3":"6","4":"russet"},{"1":"potatoes","2":"yellow","3":"6","4":"yellow"},{"1":"pumpkins","2":"saved","3":"5","4":"saved"},{"1":"pumpkins","2":"New England Sugar","3":"17","4":"new england sugar"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"21","4":"cinderella's carraige"},{"1":"radish","2":"Garden Party Mix","3":"16","4":"garden party mix"},{"1":"raspberries","2":"perrenial","3":"9","4":"perrenial"},{"1":"rutabaga","2":"Improved Helenor","3":"16","4":"improved helenor"},{"1":"spinach","2":"Catalina","3":"8","4":"catalina"},{"1":"squash","2":"delicata","3":"8","4":"delicata"},{"1":"squash","2":"Red Kuri","3":"8","4":"red kuri"},{"1":"squash","2":"Blue (saved)","3":"12","4":"blue (saved)"},{"1":"squash","2":"Waltham Butternut","3":"17","4":"waltham butternut"},{"1":"strawberries","2":"perrenial","3":"9","4":"perrenial"},{"1":"Swiss chard","2":"Neon Glow","3":"9","4":"neon glow"},{"1":"tomatoes","2":"grape","3":"5","4":"grape"},{"1":"tomatoes","2":"Big Beef","3":"8","4":"big beef"},{"1":"tomatoes","2":"Jet Star","3":"8","4":"jet star"},{"1":"tomatoes","2":"Better Boy","3":"10","4":"better boy"},{"1":"tomatoes","2":"Black Krim","3":"10","4":"black krim"},{"1":"tomatoes","2":"Bonny Best","3":"10","4":"bonny best"},{"1":"tomatoes","2":"Brandywine","3":"10","4":"brandywine"},{"1":"tomatoes","2":"Old German","3":"10","4":"old german"},{"1":"tomatoes","2":"volunteers","3":"10","4":"volunteers"},{"1":"tomatoes","2":"Amish Paste","3":"11","4":"amish paste"},{"1":"tomatoes","2":"Cherokee Purple","3":"15","4":"cherokee purple"},{"1":"tomatoes","2":"Mortgage Lifter","3":"15","4":"mortgage lifter"},{"1":"zucchini","2":"Romanesco","3":"9","4":"romanesco"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(has_er_ar = str_detect(variety, "er|ar")) %>% 
  filter(has_er_ar == "TRUE") %>% 
  distinct(vegetable, variety)  
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix"},{"1":"lettuce","2":"Farmer's Market Blend"},{"1":"peas","2":"Super Sugar Snap"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"lettuce","2":"mustard greens"},{"1":"raspberries","2":"perrenial"},{"1":"beans","2":"Bush Bush Slender"},{"1":"beets","2":"Sweet Merlin"},{"1":"hot peppers","2":"variety"},{"1":"tomatoes","2":"Cherokee Purple"},{"1":"tomatoes","2":"Better Boy"},{"1":"peppers","2":"variety"},{"1":"tomatoes","2":"Mortgage Lifter"},{"1":"tomatoes","2":"Old German"},{"1":"tomatoes","2":"Jet Star"},{"1":"carrots","2":"Bolero"},{"1":"tomatoes","2":"volunteers"},{"1":"beans","2":"Classic Slenderette"},{"1":"pumpkins","2":"Cinderella's Carraige"},{"1":"squash","2":"Waltham Butternut"},{"1":"pumpkins","2":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(x = sdate)) +
  geom_density() + 
  theme(axis.text.y = element_blank()) + 
  labs(title = "Density of Bicycle Use By Date", x = "", y = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

It seems that bicycle use decreased as the winter months approached, and then arrived. This may be attributed to worsening weather conditions. 

  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density() +
  labs(title = "Bike Rental by Time of Day (hrs)", x = "", y = "") +
  theme(axis.text.y = element_blank())
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
It seems like the largest usage of the bikes occurs at around 8am and 5pm - 6pm. This is interesting, such observations can likely be paired with the knowlege of a 9am - 5pm work day. 
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(y = week_day)) +
  geom_bar() + 
  labs(title = "Usage of Bicycles by Day of the Week", x = "", y = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
This bar graph gives us insight into the usage of bikes by weekday. The data makes it clear that the bikes were used less on weekends than they were during the week.
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE),
         hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density() +
  labs(title = "Bike Rental by Time of Day (hrs)", x = "", y = "") +
  theme(axis.text.y = element_blank()) + 
  facet_wrap(vars(week_day))
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
The correlation between the distrobution of bike rentals in the graph above does an even better job at highlighting the impact of the average work commute. During the work week the most popular times are at 8am and 5pm, while on weekends the usage is dispersed over a larger time period in the afternoon.  
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE),
         hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density(aes(fill = client, alpha = .5), color = NA) +
  labs(title = "Bike Rental by Time of Day (hrs) and Type of Client", x = "", y = "") +
  theme(axis.text.y = element_blank()) + 
  facet_wrap(vars(week_day))
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

This new "fill"ed graph shows a density plot for both types of clients, exposing a serious difference in usage patterns. During the workweek, the registered clients are quite inactive from 10am - 3pm, while this is when the casual clients are most active.

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE),
         hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density(aes(fill = client, alpha = .5), color = NA, position = position_stack()) +
  labs(title = "Bike Rental by Time of Day (hrs) and Type of Client", x = "", y = "") +
  theme(axis.text.y = element_blank()) + 
  facet_wrap(vars(week_day))
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

This new stacking functions simply graphs the registered density, and then adds the data from casual users on top. The disadvantage of this type of graph is the difference in usage patterns is not easily seen. This may be because the number of registered users is bigger, or they are just more consistant/using the bikes more often. On the other hand, this stacked graph is useful because it helps us understand how the density of the total bike user population is formed by each client subset. 
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE),
         hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60,
         type_of_day = ifelse(wday(sdate) %in% c(1,7), "weekend", "weekday")) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density(aes(fill = client, alpha = .5), color = NA) +
  labs(title = "Bike Rental by Time of Day (hrs), Type of Client, and Type of Day", x = "", y = "") +
  theme(axis.text.y = element_blank()) + 
  facet_wrap(vars(type_of_day))
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(week_day = wday(sdate, label = TRUE),
         hour2 = hour(sdate),
         minute2 = minute(sdate),
         time_of_day = hour2 + minute2/60,
         type_of_day = ifelse(wday(sdate) %in% c(1,7), "weekend", "weekday")) %>% 
  ggplot(aes(x = time_of_day)) + 
  geom_density(aes(fill = type_of_day, alpha = .5), color = NA) +
  labs(title = "Bike Rental by Time of Day (hrs), Type of Client, and Type of Day", x = "", y = "") +
  theme(axis.text.y = element_blank()) + 
  facet_wrap(vars(client))
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
This graph better compares the shift withing client type regarding the different parts of the week. It is very easy to see the most popular time of usage shift for the registered users on the weekends. 
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Trips %>% 
  count(sstation) %>% 
  inner_join(Stations,
            by = c("sstation" = "name")) %>% 
  ggplot(aes(x = lat, y = long, color = n)) +
  geom_point() +
  labs(title = "Total Departures From Each Station Represented by Their Longitude and Latitude", x = "Latitude", y = "Longitude")
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
After assigning the color to the number of departures from a given station, it becomes clear that the majority of the bikes usage comes in one central area.   
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Trips %>% 
  group_by(sstation) %>% 
  summarize(total_departures = n(),
            prop = mean(client == "Casual")) %>% 
  inner_join(Stations,
             by = c("sstation" = "name")) %>% 
  ggplot(aes(x = lat, y = long, color = prop)) +
  geom_point() + 
  labs(title = "Stations with the Highest Casual User Percentage by Longitude and Latitude", x = "Latitude", y = "Longitude")
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
While certain stations within the biggest cluster have a higher proportion of casual users, there are also stations on the outskirts that have a high percentage. It also seems that there is a correlation between the percentage of users being casual, and the overall departures from that station.   
  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

```r
Top_departure_dates <- Trips %>% 
  mutate(date = as_date(sdate)) %>% 
  group_by(sstation, date) %>% 
  summarize(num_departures = n()) %>% 
  arrange(desc(num_departures)) %>% 
  head(n = 10)
  
Top_departure_dates
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sstation"],"name":[1],"type":["chr"],"align":["left"]},{"label":["date"],"name":[2],"type":["date"],"align":["right"]},{"label":["num_departures"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Lincoln Memorial","2":"2014-10-25","3":"386"},{"1":"Lincoln Memorial","2":"2014-10-18","3":"354"},{"1":"Lincoln Memorial","2":"2014-10-26","3":"349"},{"1":"Columbus Circle / Union Station","2":"2014-10-27","3":"345"},{"1":"Lincoln Memorial","2":"2014-10-04","3":"337"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"334"},{"1":"Columbus Circle / Union Station","2":"2014-10-28","3":"333"},{"1":"Lincoln Memorial","2":"2014-10-12","3":"328"},{"1":"Columbus Circle / Union Station","2":"2014-10-09","3":"327"},{"1":"Columbus Circle / Union Station","2":"2014-10-08","3":"322"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

```r
Top_departure_dates %>% 
  left_join(Trips,
             by = c("sstation", "date"))
```

```
## Error: Join columns must be present in data.
## x Problem with `date`.
```
  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.
  

```r
Top_departure_dates %>% 
  inner_join(Trips,
             by = c("sstation","date")) %>% 
  mutate(week_day = wday(sdate, label = TRUE)) %>% 
  group_by(client, week_day) %>% 
  summarize(client_departures = n()) %>% 
  group_by(client) %>% 
  mutate(prop = client_departures/(sum(client_departures))) %>% 
  pivot_wider(id_col = week_day ,
              names_from = client,
              values_from = prop)
```

```
## Error: Join columns must be present in data.
## x Problem with `date`.
```


**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.
  
[Github Link](https://github.com/cwillia8/Weekly-Exercise-3)

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
