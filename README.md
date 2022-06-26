---
title: "Plotting_and_summary_stats"
author: "Trevor D. Millar"
date: '2022-06-26'
header-includes:
   - \usepackage('BHH2')
output: html_document

---
## Disclaimer:
Trevor D. Millar is the original author of this work, which is not to be used or distributed without express written permission by the author who can be contacted at tmillar32@gmail.com. This work is currently in progress, has not been proof read, and is not ready for publication. 

## Intro to Lecture 3
Welcome to the third lecture in the series. In this lecture, we will be going through some basic plotting and summary statistics for the turtles dataset that we looked at briefly at the end of the last lecture. 

Let's go ahead and load that data up!
```{r}
turtles <- read.csv('Phillips_et_al._2021_all_data.csv')
head(turtles)
```

This just serves as a brief reminder to see what the data set looks like. Lets get into some plots and learn more about this data set. 

## Intro to Plotting in R
R is a powerful plotting tool, but it requires that we know something about the data we are trying to plot. It will be useful to look at the variables in our data, and ask ourselves whether that variable is a continous data type or if its relegated to just a few discrete values. Let's demonstrate:

```{r}
levels(turtles$Species) ## Our species name has only two possible values
turtles$SCL ## Straight carapace length is a continous variable
```

The two variables we looked at have distinct natures to them. The Straight carapace length (one measure for the size of the shell) is not bound by any discrete values. Theoretically SCL could have any real number value above zero. Contrast that with the two discrete values that we have for species, it's either C caretta or C mydas. As we go through each of these plot types, I would encourage you to consider what types of data we are plotting in each case so that you can choose the correct plots to display the real world data you work with in the future. 

### Histogram
A histogram is used when we are trying to get a feel for the data in a single continuous numerical variable. The R code for generating a histogram is pretty straight forward, here we are using the function hist(), where the required argument is the vector of data that we would like to visualize. Let's have a look.
```{r}
hist(turtles$SCL)
```

As we can see, the histogram gives us a general understanding of the distribution of our data. The measured shell sizes are grouped into bins, and the frequency of all observations with values within that bin is used to determine the height of each bin. As for our graph, we aren't necesarily winning any style points as is. After all, take a look at the title and the label on the x axis. They're understandable to the person who wrote the code for sure, but we should probably clean those up a little bit for a presentation ready product. We can change the main title by using the parameter main in the hist function, and the x label can be changed with xlab in the same. 

```{r}
hist(turtles$SCL, main = 'Distribution of Straight Carapace Length', xlab = 'Straight Carapace Length')
```
Not to bad for our first R plot!

It should be pointed out that sometimes grouping data together into these larger bins doesn't give us the full picture we would like to see with our data. In this case, a dot plot would be better suited. 

### Dot Plot, the better alternative to the Histogram
Remember when we discussed R packages in the first lecture? We've made it to the point where we need to recruit the power of an R package to extend the tools we have in R. Recall that a large number of R-packages are managed by CRAN. Let's see about installing a BHH2, a package that gives us a useful function to create dot plots. 

```{r}
#install.packages('BHH2') ## install BHH2 (one and done)
library('BHH2') ## Tell the computer we are going to use this pacakge
```
Once we have BHH2, we have access to the dotPlot() function. It's used in much the same way as the hist() function that we already discussed. 

```{r}
dotPlot(turtles$SCL, xlab = 'Straight Carapace Length', main = 'Distribution of Turtle Shell Sizes')
```

As we can see, a dot plot gives us a little higher 'resolution' than the 'grainy' histogram. This is because each data point is graphed individually as opposed to grouping large portions of it together. This allows us to see that normally distributed data a little bit better. 

### Bar Plots
Bar plots are ideal in situations where we wish to plot the number of observations of a discrete variable. An example of this in the turtles dataset is the number of different species in the study. In order to produce a bar plot, we first need to produce a table that contains the values we are trying to plot. We can create a table using the table() function and feeding it a variable with discrete levels to it (ie Turtle Species). Table() counts up the number of each species in the data set and returns a one-way table with the results. We can then feed the table that we get returned into the barplot() function, which is used in much the same way as the other two plotting functions we have covered already. 

```{r}
turtle_table<- table(turtles$Species) ## produce a one-way table
turtle_table ## have a look
barplot(turtle_table) ## Produce a bar plot of our one way table
```

For a basic bar plot, this isn't too bad. Lets see if we can clean it up by giving it a title (using main = some character), changing the colors of the bars (using col = some vector), and by setting the scale of the y axis from 0 - 10,000 (using ylim = a numerical vector)
```{r}
barplot(turtle_table, main = 'Species Counts', col = c('blue','green'), ylim = c(0,10000))
```


### Pie Charts
turtle_props <- prop.table(turtle_table)
turtle_props
pie(turtle_props, main = 'Species Percentages', col = c('blue', 'green'))
turtle_twoway_table <- table(turtles$Species, turtles$Year)
turtle_twoway_table


barplot(turtle_twoway_table, main = 'Turtle Observations by Year', col = c('blue', 'green'), beside = T, ylim = c(0,800), args.legend = list(x = 'topleft'),legend.text = c('Loggerhead', 'Atlantic Green')) 

boxplot(turtles$SCL ~turtles$Species, main = 'Turtle Straight Carapace Length', ylab = 'Straight Carapace Length', xlab = 'Sea Turtle Species')
## Straight Carapace Length

## Now that we have the subset of the data frame that only includes
## Caretta caretta, the loggerhead sea turtle, lets learn about summary stats

## Lets do some conditional indexing, this should look familiar to 
## our first lecture on R basics (notice the comma)

car.car <- turtles[turtles$Species == 'Caretta caretta',]
car.car

## Now that we have seen a box plot, lets discuss the summary statistics that it displays
boxplot(car.car$SCL)
mean(car.car$SCL)
median(car.car$SCL)
min(car.car$SCL)
max(car.car$SCL)
IQR(car.car$SCL)
quantile(car.car$SCL, 0.25)
quantile(car.car$SCL, 0.90)
summary(car.car$SCL)
outlier_threshold <- 1.5 * IQR(car.car$SCL)
lowerbound <- quantile(car.car$SCL, 0.25) - outlier_threshold
upperbound<- quantile(car.car$SCL, 0.75) + outlier_threshold
upperbound
lowerbound
boxplot(turtles$SCL ~turtles$Species)
summary_stats<-boxplot(turtles$SCL~turtles$Species)$stats
colnames(summary_stats)<-c("Caretta caretta","Chelonia mydas")
rownames(summary_stats)<-c("Min","First Quartile","Median","Third Quartile","Maximum")
summary_stats

## Let's look at how to deal with those outliers really quick
## How can we filter the dataset for only those observations which are found
## within our defined threshold?

turtle_filter<-car.car[car.car$SCL > lowerbound & car.car$SCL < upperbound,]
boxplot(turtle_filter$SCL)





## Normal Distributions, Standard deviations


