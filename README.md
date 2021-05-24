# college-data
College dataset with college tiers, parent and child income, and quintile income info

# Loading .csv files

Loading in Excel datasets is the most common way to import data into R. Using the .csv format
is the easiest way to load them in. There are thousands of free datasets online to use, and many
of them come in .zip files because they are compressed, and this makes them easier to share
over the internet. Here is a website with .csv .zip files to check out:

https://support.spatialkey.com/spatialkey-sample-csv-data/

For the examples I use here, I am going to use this dataset I made using data from
Opportunity Insights, a research institute that uses data to identify barriers throughout America. They have thousands of free datasets to use here:

https://opportunityinsights.org/data/


They have many great datasets, and I am using one involving college tiers, parent and child income,
and quintile info. Here is a direct link to download the Excel file through Dropbox:

https://www.dropbox.com/s/mxpl8uvcis148ip/collegedata.csv?dl=0

Save the file to your downloads folder as "collegedata"

Here is a file that explains the meaning of the variables in the sheet:

https://docs.google.com/document/d/1_PHn9goPHpbYm4N3KrtQEVKDeAeKN_Ymq_uYhPgYCJg/edit?usp=sharing

# Load Data

``` r
import dataset

collegedata = read.csv("~/downloads/collegedata.csv")

view(collegedata)
```

ARGUMENT MEANINGS

`collegedata` = the name of the variable we have assigned to the dataset

`downloads` = this is where we have saved the file to on our computer, if we saved it to the desktop,
this would say desktop instead. 

`view()` = this allows you to see the dataset in r


# Subsetting Data

Subsetting data in R allows you to select specific variables in a dataset. We can subset data based on 
variable, column, and many other things

We want to subset the data by college tier so that instead of using multiple variables, therefore multiple functions,
we can subset the college tiers into bigger groups and use those variables.

We can group the tiers from 1-3, 4-6, 7-9, and 14

The collegetier column is already sorted numerically, so we can just choose the columns
we want by pulling those and assigning them a name

``` r
one = college[c(2:103), c(1:7)]
two = college[c(104:1133), c(1:7)]
three = college[c(1134:2004), c(1:7)]
four = college[c(2005:2203), c(1:7)]

```


ARGUMENT MEANINGS:
`one` `two` `three` `four` = the names we have assigned the variables

`college` the dataset we are pulling the numbers from

`c(2:103), c(2:7)` = We want all numbers from 1-3, so we can scroll through
the Excel sheet to find when those numbers end and begin. We want to contain the
data from columns 1 - 7 into the variable, so we keep that in the new variable we make.

If your data is not sorted, you can sort it in Excel, or we can use another way:

``` r
tierone = subset(college, collegetier <= 3 | kmean <= 90000)

```
Now, `tierone` contains all colleges in tiers from 1-3, and from those rows, 
only contains observations where the mean income of the child is equal to 
or less than $90,0000. We can use `<=` `==` `>=` for other functions as well. There are many
ways to subset data, and even packages, such as the `dplyr` package, which allows for more
intuitive ways to subset rather than what is included in the base R package.

# Histograms

Histograms are a great way to represent a singlular variable. In this case, we are going
to represent the parent mean income with a histogram using GGPlot2, which you will need to install.

``` r
install.packages("ggplot2")

# load libraries
library(ggplot2)

# load data
college <- read.csv("~/downloads/collegedata.csv")

# basic histogram
ggplot(collegedata, aes(x = pmean)) + geom_histogram()
```

`ggplot()` initializes the GGplot2 function, and is where you input the dataset
that is being used, and if applicable, what the x and y variables will be. Here,
we are using `pmean` as our x value which is okay because it is numerical. Non-numerical
variables cannot be used as x-values. You need to add another function to this argument,
in this case, `geom_histogram()` as this initiates the actual graph that you are trying to use.

To add more color or edit names of the graph, we edit inside of the `geom_histogram()` argument.
For a broader color selection, Google "ggplot colors" for the arguments. You can also
change the line type, such as making it a dashed line.

``` r
# change colors
ggplot(one, aes(x = pmean)) + 
  geom_histogram(color="chartreuse1", fill = "wheat", 
                 linetype = "dashed")

```

We want to add labels to our graph so viewers know what the axes mean, and
we can do that using the `labs` function

``` r
ggplot(one, aes(x = pmean)) + 
  geom_histogram(color="chartreuse1", fill = "wheat", 
                 linetype = "dashed") +
  labs(title = "College Data",
       y = "Amount", x = "Parental Mean Income") 

```
Here, `title` is what will be displayed at the top of the graph, `x` is what the
x axis will be labeled, and `y` is the label of the y axis.

## Overlaying Histograms

With ggplot2, we can overlay multiple histograms on top of one another,
allowing us to compare data easily. We are going to compare the parental mean incomes
of all four of the subsets we have earlier created.

``` r
# subset data (using the pound sign allows you to add
# notes without interfering with the code)
one = college[c(2:103), c(1:7)]
two = college[c(104:1133), c(1:7)]
three = college[c(1134:2004), c(1:7)]
four = college[c(2005:2203), c(1:7)]

# basic histogram
ggplot(one, aes(x = pmean, fill = group)) + geom_histogram(fill = "red", 
                 alpha = 0.6)

```

We add the argument `fill = group` to establish that there will be
more than one histogram on a singular graph, and we add `alpha = 0.6` to adjust
the opaqueness of the graphs.

``` r
ggplot(one, aes(x = pmean, fill = group)) + 
  geom_histogram(fill = "red", 
                 alpha = 0.6) +
  geom_histogram(data = two, fill = "green",
                 alpha = 0.6)

```

Now we have two histograms, let's add two more:

``` r
ggplot(one, aes(x = pmean, fill = group)) + 
  geom_histogram(fill = "red", 
                 alpha = 0.6) +
  geom_histogram(data = two, fill = "green",
                 alpha = 0.6) +
  geom_histogram(data = three, fill = "turquoise",
                 alpha = 0.6) +
  geom_histogram(data = four, fill = "chartreuse",
                 alpha = 0.6)
```

# Boxplots

Boxplots are very similar to histograms, as they display the data of a single 
numeric variable and show distribution. We can use ggplot2 to display multiple
boxplots in a singular graph. Here, we are going to use a dataset that is already
loaded into R, called `mtcars`.

```r
# load data and libraries
library(ggplot2)
cars = mtcars

p = ggplot(mpg, aes(class, hwy)) +
  geom_boxplot()
p

```
We now have a basic scatterplot that shows the distribution of highway mileage for different
types of cars. Notice how `p` was set equal to the function, rather than directly starting with `ggplot()`
We now have the boxplot permanently attached to `p`, so we can overlay graphs, such as dotplots,
on p.

```r
p + geom_dotplot(binaxis = 'y',
                 stackdir = 'center',
                 dotsize = .2)

```

## Violin Plots
Violin plots are very similar to boxplots, but they show the density of data at different
values. For this example, we can use the preloaded diamonds dataset

```r
# violin plots example
# load libraries and data
library(tidyverse)
library(plotly)
library(IRdisplay)

data = diamonds

# load colors
colors <- c("green", "red", "blue", "purple",
            "cyan1", "chartreuse", "wheat")
```
Now, we can make the violin plot:

```r
data %>% ggplot(aes(x = "", y = carat)) +
  geom_violin() +
  geom_boxplot(width = 0.1) +
  theme(axis.title.x = element_blank())
 
```
`geom_boxplot()` this adds a boxplot over the violin plot, which makes it easier to visualize quintiles, etc.

We leave the x argument empty because we are only measuring one variable, which is the carat.
We can measure multiple violin plots against each other as well.

```r

# side by side violin plot
data %>% ggplot(aes(x = color, y = carat, fill = color)) +
  geom_violin(draw_quantiles = TRUE) +
  geom_boxplot(width = 0.05) +
  theme(axis.title.x = element_blank()) +
  scale_fill_manual(values = colors)
```

Here, we set `fill` and `x` equal to each other so that the fill of the violin plot maatches the color data. 

`scale_fill_manual(values = colors)` here, we are using the color variable we made earlier. 


# Scatterplot

Scatterplots show the correlational relationship between and x and a y variable. We are
going to make a scatterplot out of our subsets, starting with the `one` subset.

``` r
# load libraries
library(ggplot2)

# load dataset and subsets
college <- read.csv("~/downloads/collegedata.csv")
one = college[c(2:103), c(1:7)]
two = college[c(104:1133), c(1:7)]
three = college[c(1134:2004), c(1:7)]
four = college[c(2005:2203), c(1:7)]

# construct scatterplot
ggplot(one, aes(x=pmean, y=kmean)) +
  geom_point(shape = 1)

```

Here, `geom_point()` is the argument that is actually constructing the scatterplot.
`shape = 1` is simply the shape of the dots, there are different shapes for each number
from 1 - 16. Here, `pmean` is the x variable because we are trying to find correlation.
To better observe this, we can add linear regression line.

```r
ggplot(one, aes(x=pmean, y=kmean)) +
  geom_point(shape = 1) +
  geom_smooth(method=lm, color = "turquoise")
```
`geom_smooth` adds the argument of the linear regression line, and we can add colors, etc.

# Bubble Graph

Bubble graphs are very similar to scatterplots, but a third variable is added,
which correlates to the size of the bubble. We are going to use a dataset that is
preloaded into R for this example, the `mtcars` dataset.

``` r
# load libraries and data
library(ggplot2)
library(plotly)
cars = mtcars

# observe dataset
view(cars)

# form the data variable
data = mtcars %>% mutate(cyl = factor(cyl),
                         Model = rownames(mtcars))

# set axes for the graph
plot = data %>% ggplot(aes(x = wt, y = mpg, size = hp)) +
  geom_point(alpha = 0.5)
plot

# plot data
plot
```

`view(cars)` this allows you to visualize the dataset in R

Now we have a bubble graph of car weights and mileage, and we can tell
the horsepower of a datapoint by the size of the circle corresponding to it.
We can add another variable to this graph with color:

```r
plot2 <- data %>% ggplot(aes(x = wt, y = mpg, size = hp,
                            color = cyl, label = Model)) +
  geom_point(alpha = 0.5) +
  scale_size(range = c(.1, 15))

plot2
```

To make this graph even easier to read, we can add interactivity with the `plotly` package.

```r
# convert into plotly
g = ggplotly(plot2, width = 500, height=500) %>%
  layout(xaxis = list(range = c(1, 6)),
         yaxis = list(range = c(8, 35)),
         legend = list(x = 0.825, y = .975))

g

```

the `plotly` package makes any graph interactive, which is very helpful with bubble
graphs. the `width` and `height` commands establish the dimensions of the graph,
generally 500-800 is a good range. `xaxis` and `yaxis` establish the range of the x
and y axes, and `legend` establishes the actual placement of the legend on the page.
Labeling x as .25 would move the legend to the left, as on a graph, left is closer to zero.
Labeling y as .25 would move the legend down the screen, as it gets closer to zero. After,
simply typing `g` and running the command will graph the function.




# Barplot

Barplots are useful for showing the relationship between numeric and categorical variables.
We can make a simple barplot using the college tier as the category and the count as the
y variable:

``` r
# load libraries and datasets
library(tidyverse)
library(ggplot2)
college = read.csv("~/downloads/collegedata.csv")

# establish variable
college$collegetier = as.factor(college$collegetier)

# make barplot
barplot(table(college$collegetier))


```
This makes a very simple bar graph showing the counts for each variable. We can go
deeper and make a barplot showing the average parental mean income for each college
tier. First, we need to make a table to simplify the data so it is easier to code.

```r
table = college %>% group_by(collegetier) %>%
  summarize(avg_pmean = mean(pmean))
view(table)
```

`group_by` this function allows us to choose the variable we want to categorize
our barplot with.

`avg_pmean = mean(pmean)` this sets the other half of the table, the response, as the
average of the mean parental income. So, our graph will show each college tier's mean
parental income.

```r
table %>% ggplot(aes(x = collegetier, y = avg_pmean)) +
  geom_col(aes(fill = collegetier)) + 
  labs(title = "Parental Income and College Tier", x = "College Tier", 
       y = "Parental Mean Income") 

```
This makes the height of the graphs determined by parental income.

`geom_col` makes the heights of graphs representative of values in the data, while
`geom_bar` makes the heighhts of the graphs representative of the counts in the data.

`labs` assigns labels to the axes

## Creating a Barplot in Base R

In base R, we can create items without using outside packages, such as GGPlot2. Here, we are going to create a barplot with the cohort dataset.

```r
# insert dataset and assign it a name
co = read.csv("~/downloads/cohort.csv")

# create barplot
barplot(cohort)
```
We can add different labels for the x and y axes or the main title.

```r
barplot(cohort, xlab = "Parent Income",
        ylab = "Count", main = "Parental Income")
```


## Creating a Dataframe
Sometimes, it is required that we make our own dataset within R, which is called a dataframe. Here,
I have pulled data from an Opportunity Insights dataset. 

``` r
# load libraries
library(ggplot2)
library(tidyverse)
library(dplyr)
library(breakDown)
library(reshape2)

# create a dataframe and stacked barplots
gf = data.frame(
  parentpercentile=c("1", "2", "3", "4", "5"),
  p1=c(.144, .125, .114, .103, .109), 
  p2=c(.216, .195, .178, .163, .162),
  p3=c(.230, .228, .218, .205, .186),
  p4=c(.219, .238, .250, .256, .238),
  p5=c(.190, .214, .240, .272, .305))

view(gf)
```
I calculated these values myself, and you can do this with any set of numbers. `p1`, `p2`, and so 
on represent parental income quintiles, and the percentages given within the dataset represent
the income percentile the children of those parents ended up in. For example, out of the
children born in the first income quintile, 1-20%, 14.4% will end up in the first income quintile
themselves, or 11.4% of those children will end up in the third income quintile.


`gf` is the name of the dataframe we have created, and is how we will refer to our
new dataframe.

`c` indicates that we are using names, and not numerical values.

```r
# compress gf into a ggplot
pf = ggplot(data = gf, aes(x = parentpercentile,
                           y = value,
                           fill = p1))

```

Now, we have compressed `gf` into a `ggplot` named `pf`. 

```r
# melt data
dat.m = melt(gf, id.vars = "parentpercentile")
head(dat.m)
```
`head(dat.m)` this allows us to see the first six lines of `dat.m`


Next, we have to "melt" the data. Melting a dataframe makes it easier for the
computer to read through the dataset, therefore easier for us to work with it.
It formats the data into a longer, skinnier dataset. This makes it much easier to subset,
graph, or do anything else with the data. `head(dat.m)` allows you to view the melted data,
helping us to visualize what "melting" does

```r
dat.m = dat.m %>%
  group_by(parentpercentile) %>%
  mutate(label_y = cumsum(value))
```
Next, we format our melted data so we can graph it next. `group_by(parentpercentile)` 
establishes that we want to group all data by the parental income percentile. The `mutate`
function establishes a new variable, but preserves existing variables.

```r
# graph
ggplot(dat.m, aes(x = parentpercentile, y = value,
                  fill = variable)) +
  geom_bar(stat = "identity")
```
Now we have finally graphed the data. All graphs are the same height because
we are using percentages of a whole. 

`geom_bar(stat = "identity")` this is set equal to "identity" because the data contains
y values in the column, and identity is the default setting to achieve that.

`fill = variable` The fill of the barplot comes from the variables in the dataframe, so 
we set the fill equal to whatever our variables are.


# Linegraphs

Linegraphs are useful for showing change over time, or any other kind of association. Here, we
are going to pull data from the `cohort.csv` dataset from Opportunity Insights, which you can
download here:

https://www.dropbox.com/s/dvhwbo4j322ovpb/cohort.csv?dl=0

Let's try making a linegraph using the year as the x variable, and mean parental income
as the y variable.

```r
# load libraries and data
library(ggplot2)
library(dplyr)
co = read.csv("~/downloads/cohort.csv")

# create line graph
ggplot(co) +
  geom_line(aes(x = cohort, y = par_mean))
```

`geom_line()` is the ggplot2 function for a line graph

Here, I put the data, `co` into the `ggplot()` function. This is not the only way to 
import data into the `ggplot()`, but shows
the variety of ways to do things in R! When we graph this, it is not very easy to read.
Because of this, we can make a dataframe using the averages of parental 
income means and child income means based off of year to easier observe. I pulled the means
directly from the Excel file, and here is how to make the data frame:

```r
# create dataframe
gf = data.frame(
  year = c(1980, 1981, 1982, 1983,
           1984, 1985, 1986, 1987,
           1988, 1989, 1990, 1991),
  par_mean = c(141116, 145788, 145155, 156753,
               152916, 154771, 160378, 165877,
               181294, 181197, 173637, 170788),
  k_mean = c(51916, 49482, 47817, 43913,
             40658, 37593, 37593, 34617,
             28370, 25208, 22136, 17659))

gf
```

Now, the graph will be easier to interpret.

```r
ggplot(gf) +
  geom_line(aes(x = year, y = par_mean))
```
We still have the `k_mean` variable left, and we want to include that in our graph.
We can assign the previous function a variable, and then add another `geom_line()` function
over top of it.

```r
# assign graph1 as the variable and add color
pf = ggplot(gf) +
  geom_line(aes(x = year, y = par_mean),
            color = "cyan1")
# add the next variable over the top of it, and 
# add a color to differentiate and assign this function variable "final"
final = pf + geom_line(aes(x = year, y = k_mean),
               color = "black")
# add labels
final + labs(title = "Parent and Child Mean Income By Child Cohort",
             y = "Mean Income", x = "Cohort")
```        


# 3D Scatterplots

3D scatterplots are useful for representing the relationships between three variables.
Here, we are going to again use the `mtcars` dataset. First, we need to install the packages
necessary to use the 3D scatterplot function.

```r
# install libraries as needed (remember, anything in quotes is case sensitive)
install.packages("plot3D")
install.packages("scatterplot3d")
install.packages("rgl")

# load libraries and dataset
library(ggplot2)
library(rgl)
library(scatterplot3d)
library(plot3D)
require(rgl)
cars = mtcars
```

Now, we have the libraries needed loaded into R. The scatterplot3d function requires
us to enter our data by row and column, so we need to know how many total rows and columns
we have, and of those, which columns and rows we want to input to our plot. We can do that
using the `head()` function, which shows the first six lines of the dataset, and the
`dim()` function which tells us how many rows and columns are in the dataset

```r
# observe dataset
> head(cars)
                   mpg cyl disp  hp drat    wt
Mazda RX4         21.0   6  160 110 3.90 2.620
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875
Datsun 710        22.8   4  108  93 3.85 2.320
Hornet 4 Drive    21.4   6  258 110 3.08 3.215
Hornet Sportabout 18.7   8  360 175 3.15 3.440
Valiant           18.1   6  225 105 2.76 3.460
                   qsec vs am gear carb
Mazda RX4         16.46  0  1    4    4
Mazda RX4 Wag     17.02  0  1    4    4
Datsun 710        18.61  1  1    4    1
Hornet 4 Drive    19.44  1  0    3    1
Hornet Sportabout 17.02  0  0    3    2
Valiant           20.22  1  0    3    1
> dim(cars)
[1] 32 11
```

From here, we can input our data into the 3D function:

```r
scatterplot3d(cars[1:32,5:7 ])
```

There are 32 rows so we told the computer to read rows 1 - 32, and we want the computer to 
show variables `drat`, `wt`, and `qsec`, so we told it to read columnns 5 - 7. If the variables
you want to measure are not right next to each other, you can make a new dataframe with
the existing dataset. Say we wanted to create a graph using `mpg`, `hp`, and `wt`:

```r
# create dataframe
tab = data.frame(cars$mpg, cars$hp, cars$wt, cars$cyl)
# observe data
> dim(tab)
[1] 32  4
> head(tab)
  cars.mpg cars.hp cars.wt cars.cyl
1     21.0     110   2.620        6
2     21.0     110   2.875        6
3     22.8      93   2.320        4
4     21.4     110   3.215        6
5     18.7     175   3.440        8
6     18.1     105   3.460        6
```
Now, if we wanted to make a graph using these three variables, the code would simply be:

```r
# create 3D plot
scatterplot3d(cars[1:32, 1:3])
```

Back to our previous data, we now have a 3D plot.
The issue with the graph is that it looks like many black dots,
and we cannot really differentiate anything. We can measure another variable, `cyl`, by using color. First
we need to assign the colors we want to use a variable, and since our graph has 3 factors,
we need to use three colors.

```r
# assign a color variable
colors = c("red", "blue", "green")

# allow the computer to assign
# the colors to numeric variables by cylinder
colors <- as.numeric(cars$cyl)

# add colors into the graph
scatterplot3d(cars[1:32,5:7], 
            pch = 16, color = colors)
```

We can also make the graph interactive, using the `rgl` package, which we already have loaded.

First, we need to install the following software so that the computer can interpret the packages we import:

https://www.xquartz.org/


```r
plot3d(cars[1:32,5:7])
```

Now we have a black and white graph, but we need to get back our `cyl` variable, which was communicated through color.

```r
colors = c("red", "blue", "green")
colors <- as.numeric(cars$cyl)

# create graph
with(cars, plot3d(cars[1:32, 5:7],
                  type = "s",
                  col = as.integer(cars$cyl)))
```

The `with` function modifies data that already exists, so that is why we put the `plot3d` function within `with`

`type = "s"` s is solid, and this represents the circles that are on the graph

`col = as.integer(cars$cyl)))` This is setting the color equal to the integer we designated it to earlier.

# Citations

I used the DataDaft Youtube page for some tutorials, and I utilized the

https://www.r-graph-gallery.com/

website for many of my ggplot instructions.

I utilized datasets within R, and I used datasets from Opportunity Insights

https://opportunityinsights.org/data/



  
  




















