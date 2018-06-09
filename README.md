# LERCA

Local Exposure Response Confounding Adjustment R package.

## Installing LERCA

Installing and using LERCA in Rstudio is straightforward. You will first need the ```devtools``` R package.

```
install.packages('devtools')
library(devtools)
devtools::install_github("gpapadog/DAPSm")
```

## LERCA example

### Generating data

In order to generate data with local confounding you can use the ```SimDifferentialConfounding``` function. Below is the code that one could use to generate the toyData used throughout the examples on this R package.

This specific data set has three true experiments with four potential confounders. In experiment 1, C<sub>1</sub> is a confounder, in experiment 2, C<sub>1</sub> and C<sub>3</sub> are confounders, and in experiment 3, C<sub>3</sub> is a confounder.

```
library(LERCA)
set.seed(1234)

N <- 400  # Sample size.
num_exper <- 3  # Number of experiments.
num_conf <- 4  # Number of potential confounders.

# Correlation of covariates with the exposure in each experiment.
XCcorr <- matrix(NA, nrow = num_conf, ncol = num_exper)
XCcorr[1, ] <- c(0.3, 0.2, 0)
XCcorr[2, ] <- c(0.3, 0, 0)
XCcorr[3, ] <- c(0, 0.2, 0.4)
XCcorr[4, ] <- c(0, 0, 0)

# Covariates' outcome model coefficients.
out_coef <- matrix(NA, nrow = num_conf, ncol = num_exper)
out_coef[1, ] <- c(0.5, 0.6, 0)
out_coef[2, ] <- c(0, 0, 0)
out_coef[3, ] <- c(0, 0.6, 0.4)
out_coef[4, ] <- c(0.3, 0.3, 0.3)

varC <- rep(1, num_conf)

# Exposure range and true experiment configuration.
Xrange <- c(0, 6)
exper_change <- c(0, 2, 4, 6)

# True coefficients of exposure in each experiment.
bYX <- c(0, 2, 1)

toyData <- SimDifferentialConfounding(N = N, num_exper = num_exper,
                                      XCcorr = XCcorr, out_coef = out_coef,
                                      varC = varC, Xrange = Xrange,
                                      exper_change = exper_change, bYX = bYX)
toyData <- toyData$data
```

## Functions

- BurnThin: Function that performs burning and thinning of the MCMC chains.

- GenYgivenXC: Function that generates the outcome given exposure and covariates.

- GetbYvalues: Getting the intercepts of the outcome models for each experiment ensuring a continuous ER.

- GetER_1chain: Using the posterior samples of one chain to acquire samples of the mean ER.

- GetER: Calling GetER_1chain for each chain to aquire posterior samples of the mean ER for all chains.

- JumpOver: Performing the jump over move for the update of the experiment configuration and inclusion indicators.

- JumpWithin: Performing the jump within move for the update of the experiment configuration and inclusion indicators.

- **LERCA**: The central function of the package. Function to fit LERCA to the data.

- LogLike: Function that calculates the log likelihood of the data.

- MakeArrays: Function that creates arrays where the LERCA results are saved.

- psr: Function that calculates the PSR based on the mean ER posterior samples.

- SimDifferentialConfounding: Function that generates data with differential confounding.

- TrueER: Function that calculates the true mean ER.

- UpdateAlphas: Function that updates the inclusion indicators.

- UpdateCoefficients: Function that updates all coefficients but intercepts of the outcome model and the coefficient of exposure in the outcome model.

- UpdateExperiments: Function that updates the experiment configuration when the update is separate from the update of the inclusion indicators.

- UpdateVariances: Function that updates the residual variances.

- WAIC: Function that calculates the WAIC of the LERCA fit.

- XCcontinuous: Function that ensures that the mean exposure-covariates relationship is continuous.


