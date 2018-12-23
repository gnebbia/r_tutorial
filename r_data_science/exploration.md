# Exploration

Data exploration is the art of looking at your data, rapidly generating hypotheses,
quickly testing them, then repeating again and again and again.
The goal of data exploration is to generate many promising leads

The most common and famous library to perform visualization in R is ggplot2.
It implements a full grammar of graphics.

Let's use plotting to answer a simple question:
"Do cars with big engines use more fuel than cars with small engines?"
What is the relationship between engine size and fuel efficiency?
Positive or Negative? Linear or Nonlinear?

For the examples in this guide we will use the mpg dataset of cars contained
in the ggplot2 library.

We can inspect the dataset with:
```R
mpg
```
where `displ` is the car's engine size, in liters, while `hwy` is a car's
efficiency on the highway, in miles per gallon (mpg).

We can plot with:
```R
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy))
```

as we can see, the relation is negative, in other words, cars with big engines
use more fuel.

Basically ggplot(data = mpg), just creates an empty graph and loads the
specified data. Next we specify one or more layers, in our case `geom_point()`
adds a layer of points to our plot. This is done to create a scatterplot.

There are a bunch of geom_ functions in ggplot2, each one adds a different type
of layer.

Each geom function in ggplot2 takes a mapping argument; which defines how
variables in our dataset are mapped to visual properties.
Each mapping is always paired with `aes()` which specifies which variables are
mapped on the x and y axis.

So the graphing template is always:

```R
ggplot(data = <DATA>) + 
    <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>)
```

Let's see some variations of this template.
With ggplot we can use aesthetics in mappings to convey more information, for
example we can add colors for different classes in a dataset with:
```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy, color = class))
```
this will automatically create a legend.

We can also use size instead of the color, for example:
```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy, size = class))
```

let's see other examples of aesthetics, to map class to trasparency we can use:
```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy, alpha = class))
```

To map class to the shape of points we can instead do:
```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

We can also set the aesthetic properties of your geom manually. For example, we
can make all of the points in our plot blue:
```R
ggplot(data=mpg) + geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```
To set an aesthetic manually, we use it outside aes() but inside the geom function.

## Facets

One way too add additional variables is with aesthetics, but another way useful
in particular for categorical variables, is to split our plot into facets, which
are subplots which display one subset of the data.

To facet our plot by a single variable we can use `facet_wrap()`.
For example:

```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy)) +
facet_wrap(~ class, nrow = 2)
```
this shows the plots organized by category on two rows.

If we want to facet our plot on the combination of two variables, we can use
`facet_grid()`, for example:

```R
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy)) +
facet_grid(drv ~ cyl)
```



