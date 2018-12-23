# R Tutorial


In R we general work with the following common packages:

    * dplyr, used to manipulate data
    * ggplot2, used to plot data

## Dealing with Data

Once we have loaded a dataset, we can explore it by executing:

```R
# if our dataset is called 'ds', we can do:
dim(ds)
# this will tell us number of rows and columns of dataset ds
```

We can get the column names by executing:
```R
names(ds)
```
To give a brief peek into our data we can use the `str` function:

```R
# str stands for "structure"
str(ds)
```

```R
# checks class of variable
class(ds)
```

```R
# To inspect functions and classes contained in a package, we can do:
ls(package:MASS)
```

To select a single column we can do:
```R
ds$colname
```

Understand the range of a column:
```R
range(present$year)
```

We can inspect head and tail of a dataset with:

```R
head(ds)
tail(ds, 18) # shows 18 elements from the tail
```

```R
x = c(4,2,45,10) # creates a 4 element vector
y = seq(1,12) # creates a 12 element vector
y = seq(1,100, by=4) # creates a 100 element vector, with elements skipped by 4 units
s = seq(from=4, length=3, by=3) # creates a 3 element vector
z = matrix(seq(1,12), 4,3) # creates a 4x3 matrix
```

```R
x = runif(50) # generates 50 points from a random uniform distribution
y = rnorm(50) # generates 50 points from a random normal distribution
x = rnorm(n, mean = 2, sd = 5)
```

```R
ls() # shows current variables in the environment
ls(package:MASS) # shows the functions and classes provided by the package called "MASS"
rm(var1) # removes var1 from the environment
```

```R
install.packages(c("MASS", "ggplot2")) # installs the mentioned packages
installed.packages() # shows the list of installed packages
```

```R
attach(ds) # now all the columns will be single variables, and we don't have to
# use the dollar syntax to refer to them
```

```R
cylinders = as.factor(cylinders) # convert a quantitative variable into a
# qualitative variable
```

### Basic Selection of data in basic R

```R
data(iris)

iris = ds

# Select only two columns
ds[,c('Sepal.Length','Sepal.Width')]

# Select only two columns but with rows satisfying a specific condition
ds[ds$Species == 'versicolor',c('Sepal.Length','Sepal.Width')]

# Takes the first 50 rows
ds[1:50,1:4]

# Takes the first 50 rows and the first 2 columns
ds[1:50,1:2]
```

We can select only numeric variables from a dataframe with:
```R
data(iris)
select_if(iris, is.numeric)
```

### Basic Data Type Transformation

```R
iris$Species = as.factor(iris$Species)
levels(iris$Species)
```

### Renaming Columns

We can rename a column by using the rename verb, for example:
```R
data(iris)

# In this case we rename the Sepal.Length column into
# "sepallengthnewname"
iris %>% rename(sepallengthnewname = "Sepal.Length")
```


### Creation of new columns with apply

```R
# m is our dataframe, here we create a new column which keeps the maximum
# between the columns V2 and V3
m$max = apply(m %>% select(c(V2, V3)), 1, max)

# here we do the same, but we compute the average
m$mean_v2_v3 = apply(m %>% select(c(V2, V3)), 1, mean)

# Here we apply the mean to all columns per row
m$max = apply(m, 1, mean)
```

### Basic Plotting

```R
plot(x,y) # a basic scatter plot
plot(x,y, xlab="X label", ylab="Y label", pch ="*", col="blue")

par(mfrow=c(2,1)) # par allows us to set certain options for plots, for example
                  # here we are stating that we want a 2 rows and 1 column plot
plot(x,y) # will plot in the first row first column of the plot
hist(y) # will plot in the second row first column of the plot
boxpllot(x) # plots a boxplot

plot(Sepal.Length~Sepal.Width, iris) # plots a scatter plot of sepal length as a
                                     # function of sepal width using the iris dataset

par(mfrow=c(1,1)) # go back to a single plot setting


hist(ds$Sepal.Length, col='red', breaks=40, main='Sepal Length Distribution', xlab='Sepal Length')

boxplot(ds$Sepal.Width, col='lightblue', main='Sepal Width Boxplot')

boxplot(Sepal.Length ~ Species, iris, col='lightblue', xlab='Species', main='Species vs Sepal Length Boxplot')

boxplot(Sepal.Length ~ Species, iris, col='lightblue', xlab='Species', main='Species vs Sepal Length Boxplot')
```

### Concatenating Stuff

```R
# Concatenating rows of a dataframe
a = rbind(dataframe1, dataframe2)
```

```R
# Concatenating columns of a dataframe
b = cbind(dataframe1, dataframe2)
# We can change names by doing

colnames(b) <- c("aa", "bb", "cc")

# or change specific columns with
colnames(b)[1] <- "abc"
```

### Merging Stuff

```R
data(iris)

ds1 = iris %>% select("Sepal.Length")
ds1 = cbind(ds1, c = seq(1,150))
ds2 = iris %>% select("Species")
ds2 = cbind(ds2, c = seq(1,150))

# Merge on common field name
merged_ds = merge(ds1, ds2, by='c')

ds1 = iris %>% select("Sepal.Length")
ds1 = cbind(ds1, colname = seq(1,150))
ds2 = iris %>% select("Species")
ds2 = cbind(ds2, newcol = seq(1,150))

# Merge on fields with different field name
merged_ds = merge(ds1, ds2, by.x='c', by.y='d')

merge(ds1, ds2, by='c')
```




## Summary Statistics


Summary statistics: Some useful function calls for summary statistics for a single numerical variable are as follows:

* summary
* mean
* median
* sd
* var
* IQR
* range
* min
* max
* n, which is the length of a vector
* n_distinct, which is the number of distinct values of a vector

```R
data(iris)
summary(iris)

# We can also put it in a nice looking dataframe with:

tmp <- do.call(data.frame, list(
    mean = apply(ds, 2, mean),
    sd = apply(ds, 2, sd),
    median = apply(ds, 2, median),
    min = apply(ds, 2, min),
    max = apply(ds, 2, max)
))
```


## Data Wrangling with dplyr

The dplyr package offers mainly seven verbs (functions) for basic data manipulation:

*   filter()
*   select()
*   arrange()
*   distinct()
*   mutate()
*   summarise()
*   sample_n()

Other than the extreme flexibility of these functions, dplyr inherit from another
package (called magrittr) the `pipe` operator `%>%`. This operator
allows to chain dplyr operations, allowing us to write shorter snippets of code
when transforming data.




### Basics: Selecting Columns and Rows

### Selecting Rows and Columns by indexes

In basic R without any external package we can select data by doing:

```R
iris[1]                 # selects the first column
iris[1,]                # selects the first row
iris[2,3]               # selects the element 2,3, that is, second row and third column value
iris[1:5,3]             # selects the rows from 1 to 5 in the third column
iris[1,3:5]             # selects the first row and shows values of columns from 3 to 5
iris[1:4,c(2,4)]        # selects rows from 1 to 4 and columns 2 and 4
iris[1:4,c(1,3:5)]      # selects rows from 1 to 4 and columns 1 and from 3 to 5
```

### Selecting with dplyr

We can select rows and columns in R by using the most basic dplyr commands 
which are `filter` and `select`.

#### Selecting Columns

```R
sleepData <- select(msleep, name, sleep_total) 
 # this selects from the dataframe msleep only
 # the columns name and sleep_total
```

We can also exclude columns by doing:
```R
select(msleep, -name) # we are excluding the column name
```

To select a range of columns by name, use the ":" (colon) operator

```R
head(select(msleep, col1name:col4name))
```

We can also select columns by using also other criteria, like:

   * starts_with() = Select all columns that start with a character string
   * ends_with()   = Select columns that end with a character string
   * contains()    = Select columns that contain a character string
   * matches()     = Select columns that match a regular expression
   * one_of()      = Select columns names that are from a group of names

for example:
```R
select(ds, starts_with("col_"))
```

#### Removing Columns

We can remove columns by using `select`.
```R
iris %>% select(-c("Species", "Sepal.Length"))
```

#### Selecting Rows

```R
filter(ds, colname1 >= 26)
```

We can also build more complex filters e.g.:
```R
filter(ds, colname1 >= 16, colname2 >= 1)
```

```R
filter(ds, country %in% c("Italy", "Romania"))
```

Let's do an example, using the pipe operator:
```R
msleep %>% 
    select(col1, col2) %>% 
    head
```

Let's see a combination of subsetting using both rows and columns:
```R
iris %>% select(c(Sepal.Length, Sepal.Width)) %>% filter(Sepal.Length > 6, Sepal.Width > 3)
```

### Arrange

This dplyr is used to order or reorder data, for example:

```R
ds %>% arrange(col1) %>% head # we are ordering data by the column called col1
```

```R
ds %>% arrange(desc(col1)) %>% head # we are ordering data by the column called col1 in descending order
```

Let's see a more complex example, where we first select
some of the columns, then we arrange first by a column
called `col1` and then with a descending order by the column
called `col2` and the we filter rows by imposinng a condition
on column `col3`.

```R
ds %>% 
    select(col1, col2, col3) %>%
    arrange(col1, desc(col2)) %>% 
    filter(col3 >= 16)
```

### Mutate

```R
ds %>% 
    mutate(col5= col2 / col1) %>%
    head
```

We can also create more columns:
```R
ds %>% 
    mutate(col5 = col2 / col1,
           col6 = col2 + col1) %>%
    head  
```

In this example we create a new column which will contain
the string "on time" if the dep_delay is less than 5 
in the other case we will set as string "delayed"

```R
ds %>% 
    mutate(dep_type = ifelse(dep_delay < 5, "on time", "delayed"))
```


### Summarise

Summarise is used to create summaries on specific columns.

Let's see some examples:
```R
msleep %>% 
    summarise(avg_sleep = mean(sleep_total), 
              min_sleep = min(sleep_total),
              max_sleep = max(sleep_total),
              total = n())
```

### Groupby

The group_by() verb implements the concept of "split-apply-combine".
This is generally used when we want split the data by some variable then apply a 
function to the individual data frames and then combine the output together.

In this example we split by order and then compute some statistics.
```R
msleep %>% 
    group_by(order) %>%
    summarise(avg_sleep = mean(sleep_total), 
    min_sleep = min(sleep_total), 
    max_sleep = max(sleep_total),
    total = n())
```


## Basic Plotting

```R
plot(iris) # gives the scatter matrix

boxplot(iris$Sepal.Length) # gives a boxplot

boxplot(Sepal.Length ~ Species, iris) # gives a set of boxplots organized by category "Species"

hist(iris$Sepal.Length) # gives a histogram

plot(iris$Sepal.Length) # gives a scatter plot, this is equivalent to type='p'

plot(iris$Sepal.Length, type='l') # gives a line plot

plot(iris$Sepal.Length, type='o') # gives a line plot but also mantaining dots

barplot(table(iris$Species)) # provides a bar plot
barplot(table(iris$Species), horiz=TRUE) # provides a bar plot

# Advanced stacked barplot
barplot(table(iris$Species, iris$Sepal.Width), col=c("darkblue", "red", "green"), legend = rownames(table(iris$Species, iris$Sepal.Width)))

plot(density(iris$Sepal.Length)) # plot a density plot, similar to kde, probably it is kde 

```

## Data Plotting with ggplot2


```R
# Plot a histogram
ggplot(housing, aes(x = col1)) +
  geom_histogram()
```

```R
# Plot a scatter plot
ggplot(data = ds, aes(x = col1, y = col2)) +
  geom_point()
```
```R
# Plot a line plot
ggplot(data = ds, aes(x = col1, y = col2)) +
  geom_line()
```
```R
# Plot a line plot with also points
ggplot(data = ds, aes(x = col1, y = col2)) +
  geom_line() + geom_point()
```

```R
# Plot a scatter plot selecting only specific data and a legend
ggplot(subset(housing, State %in% c("MA", "TX")),
       aes(x=col1,
           y=col2,
           color=State))+
  geom_point()
```

## Adding Columns

```R
arbuthnot <- arbuthnot %>%
  mutate(total = boys + girls)
```

```R
arbuthnot <- arbuthnot %>%
  mutate(more_boys = boys > girls)
```

## Playing with integrated datasets

We can use explore example datasets with the command:

```R
data()
```


## Regression

### Simple Linear Regression
```R
library(MASS)
library(ISLR)

fit1 = lm(medv~lstat, data=Boston)
summary(fit1)
plot(medv~lstat, Boston)
abline(fit1, col="red")
confint(fit1) # gives us the confidence intervals of the cofficient of the model

# Now we predict, by feeding the model with 3 values (5,10,15) and 
# inspect the prediction with its confidence interval
predict(fit1, data.frame(lstat=c(5,10,15), interval='confidence')
```
### Multiple Linear Regression

```R
library(MASS)
library(ISLR)

# Here we use as predictors lstat and age variables
fit2 = lm(medv~lstat+age, data=Boston)
summary(fit2)
```

Let's see an example where we use all the variables as predictors:
```R
library(MASS)
library(ISLR)

# We fit the model using all the variables except medv as predictors, and
# medv as response variable
fit3 = lm(medv~., Boston)

summary(fit3)
par(mfrow=c(2,2))
plot(fit3) # gives various plots about the linear regression model, some are quite advanced
par(mfrow=c(1,1))
```

#### Interaction Terms in Linear Regression

We can build an interaction model by doing:
```R
library(MASS)
library(ISLR)

fit1 = lm(medv~lstat*age, Boston)

# Now the model fit1 will be composed by 3 variables, that are, age, lstat and
# age * lstat
summary(fit1)


# We can include quadratic terms also by doing

fit2 = lm(medv~lstat +I(lstat^2), Boston); summary(fit2)
# In this case we should put the identity function "I()" since
# the character "^" have a special interpretation in the formula language
# in this case our model will have as predictors, lstat and lstat^2
summary(fit2)

# Plotting regression
plot(medv~lstat)
points(lstat, fitted(fit2), col="red", pch=20)

```

Another example with polynomial features:
```R
library(MASS)
library(ISLR)

plot(medv~lstat, Boston)
fit1 = lm(medv~poly(lstat,4), Boston)
points(Boston$lstat, fitted(fit7), col="red", pch=20)
```


Let's see how to add other interactions in a model
```R
# Here we add all the features and the interaction terms
# between Income and Advertising and interactions between Age and Price
fit1 = lm(Sales ~. + Income:Advertising + Age:Price, Carseats)

# We can show how R encodes categorical variables
contrasts(Carseats$ShelveLoc) 

```




#### Updating a model

```R
library(MASS)
library(ISLR)

# We fit the model using all the variables except medv as predictors, and
# medv as response variable
fit1 = lm(medv~., Boston)
fit2 = update(fit1, ~.-age-indus) # we take model 3 and update it by removing two predictors
summary(fit2)

# Plotting regression
plot(medv~lstat, Boston)
# we use points to plot since abline, can just plot lines
points(Boston$lstat, fitted(fit2), col="red", pch=20)
```


### Classification in R

```R

require(ISLR)
names(Smarket)
summary(Smarket)
?Smarket

pairs(Smarket, col = Smarket$Direction)

# Logistic Regression
glm.fit = glm(Direction ~ Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + Volume, data = Smarket, family=binomial)

summary(glm.fit)

# Obtain predictions probabilities
glm.probs = predict(glm.fit, type='response')

# Transform prediction probabilities into predictions by setting a threshold
glm.pred = ifelse(glm.probs > 0.5, "Up", "Down")
```


## Appendix A

### Fixing output of columns from R console
Sometimes R could break our output to a limited amount of columns, in order to
fix this, we can do:

```R
options(width=Sys.getenv("COLUMNS"))
```
