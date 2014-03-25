Simple Neural Network example and steps
========================================================
1. Setup and Training the Neural Network
--------------
### Get the neuralnet package

```r
install.packages("neuralnet")
```

```
## Installing package into 'C:/Users/A0121869X/R/win-library/3.0'
## (as 'lib' is unspecified)
```

```
## Error: trying to use CRAN without setting a mirror
```

```r
library("neuralnet")
```

```
## Warning: package 'neuralnet' was built under R version 3.0.3
```

```
## Loading required package: grid
## Loading required package: MASS
```


### Create the training data set to train the Neural Network. We want to create 2 columns with the Input and the Expected output.

```r
# Get 50 random numbers and put them into a dataframe
traindata <- as.data.frame(runif(50, min = 0, max = 100))

# Calculate the sqrt and then put that into a dataframe
trainexpectedoutput <- sqrt(traindata)

# Create the data frame to hold both values
trainingset <- cbind(traindata, trainexpectedoutput)

# Rename the columns
colnames(trainingset) <- c("Input", "Output")
```


### Now we are ready to train the Neural Network.

```r
# We train sqrtnn for deployment.
sqrtnn <- neuralnet(Output ~ Input, trainingset, hidden = 5, threshold = 0.01, 
    lifesign = "full")
```

```
## hidden: 5    thresh: 0.01    rep: 1/1    steps:    1000	min thresh: 0.08192956169
##                                                    2000	min thresh: 0.02967721728
##                                                    3000	min thresh: 0.02967721728
##                                                    4000	min thresh: 0.02967721728
##                                                    4989	error: 0.00385	time: 0.76 secs
```

```r
print(sqrtnn)
```

```
## Call: neuralnet(formula = Output ~ Input, data = trainingset, hidden = 5,     threshold = 0.01, lifesign = "full")
## 
## 1 repetition was calculated.
## 
##            Error Reached Threshold Steps
## 1 0.003851311193    0.007257232397  4989
```

```r
plot(sqrtnn)
```


### Training is complete. Now we test the NN

```r
# Create a set of numbers. (1:10)^2 creates a list of squared numbers.
# as.data.frame will convert the list into a dataframe
data <- as.data.frame((1:10)^2)
View(data)

# Here we use the NN that we trained to generate a set of results given the
# above data. compute, a method for objects of class nn, typically produced
# by neuralnet. Computes the outputs of all neurons for specific arbitrary
# covariate vectors given a trained neural network. Please make sure that
# the order of the covariates is the same in the new matrix or dataframe as
# in the original neural network.

net.result <- compute(sqrtnn, data)

print(net.result$net.result)
```

```
##              [,1]
##  [1,] 1.052249112
##  [2,] 1.991117959
##  [3,] 3.019778924
##  [4,] 3.992202616
##  [5,] 5.001269174
##  [6,] 5.991447117
##  [7,] 7.009109740
##  [8,] 7.993583973
##  [9,] 9.000243903
## [10,] 9.982359978
```

```r
print(sqrt(data))
```

```
##    (1:10)^2
## 1         1
## 2         2
## 3         3
## 4         4
## 5         5
## 6         6
## 7         7
## 8         8
## 9         9
## 10       10
```

```r

overview <- cbind(data, sqrt(data), net.result$net.result)
colnames(overview) <- c("Test Data", "Expected Output", "NN Output")
print(overview)
```

```
##    Test Data Expected Output   NN Output
## 1          1               1 1.052249112
## 2          4               2 1.991117959
## 3          9               3 3.019778924
## 4         16               4 3.992202616
## 5         25               5 5.001269174
## 6         36               6 5.991447117
## 7         49               7 7.009109740
## 8         64               8 7.993583973
## 9         81               9 9.000243903
## 10       100              10 9.982359978
```

