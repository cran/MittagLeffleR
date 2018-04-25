
# MittagLeffleR

The MittagLeffleR R package

  - calculates probability densities, probabilities and quantiles, based
    on a  
    [Laplace-Inversion
    algorithm](https://au.mathworks.com/matlabcentral/fileexchange/48154-the-mittag-leffler-function)
    by [Roberto Garrappa](https://twitter.com/rgarrappa).
  - simulates random variables from both types Mittag-Leffler
    distributions
  - fits a Mittag-Leffler distribution to data, using the [log-moments
    estimator](http://doi.org/10.1080/03610918.2011.640094) by [Dexter
    Cahoy](https://www.uhd.edu/academics/sciences/mathematics-statistics/Pages/bio-cahoyd.aspx).

The first type Mittag-Leffler distribution is a heavy-tailed
distribution, and occurs mainly as a waiting time distribution in
problems with “fractional” time scales, e.g. times between earthquakes.

The second type Mittag-Leffler distribution is light-tailed, and
“inverse” to the sum-stable distributions. It typically models the
number of events in fractional systems and is used for time-changes of
stochastic processes, e.g. anomalous diffusion processes.

## Installation

### Stable release on CRAN

You can install MittagLeffleR from CRAN via

``` r
install.packages("MittagLeffleR")
library(MittagLeffleR)
```

### Development version on Github

Install the `devtools` package first, then

``` r
# install.packages("devtools")
devtools::install_github("strakaps/MittagLeffler")
library(MittagLeffleR)
```

## Usage

See the reference manual at
[strakaps.github.io/MittagLeffleR/reference/index.html](https://strakaps.github.io/MittagLeffleR/reference/index.html)

## Examples

### Fitting a Mittag-Leffler distribution

Generate a dataset first:

``` r
library(MittagLeffleR)
y = rml(n = 10000, tail = 0.9, scale = 2)
```

Fit the distribution:

``` r
logMomentEstimator(y, 0.95)
#>        nu     delta      nuLo      nuHi   deltaLo   deltaHi 
#> 0.8979629 2.0015598 0.8976156 0.8983103 1.9996012 2.0035184
```

Read off

  - the shape parameter \(0 < \nu < 1\),
  - the scale parameter \(\delta > 0\),
  - their 95% confidence intervals.

### Calculate the probability density of an anomalous diffusion process

Standard Brownian motion with drift \(1\) has, at time \(t\), has a
normal probability density \(n(x|\mu = t, \sigma^2 = t)\). A fractional
diffusion at time \(t\) has the time-changed probability density

\[p(x,t) = \int n(x| \mu = u, \sigma^2 = u)h(u,t) du\]

where \(h(u,t)\) is a second type Mittag-Leffler probability density
with scale \(t^\alpha\). (We assume \(t=1\).)

``` r
tail <- 0.65
dx <- 0.01
x <- seq(-2,5,dx)
umax <- qml(p = 0.99, tail = tail, scale = 1, second.type = TRUE)
u <- seq(0.01,umax,dx)
h <- dml(x = u, tail = tail, scale = 1, second.type = TRUE)
N <- outer(x,u,function(x,u){dnorm(x = x, mean = u, sd = sqrt(u))})
p <- N %*% h * dx
plot(x,p, type='l', main = "Fractional diffusion with drift at t=1")
```

## Vignettes

See the page
[strakaps.github.io/MittagLeffleR/articles/](https://strakaps.github.io/MittagLeffleR/articles/)
for vignettes on

  - Plots of the Mittag-Leffler distributions
  - Details of Mittag-Leffler random variate generation
  - Probabilities and Quantiles
