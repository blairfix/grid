# grid

`grid` is an R function that takes an input vector and organizes the elements as squares on a 2D plane. This is useful for visualizing a size distribution. 

### Inputs

The `grid` function has two inputs


* `input_vec` = the vector you want to turn into a 2D grid


* `stretch` = a parameter that determines the relative size of the x-axis relative to the y-axis. Try `stretch = 2` and go from there. If you get the error message `Mat::operator(): index out of bounds`, choose a larger value for `stretch`.


### Output
`grid` returns a 5-column matrix:

* `column 1` = index of the input elements
* `column 2` = xmin of rectangle
* `column 3` = xmax of rectangle
* `column 4` = ymin of rectangle
* `column 5` = ymax of rectangle

### Example

Here is code that visualizes a truncated normal distribution.

```R
library(RcppArmadillo)
library(Rcpp)
library(ggplot2)
library(truncnorm)

sourceCpp("grid.cpp")


x# normal distribution
x = rtruncnorm(10000, mean = 40, sd = 40, a = 0, b = Inf)

# turn elements into rectangles
coords = grid(x, stretch = 2.5) 

# plot
coords = data.frame(coords)

grid_plot_norm = ggplot() +
  geom_rect(data = coords, aes(xmin = X2, xmax = X3, ymin = X4, ymax = X5)) +
  theme_void()

```

Here's the resulting plot:

![truncnorm](https://economicsfromthetopdown.files.wordpress.com/2020/04/truncnorm.png)

Now let's visualize a lognormal distribution.



```R
x = rlnorm(10000, 1, 2.5)

# turn elements into rectangles
coords = grid(x, stretch = 2.5) 

# plot
coords = data.frame(coords)

grid_plot_lnorm = ggplot() +
  geom_rect(data = coords, aes(xmin = X2, xmax = X3, ymin = X4, ymax = X5)) +
  theme_void()

```

Here's the resulting plot:
![lognormal](https://economicsfromthetopdown.files.wordpress.com/2020/04/lnorm.png)




### Installation
To use `grid`, install the following R packages:
 * [Rcpp](https://cran.r-grid.org/web/packages/Rcpp/index.html) 
 * [RcppArmadillo](https://cran.r-grid.org/web/packages/RcppArmadillo/index.html) 

Put the source code (`grid.cpp`) in the directory of your R script. Then source it with the command `sourceCpp('grid.cpp')`.



