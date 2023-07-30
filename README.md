# Draw fractals from root finding iteration in R

Keren Xu
04/23/2020

**The work in this repository was prepared for [LA R USER MEETUP](https://www.meetup.com/Los-Angeles-R-Users-Group-Data-Science/events/270120172/)**

**[Slides](https://xukeren.github.io/draw-fractals-from-root-finding-iteration-in-R/slides#1)**

**Recommended color palettes**

https://github.com/EmilHvitfeldt/r-color-palettes

https://github.com/karthik/wesanderson

**Loading packages**

```r
library(tidyverse)
library(purrr)
library(furrr)
library(RColorBrewer)
library(cartography)
library(Polychrome)
library(Cairo)
library(wesanderson)
```

# Create function newtonraphson

```r
# ftn is the name of a function that has two output including f(x) and f'(x)
# x0 is the starting point for the algorithm
# tol is a good stop condition when |f(x)| <= tol for the algorithm, the default here is 1e-9
# max.iter is a stop condition for the algorithm when n = max.itr

newtonraphson <- function(ftn, x0, tol = 1e-9, max.iter) {
  # initialize
  x <- x0
  fx <- ftn(x)
  iter <- 0

  # continue iterating until stopping conditions are met
  while((abs(fx[1]) > tol) && (iter < max.iter)) {
    x <- x - fx[1]/fx[2]
    fx <- ftn(x)
    iter <- iter + 1
    # cat("At iteration", iter, "value of x is:", x, "\n")
  }

  # output depends on the success of the algorithm
  if (abs(fx[1]) > tol){
    # cat("Algorithm failed to converge\n")
    return(data.frame(x0, root = NA, iter = NA))
  } else {
    # cat("Algorithm converged\n")
    return(data.frame(x0, root = x, iter))
  }

}
```

# Draw graph for `x^3-1`

```r
F1 <- function(x){
  return(c(x^3-1, 3*(x^2)))
}

# create complex numbers
x <- seq(-1, 1, length.out = 500)
y <- seq(-1, 1, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F1,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: ------------                                                     100% Progress: -----------------                                                100% Progress: -----------------------                                          100% Progress: -----------------------------                                    100% Progress: -----------------------------------                              100% Progress: -----------------------------------------                        100% Progress: ----------------------------------------------                   100% Progress: ---------------------------------------------------              100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE)+ scale_fill_gradientn(colors=brewer.pal(12, "Paired"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("turquoise.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-4.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Rushmore1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-5.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Royal1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-6.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Zissou1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-7.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-8.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-9.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("FantasticFox1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-10.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-11.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-12.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Cavalcanti1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-13.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("GrandBudapest1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-14.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("IsleofDogs1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-2-15.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-1` - Zoom out

```r
# create complex numbers
x <- seq(-10, 10, length.out = 500)
y <- seq(-10, 10, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F1,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: -----------                                                      100% Progress: -----------------                                                100% Progress: -----------------------                                          100% Progress: ----------------------------                                     100% Progress: ----------------------------------                               100% Progress: ----------------------------------------                         100% Progress: ---------------------------------------------                    100% Progress: ---------------------------------------------------              100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-3-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-3-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-3-4.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-1` - Zoom in

```r
# create complex numbers
x <- seq(-0.01, 0.01, length.out = 500)
y <- seq(-0.01, 0.01, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F1,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: ----------                                                       100% Progress: ---------------                                                  100% Progress: ---------------------                                            100% Progress: --------------------------                                       100% Progress: -------------------------------                                  100% Progress: -------------------------------------                            100% Progress: ------------------------------------------                       100% Progress: -----------------------------------------------                  100% Progress: ----------------------------------------------------             100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-4-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-4-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-4-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-4-4.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-1` - Move a bit to the left

```r
# create complex numbers
x <- seq(-2, 0, length.out = 500)
y <- seq(-1, 1, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F1,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: --------                                                         100% Progress: ------------                                                     100% Progress: ----------------                                                 100% Progress: ---------------------                                            100% Progress: ---------------------------                                      100% Progress: ----------------------------------                               100% Progress: ---------------------------------------                          100% Progress: ---------------------------------------------                    100% Progress: ---------------------------------------------------              100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-5-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-5-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-5-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-5-4.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-1` - Zoom in

```r
# create complex numbers
x <- seq(-1, -0.5, length.out = 500)
y <- seq(-0.25, 0.25, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F1,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: ------------                                                     100% Progress: -----------------                                                100% Progress: -----------------------                                          100% Progress: -----------------------------                                    100% Progress: -----------------------------------                              100% Progress: -----------------------------------------                        100% Progress: -----------------------------------------------                  100% Progress: ----------------------------------------------------             100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-6-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-6-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-6-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-6-4.png" style="display: block; margin: auto;" />

# Draw graph for `x^5-1`

```r
F2 <- function(x){
  return(c(x^5-1,5*(x^4)))
}

# create complex numbers
x <- seq(-5, 5, length.out = 500)
y <- seq(-5, 5, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)

df <- z %>% future_map_dfr(~newtonraphson(F2,.,1e-9, 40), .progress = TRUE)
```

    ##  Progress: -----------                                                      100% Progress: ----------------                                                 100% Progress: ----------------------                                           100% Progress: ----------------------------                                     100% Progress: ---------------------------------                                100% Progress: ---------------------------------------                          100% Progress: --------------------------------------------                     100% Progress: --------------------------------------------------               100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE)+ scale_fill_gradientn(colors=brewer.pal(12, "Paired"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("turquoise.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-4.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Rushmore1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-5.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Royal1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-6.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Zissou1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-7.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-8.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-9.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("FantasticFox1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-10.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-11.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-12.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Cavalcanti1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-13.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("GrandBudapest1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-14.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("IsleofDogs1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-7-15.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-0.64310316*x-0.35689684`

```r
F3 <- function(x){
  return(c(x^3-0.64310316*x-0.35689684, 3*x^2-0.64310316))
}

x <- seq(-5, 5, length.out = 500)
y <- seq(-5, 5, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F3,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: -----------                                                      100% Progress: -----------------                                                100% Progress: ----------------------                                           100% Progress: ----------------------------                                     100% Progress: ----------------------------------                               100% Progress: ----------------------------------------                         100% Progress: ----------------------------------------------                   100% Progress: ----------------------------------------------------             100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration

df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE)+ scale_fill_gradientn(colors=brewer.pal(12, "Paired"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("turquoise.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-4.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Rushmore1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-5.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Royal1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-6.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Zissou1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-7.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-8.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-9.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("FantasticFox1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-10.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-11.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-12.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Cavalcanti1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-13.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("GrandBudapest1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-14.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("IsleofDogs1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-8-15.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-0.64310316*x-0.35689684`

```r
F3 <- function(x){
  return(c(x^3-0.64310316*x-0.35689684, 3*x^2-0.64310316))
}

x <- seq(-1.5, 1.5, length.out = 500)
y <- seq(-1, 1, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F3,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: --------------                                                   100% Progress: --------------------                                             100% Progress: ----------------------------                                     100% Progress: -----------------------------------                              100% Progress: ------------------------------------------                       100% Progress: -------------------------------------------------                100% Progress: -----------------------------------------------------            100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-9-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-9-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-9-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-9-4.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-0.64310316*x-0.35689684`

```r
F3 <- function(x){
  return(c(x^3-0.64310316*x-0.35689684, 3*x^2-0.64310316))
}

x <- seq(-0.5, 0.5, length.out = 500)
y <- seq(-0.5, 0.5, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F3,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: -------------                                                    100% Progress: --------------------                                             100% Progress: ---------------------------                                      100% Progress: ----------------------------------                               100% Progress: -----------------------------------------                        100% Progress: ------------------------------------------------                 100% Progress: ------------------------------------------------------           100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration

df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2"), na.value = 'black') + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-10-1.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-0.64310316*x-0.35689684`

```r
F3 <- function(x){
  return(c(x^3-0.64310316*x-0.35689684, 3*x^2-0.64310316))
}

x <- seq(-0.05, 0.05, length.out = 500)
y <- seq(-0.03, 0.07, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F3,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: -------------                                                    100% Progress: -------------------                                              100% Progress: --------------------------                                       100% Progress: --------------------------------                                 100% Progress: ---------------------------------------                          100% Progress: ----------------------------------------------                   100% Progress: ----------------------------------------------------             100% Progress: -------------------------------------------------------          100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration

df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2"), na.value = 'black') + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-11-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"), na.value = 'black') + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-11-2.png" style="display: block; margin: auto;" />

# Draw graph for `x^3-0.64310316*x-0.35689684`

```r
F3 <- function(x){
  return(c(x^3-0.64310316*x-0.35689684, 3*x^2-0.64310316))
}

x <- seq(-0.15, 0.15, length.out = 500)
y <- seq(-0.15, 0.15, length.out = 500)
z <- outer(x, 1i*y,'+')

# parallel processing using furrr
plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F3,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: --------------                                                   100% Progress: --------------------                                             100% Progress: ---------------------------                                      100% Progress: ----------------------------------                               100% Progress: -----------------------------------------                        100% Progress: -----------------------------------------------                  100% Progress: -----------------------------------------------------            100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration

df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-12-1.png" style="display: block; margin: auto;" />

# Draw graph for `(exp(x)+exp(‑x))/2 - 1`

```r
F5 <- function(x){
  return(c((exp(x)+exp(‑x))/2 - 1, (exp(x) - exp(-x))/2))
}


x <- seq(-4, 4, length.out = 1000)
y <- seq(2, 4, length.out = 500)
z <- outer(x, 1i*y,'+')

plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F5,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: ------                                                           100% Progress: ---------                                                        100% Progress: -------------                                                    100% Progress: ----------------                                                 100% Progress: --------------------                                             100% Progress: -----------------------                                          100% Progress: --------------------------                                       100% Progress: ------------------------------                                   100% Progress: ---------------------------------                                100% Progress: ------------------------------------                             100% Progress: ----------------------------------------                         100% Progress: -------------------------------------------                      100% Progress: -----------------------------------------------                  100% Progress: --------------------------------------------------               100% Progress: -----------------------------------------------------            100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE)+ scale_fill_gradientn(colors=brewer.pal(12, "Paired"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("turquoise.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-4.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Rushmore1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-5.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Royal1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-6.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Zissou1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-7.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-8.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-9.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("FantasticFox1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-10.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-11.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-12.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Cavalcanti1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-13.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("GrandBudapest1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-14.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("IsleofDogs1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-13-15.png" style="display: block; margin: auto;" />

# Draw graph for `x^8 + 15*x^4 - 16`

```r
F6 <- function(x){
  return(c(x^8 + 15*x^4 - 16, 8*x^7 + 60*x^3))
}


x <- seq(-5, 5, length.out = 1000)
y <- seq(-5, 5, length.out = 500)
z <- outer(x, 1i*y,'+')

plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F6,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: -------                                                          100% Progress: ----------                                                       100% Progress: -------------                                                    100% Progress: -----------------                                                100% Progress: --------------------                                             100% Progress: ------------------------                                         100% Progress: ---------------------------                                      100% Progress: ------------------------------                                   100% Progress: ----------------------------------                               100% Progress: -------------------------------------                            100% Progress: -----------------------------------------                        100% Progress: --------------------------------------------                     100% Progress: -----------------------------------------------                  100% Progress: --------------------------------------------------               100% Progress: ------------------------------------------------------           100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE)+ scale_fill_gradientn(colors=brewer.pal(12, "Paired"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("turquoise.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-4.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Rushmore1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-5.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Royal1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-6.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Zissou1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-7.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-8.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Darjeeling2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-9.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("FantasticFox1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-10.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-11.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-12.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Cavalcanti1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-13.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("GrandBudapest1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-14.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("IsleofDogs1")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-14-15.png" style="display: block; margin: auto;" />

# Draw graph for `sin(x) - 1`

```r
F7 <- function(x){
  return(c(sin(x) - 1, cos(x)))
}

x <- seq(-4, 1, length.out = 500)
y <- seq(-3, 3, length.out = 500)
z <- outer(x, 1i*y,'+')


plan(multiprocess)
df <- z %>% future_map_dfr(~newtonraphson(F7,.,1e-3, 40), .progress = TRUE)
```

    ##  Progress: ---------------                                                  100% Progress: ----------------------                                           100% Progress: -----------------------------                                    100% Progress: ------------------------------------                             100% Progress: --------------------------------------------                     100% Progress: ---------------------------------------------------              100% Progress: -----------------------------------------------------            100% Progress: ---------------------------------------------------------------- 100%

```r
df$x <- Re(df$x0)
df$y <- Im(df$x0)

# color by iteration

df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-15-1.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("Moonrise3")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-15-2.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = wes_palette("BottleRocket2")) + theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-15-3.png" style="display: block; margin: auto;" />

```r
df %>% ggplot(aes(x = x, y = y)) + geom_raster(aes(fill = iter), interpolate = TRUE) + scale_fill_gradientn(colors = carto.pal("multi.pal"))+ theme_void() + theme(legend.position = "none")
```

<img src="README_files/figure-gfm/unnamed-chunk-15-4.png" style="display: block; margin: auto;" />

# Secant method

```r
secant <- function(ftn, x0, x1, tol = 1e-9, max.iter) {
  # initialize
  x_n0 <- x0
  x_n1 <- x1
  ftn_n0 <- ftn(x_n0)
  ftn_n1 <- ftn(x_n1)
  iter <- 0

  # continue iterating until stopping conditions are met
  while((abs(ftn_n1) > tol) && (iter < max.iter)) {
    x_n2 <- x_n1 - ftn_n1*(x_n1 - x_n0)/(ftn_n1 - ftn_n0)
    x_n0 <- x_n1
    ftn_n0 <- ftn(x_n0)
    x_n1 <- x_n2
    ftn_n1 <- ftn(x_n1)
    iter <- iter + 1
    # cat("At iteration", iter, "value of x is:", x_n1, "\n")
  }

  return(c(x_n1, iter))
}
```
