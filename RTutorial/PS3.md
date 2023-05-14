# Question 1

Endogeneity does not necessarily lead to bad prediction performance on a training set, or even a holdour test set, if the test set is very similar to the training set. The main problem is we are overfitting the data in such a situation. The main problem arises when we use such a model to predict something that does not resemble the training set. The main benefit of using a method such as SLS is that it can improve the generalizability of the model. In a more causal sense, if we use 2SLS, we try to solve the endogeneity problem. When doing this, we generally end up getting a worse fit on the training data. However, the model we get is more likely to work well on scenarios and datasets that are different than ours. 


# Qeustion 2


```R
library(data.table)
library(tidyverse)
library(dtplyr)
```

    -- [1mAttaching packages[22m --------------------------------------- tidyverse 1.3.2 --
    [32mv[39m [34mggplot2[39m 3.4.0      [32mv[39m [34mpurrr  [39m 1.0.1 
    [32mv[39m [34mtibble [39m 3.1.8      [32mv[39m [34mdplyr  [39m 1.0.10
    [32mv[39m [34mtidyr  [39m 1.2.1      [32mv[39m [34mstringr[39m 1.5.0 
    [32mv[39m [34mreadr  [39m 2.1.3      [32mv[39m [34mforcats[39m 0.5.2 
    -- [1mConflicts[22m ------------------------------------------ tidyverse_conflicts() --
    [31mx[39m [34mdplyr[39m::[32mbetween()[39m   masks [34mdata.table[39m::between()
    [31mx[39m [34mdplyr[39m::[32mfilter()[39m    masks [34mstats[39m::filter()
    [31mx[39m [34mdplyr[39m::[32mfirst()[39m     masks [34mdata.table[39m::first()
    [31mx[39m [34mdplyr[39m::[32mlag()[39m       masks [34mstats[39m::lag()
    [31mx[39m [34mdplyr[39m::[32mlast()[39m      masks [34mdata.table[39m::last()
    [31mx[39m [34mpurrr[39m::[32mtranspose()[39m masks [34mdata.table[39m::transpose()



```R
card_colnames <- c("id",        "nearc2",    "nearc4",    "educ",      "age",       "fatheduc",  "motheduc",  "weight",   
"momdad14",  "sinmom14",  "step14",    "reg661",    "reg662",    "reg663",    "reg664",    "reg665",   
"reg666",    "reg667",    "reg668",    "reg669",    "south66",   "black",     "smsa",      "south",    
"smsa66",    "wage",      "enroll",    "KWW",       "IQ",        "married",   "libcrd14",  "exper",    
"lwage",     "expersq")

card <- read_fwf("./CARD.raw", na = c(".","NA"))

colnames(card) <- card_colnames
```

    [1mRows: [22m[34m3010[39m [1mColumns: [22m[34m34[39m
    [36m--[39m [1mColumn specification[22m [36m--------------------------------------------------------[39m
    
    [31mchr[39m  (1): X8
    [32mdbl[39m (33): X1, X2, X3, X4, X5, X6, X7, X9, X10, X11, X12, X13, X14, X15, X16,...
    
    [36mi[39m Use `spec()` to retrieve the full column specification for this data.
    [36mi[39m Specify the column types or set `show_col_types = FALSE` to quiet this message.



```R
head(card)
```


<table class="dataframe">
<caption>A tibble: 6 × 34</caption>
<thead>
	<tr><th scope=col>id</th><th scope=col>nearc2</th><th scope=col>nearc4</th><th scope=col>educ</th><th scope=col>age</th><th scope=col>fatheduc</th><th scope=col>motheduc</th><th scope=col>weight</th><th scope=col>momdad14</th><th scope=col>sinmom14</th><th scope=col>⋯</th><th scope=col>smsa66</th><th scope=col>wage</th><th scope=col>enroll</th><th scope=col>KWW</th><th scope=col>IQ</th><th scope=col>married</th><th scope=col>libcrd14</th><th scope=col>exper</th><th scope=col>lwage</th><th scope=col>expersq</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2</td><td>0</td><td>0</td><td> 7</td><td>29</td><td>NA</td><td>NA</td><td>158413</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>548</td><td>0</td><td>15</td><td> NA</td><td>1</td><td>0</td><td>16</td><td>6.306275</td><td>256</td></tr>
	<tr><td>3</td><td>0</td><td>0</td><td>12</td><td>27</td><td> 8</td><td> 8</td><td>380166</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>481</td><td>0</td><td>35</td><td> 93</td><td>1</td><td>1</td><td> 9</td><td>6.175867</td><td> 81</td></tr>
	<tr><td>4</td><td>0</td><td>0</td><td>12</td><td>34</td><td>14</td><td>12</td><td>367470</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>721</td><td>0</td><td>42</td><td>103</td><td>1</td><td>1</td><td>16</td><td>6.580639</td><td>256</td></tr>
	<tr><td>5</td><td>1</td><td>1</td><td>11</td><td>27</td><td>11</td><td>12</td><td>380166</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>250</td><td>0</td><td>25</td><td> 88</td><td>1</td><td>1</td><td>10</td><td>5.521461</td><td>100</td></tr>
	<tr><td>6</td><td>1</td><td>1</td><td>12</td><td>34</td><td> 8</td><td> 7</td><td>367470</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>729</td><td>0</td><td>34</td><td>108</td><td>1</td><td>0</td><td>16</td><td>6.591674</td><td>256</td></tr>
	<tr><td>7</td><td>1</td><td>1</td><td>12</td><td>26</td><td> 9</td><td>12</td><td>380166</td><td>1</td><td>0</td><td>⋯</td><td>1</td><td>500</td><td>0</td><td>38</td><td> 85</td><td>1</td><td>1</td><td> 8</td><td>6.214608</td><td> 64</td></tr>
</tbody>
</table>



## a


```R
ols_card <- lm(
    log(wage) ~
        educ +
        reg661 +
        reg662 +
        reg663 +
        reg664 +
        reg665 +
        reg666 +
        reg667 +
        reg668 +
        black +
        smsa +
        south +
        smsa66 +
        exper +
        expersq,
    data = card
)


```

### homoskedastic


```R
summary(ols_card)$coefficients %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td> 4.74</td><td>0.07</td><td> 66.26</td><td>0.00</td></tr>
	<tr><th scope=row>educ</th><td> 0.07</td><td>0.00</td><td> 21.35</td><td>0.00</td></tr>
	<tr><th scope=row>reg661</th><td>-0.12</td><td>0.04</td><td> -3.05</td><td>0.00</td></tr>
	<tr><th scope=row>reg662</th><td>-0.02</td><td>0.03</td><td> -0.79</td><td>0.43</td></tr>
	<tr><th scope=row>reg663</th><td> 0.03</td><td>0.03</td><td>  0.95</td><td>0.34</td></tr>
	<tr><th scope=row>reg664</th><td>-0.06</td><td>0.04</td><td> -1.78</td><td>0.08</td></tr>
	<tr><th scope=row>reg665</th><td> 0.01</td><td>0.04</td><td>  0.26</td><td>0.79</td></tr>
	<tr><th scope=row>reg666</th><td> 0.02</td><td>0.04</td><td>  0.55</td><td>0.58</td></tr>
	<tr><th scope=row>reg667</th><td> 0.00</td><td>0.04</td><td> -0.01</td><td>0.99</td></tr>
	<tr><th scope=row>reg668</th><td>-0.18</td><td>0.05</td><td> -3.78</td><td>0.00</td></tr>
	<tr><th scope=row>black</th><td>-0.20</td><td>0.02</td><td>-10.91</td><td>0.00</td></tr>
	<tr><th scope=row>smsa</th><td> 0.14</td><td>0.02</td><td>  6.79</td><td>0.00</td></tr>
	<tr><th scope=row>south</th><td>-0.15</td><td>0.03</td><td> -5.69</td><td>0.00</td></tr>
	<tr><th scope=row>smsa66</th><td> 0.03</td><td>0.02</td><td>  1.35</td><td>0.18</td></tr>
	<tr><th scope=row>exper</th><td> 0.08</td><td>0.01</td><td> 12.81</td><td>0.00</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td>0.00</td><td> -7.22</td><td>0.00</td></tr>
</tbody>
</table>



### heteroskedasticity consistent


```R
library(lmtest)
library(sandwich)
coeftest(ols_card, vcov = vcovHC(old_card, type = "HC0"))[,] %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td> 4.74</td><td>0.07</td><td> 63.74</td><td>0.00</td></tr>
	<tr><th scope=row>educ</th><td> 0.07</td><td>0.00</td><td> 20.54</td><td>0.00</td></tr>
	<tr><th scope=row>reg661</th><td>-0.12</td><td>0.04</td><td> -3.07</td><td>0.00</td></tr>
	<tr><th scope=row>reg662</th><td>-0.02</td><td>0.03</td><td> -0.74</td><td>0.46</td></tr>
	<tr><th scope=row>reg663</th><td> 0.03</td><td>0.03</td><td>  0.91</td><td>0.36</td></tr>
	<tr><th scope=row>reg664</th><td>-0.06</td><td>0.04</td><td> -1.73</td><td>0.08</td></tr>
	<tr><th scope=row>reg665</th><td> 0.01</td><td>0.04</td><td>  0.24</td><td>0.81</td></tr>
	<tr><th scope=row>reg666</th><td> 0.02</td><td>0.04</td><td>  0.54</td><td>0.59</td></tr>
	<tr><th scope=row>reg667</th><td> 0.00</td><td>0.04</td><td> -0.01</td><td>0.99</td></tr>
	<tr><th scope=row>reg668</th><td>-0.18</td><td>0.05</td><td> -3.74</td><td>0.00</td></tr>
	<tr><th scope=row>black</th><td>-0.20</td><td>0.02</td><td>-10.99</td><td>0.00</td></tr>
	<tr><th scope=row>smsa</th><td> 0.14</td><td>0.02</td><td>  7.12</td><td>0.00</td></tr>
	<tr><th scope=row>south</th><td>-0.15</td><td>0.03</td><td> -5.29</td><td>0.00</td></tr>
	<tr><th scope=row>smsa66</th><td> 0.03</td><td>0.02</td><td>  1.42</td><td>0.16</td></tr>
	<tr><th scope=row>exper</th><td> 0.08</td><td>0.01</td><td> 12.59</td><td>0.00</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td>0.00</td><td> -7.18</td><td>0.00</td></tr>
</tbody>
</table>



### Explanation

There was no substantial difference between the two estimates. We would be better of using the heteroskedasticity consistent methods if there was a correlation between the variance of the error term and the predictors.

## b


```R
educ_card <- lm(
    educ ~
        nearc4 +
        reg661 +
        reg662 +
        reg663 +
        reg664 +
        reg665 +
        reg666 +
        reg667 +
        reg668 +
        black +
        smsa +
        south +
        smsa66 +
        exper +
        expersq,
    data = card
)


```

### homoskedastic


```R
summary(educ_card)$coefficients %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>16.85</td><td>0.21</td><td> 79.80</td><td>0.00</td></tr>
	<tr><th scope=row>nearc4</th><td> 0.32</td><td>0.09</td><td>  3.64</td><td>0.00</td></tr>
	<tr><th scope=row>reg661</th><td>-0.21</td><td>0.20</td><td> -1.04</td><td>0.30</td></tr>
	<tr><th scope=row>reg662</th><td>-0.29</td><td>0.15</td><td> -1.96</td><td>0.05</td></tr>
	<tr><th scope=row>reg663</th><td>-0.24</td><td>0.14</td><td> -1.67</td><td>0.10</td></tr>
	<tr><th scope=row>reg664</th><td>-0.09</td><td>0.19</td><td> -0.50</td><td>0.62</td></tr>
	<tr><th scope=row>reg665</th><td>-0.48</td><td>0.19</td><td> -2.57</td><td>0.01</td></tr>
	<tr><th scope=row>reg666</th><td>-0.51</td><td>0.21</td><td> -2.45</td><td>0.01</td></tr>
	<tr><th scope=row>reg667</th><td>-0.43</td><td>0.21</td><td> -2.08</td><td>0.04</td></tr>
	<tr><th scope=row>reg668</th><td> 0.31</td><td>0.24</td><td>  1.30</td><td>0.19</td></tr>
	<tr><th scope=row>black</th><td>-0.94</td><td>0.09</td><td> -9.98</td><td>0.00</td></tr>
	<tr><th scope=row>smsa</th><td> 0.40</td><td>0.10</td><td>  3.84</td><td>0.00</td></tr>
	<tr><th scope=row>south</th><td>-0.05</td><td>0.14</td><td> -0.38</td><td>0.70</td></tr>
	<tr><th scope=row>smsa66</th><td> 0.03</td><td>0.11</td><td>  0.24</td><td>0.81</td></tr>
	<tr><th scope=row>exper</th><td>-0.41</td><td>0.03</td><td>-12.24</td><td>0.00</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td>0.00</td><td>  0.53</td><td>0.60</td></tr>
</tbody>
</table>



### heteroskedasticity consistent


```R
coeftest(educ_card, vcov = vcovHC(educ_card, type = "HC0"))[,] %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>16.85</td><td>0.19</td><td> 90.55</td><td>0.00</td></tr>
	<tr><th scope=row>nearc4</th><td> 0.32</td><td>0.08</td><td>  3.77</td><td>0.00</td></tr>
	<tr><th scope=row>reg661</th><td>-0.21</td><td>0.20</td><td> -1.06</td><td>0.29</td></tr>
	<tr><th scope=row>reg662</th><td>-0.29</td><td>0.15</td><td> -1.91</td><td>0.06</td></tr>
	<tr><th scope=row>reg663</th><td>-0.24</td><td>0.14</td><td> -1.67</td><td>0.10</td></tr>
	<tr><th scope=row>reg664</th><td>-0.09</td><td>0.18</td><td> -0.52</td><td>0.60</td></tr>
	<tr><th scope=row>reg665</th><td>-0.48</td><td>0.19</td><td> -2.48</td><td>0.01</td></tr>
	<tr><th scope=row>reg666</th><td>-0.51</td><td>0.21</td><td> -2.46</td><td>0.01</td></tr>
	<tr><th scope=row>reg667</th><td>-0.43</td><td>0.21</td><td> -2.03</td><td>0.04</td></tr>
	<tr><th scope=row>reg668</th><td> 0.31</td><td>0.23</td><td>  1.35</td><td>0.18</td></tr>
	<tr><th scope=row>black</th><td>-0.94</td><td>0.09</td><td>-10.14</td><td>0.00</td></tr>
	<tr><th scope=row>smsa</th><td> 0.40</td><td>0.11</td><td>  3.63</td><td>0.00</td></tr>
	<tr><th scope=row>south</th><td>-0.05</td><td>0.14</td><td> -0.36</td><td>0.72</td></tr>
	<tr><th scope=row>smsa66</th><td> 0.03</td><td>0.11</td><td>  0.23</td><td>0.82</td></tr>
	<tr><th scope=row>exper</th><td>-0.41</td><td>0.03</td><td>-12.90</td><td>0.00</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td>0.00</td><td>  0.51</td><td>0.61</td></tr>
</tbody>
</table>



### explanation

Again, there was no sustantial difference between the homo and heteroskedasticity consistent resutls. We did find a statistically significant correlation beween nearc4 and educ. 

## c


```R
library(ivreg)

iv_card <- ivreg(
    log(wage) ~
    reg661 +
    reg662 +
    reg663 +
    reg664 +
    reg665 +
    reg666 +
    reg667 +
    reg668 +
    black +
    smsa +
    south +
    smsa66 +
    exper +
    expersq|
    educ | nearc4,
    data = card
)

```


```R
summary(iv_card, vcov = sandwich)
```


    
    Call:
    ivreg(formula = log(wage) ~ reg661 + reg662 + reg663 + reg664 + 
        reg665 + reg666 + reg667 + reg668 + black + smsa + south + 
        smsa66 + exper + expersq | educ | nearc4, data = card)
    
    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.83164 -0.24075  0.02429  0.25208  1.42760 
    
    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  3.7739653  0.9174053   4.114 4.00e-05 ***
    educ         0.1315038  0.0539995   2.435 0.014938 *  
    reg661      -0.1078142  0.0409668  -2.632 0.008538 ** 
    reg662      -0.0070465  0.0336994  -0.209 0.834387    
    reg663       0.0404445  0.0325208   1.244 0.213725    
    reg664      -0.0579171  0.0392106  -1.477 0.139759    
    reg665       0.0384576  0.0494675   0.777 0.436965    
    reg666       0.0550887  0.0521309   1.057 0.290716    
    reg667       0.0267580  0.0501066   0.534 0.593367    
    reg668      -0.1908912  0.0506897  -3.766 0.000169 ***
    black       -0.1467758  0.0523622  -2.803 0.005094 ** 
    smsa         0.1118083  0.0310619   3.600 0.000324 ***
    south       -0.1446715  0.0290653  -4.977 6.81e-07 ***
    smsa66       0.0185311  0.0205103   0.904 0.366333    
    exper        0.1082711  0.0233466   4.638 3.68e-06 ***
    expersq     -0.0023349  0.0003478  -6.713 2.27e-11 ***
    
    Diagnostic tests:
                      df1  df2 statistic  p-value    
    Weak instruments    1 2994    14.214 0.000166 ***
    Wu-Hausman          1 2993     1.219 0.269649    
    Sargan              0   NA        NA       NA    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 0.3883 on 2994 degrees of freedom
    Multiple R-Squared: 0.2382,	Adjusted R-squared: 0.2343 
    Wald test: 56.06 on 15 and 2994 DF,  p-value: < 2.2e-16 




```R
confint(iv_card, vcov = sandwich) %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 2 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>2.5 %</th><th scope=col>97.5 %</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td> 1.98</td><td> 5.57</td></tr>
	<tr><th scope=row>educ</th><td> 0.03</td><td> 0.24</td></tr>
	<tr><th scope=row>reg661</th><td>-0.19</td><td>-0.03</td></tr>
	<tr><th scope=row>reg662</th><td>-0.07</td><td> 0.06</td></tr>
	<tr><th scope=row>reg663</th><td>-0.02</td><td> 0.10</td></tr>
	<tr><th scope=row>reg664</th><td>-0.13</td><td> 0.02</td></tr>
	<tr><th scope=row>reg665</th><td>-0.06</td><td> 0.14</td></tr>
	<tr><th scope=row>reg666</th><td>-0.05</td><td> 0.16</td></tr>
	<tr><th scope=row>reg667</th><td>-0.07</td><td> 0.13</td></tr>
	<tr><th scope=row>reg668</th><td>-0.29</td><td>-0.09</td></tr>
	<tr><th scope=row>black</th><td>-0.25</td><td>-0.04</td></tr>
	<tr><th scope=row>smsa</th><td> 0.05</td><td> 0.17</td></tr>
	<tr><th scope=row>south</th><td>-0.20</td><td>-0.09</td></tr>
	<tr><th scope=row>smsa66</th><td>-0.02</td><td> 0.06</td></tr>
	<tr><th scope=row>exper</th><td> 0.06</td><td> 0.15</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td> 0.00</td></tr>
</tbody>
</table>




```R
confint(ols_card, vcov = sandwich) %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 16 × 2 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>2.5 %</th><th scope=col>97.5 %</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td> 4.60</td><td> 4.88</td></tr>
	<tr><th scope=row>educ</th><td> 0.07</td><td> 0.08</td></tr>
	<tr><th scope=row>reg661</th><td>-0.19</td><td>-0.04</td></tr>
	<tr><th scope=row>reg662</th><td>-0.08</td><td> 0.03</td></tr>
	<tr><th scope=row>reg663</th><td>-0.03</td><td> 0.08</td></tr>
	<tr><th scope=row>reg664</th><td>-0.13</td><td> 0.01</td></tr>
	<tr><th scope=row>reg665</th><td>-0.06</td><td> 0.08</td></tr>
	<tr><th scope=row>reg666</th><td>-0.06</td><td> 0.10</td></tr>
	<tr><th scope=row>reg667</th><td>-0.08</td><td> 0.08</td></tr>
	<tr><th scope=row>reg668</th><td>-0.27</td><td>-0.08</td></tr>
	<tr><th scope=row>black</th><td>-0.23</td><td>-0.16</td></tr>
	<tr><th scope=row>smsa</th><td> 0.10</td><td> 0.18</td></tr>
	<tr><th scope=row>south</th><td>-0.20</td><td>-0.10</td></tr>
	<tr><th scope=row>smsa66</th><td>-0.01</td><td> 0.06</td></tr>
	<tr><th scope=row>exper</th><td> 0.07</td><td> 0.10</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td> 0.00</td></tr>
</tbody>
</table>



### explanation

In the original regression the estimate was 0.07, but in the IV, the estimate was 0.13. This means that the endogeneity was negatively biasing our estimate. Both estimates are significantly non-zero. The confidence interval of IV is much wider.

## d


```R
educ_card <- lm(
    educ ~
        nearc4 +
        nearc2 +
        reg661 +
        reg662 +
        reg663 +
        reg664 +
        reg665 +
        reg666 +
        reg667 +
        reg668 +
        black +
        smsa +
        south +
        smsa66 +
        exper +
        expersq,
    data = card
)

```


```R
coeftest(educ_card, vcov = vcovHC(educ_card, type = "HC0"))[, ] %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 17 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>16.77</td><td>0.19</td><td> 86.69</td><td>0.00</td></tr>
	<tr><th scope=row>nearc4</th><td> 0.32</td><td>0.08</td><td>  3.78</td><td>0.00</td></tr>
	<tr><th scope=row>nearc2</th><td> 0.12</td><td>0.08</td><td>  1.59</td><td>0.11</td></tr>
	<tr><th scope=row>reg661</th><td>-0.17</td><td>0.20</td><td> -0.84</td><td>0.40</td></tr>
	<tr><th scope=row>reg662</th><td>-0.27</td><td>0.15</td><td> -1.77</td><td>0.08</td></tr>
	<tr><th scope=row>reg663</th><td>-0.19</td><td>0.15</td><td> -1.30</td><td>0.19</td></tr>
	<tr><th scope=row>reg664</th><td>-0.04</td><td>0.18</td><td> -0.21</td><td>0.84</td></tr>
	<tr><th scope=row>reg665</th><td>-0.44</td><td>0.20</td><td> -2.22</td><td>0.03</td></tr>
	<tr><th scope=row>reg666</th><td>-0.50</td><td>0.21</td><td> -2.40</td><td>0.02</td></tr>
	<tr><th scope=row>reg667</th><td>-0.38</td><td>0.21</td><td> -1.77</td><td>0.08</td></tr>
	<tr><th scope=row>reg668</th><td> 0.38</td><td>0.24</td><td>  1.61</td><td>0.11</td></tr>
	<tr><th scope=row>black</th><td>-0.95</td><td>0.09</td><td>-10.24</td><td>0.00</td></tr>
	<tr><th scope=row>smsa</th><td> 0.40</td><td>0.11</td><td>  3.62</td><td>0.00</td></tr>
	<tr><th scope=row>south</th><td>-0.04</td><td>0.14</td><td> -0.30</td><td>0.77</td></tr>
	<tr><th scope=row>smsa66</th><td> 0.00</td><td>0.11</td><td>  0.00</td><td>1.00</td></tr>
	<tr><th scope=row>exper</th><td>-0.41</td><td>0.03</td><td>-12.92</td><td>0.00</td></tr>
	<tr><th scope=row>expersq</th><td> 0.00</td><td>0.00</td><td>  0.50</td><td>0.62</td></tr>
</tbody>
</table>




```R
library(ivreg)

iv_card <- ivreg(
    log(wage) ~
        reg661 +
        reg662 +
        reg663 +
        reg664 +
        reg665 +
        reg666 +
        reg667 +
        reg668 +
        black +
        smsa +
        south +
        smsa66 +
        exper +
        expersq |
        educ | nearc4 + nearc2,
    data = card
)

```


```R
summary(iv_card, vcov = sandwich) 
```


    
    Call:
    ivreg(formula = log(wage) ~ reg661 + reg662 + reg663 + reg664 + 
        reg665 + reg666 + reg667 + reg668 + black + smsa + south + 
        smsa66 + exper + expersq | educ | nearc4 + nearc2, data = card)
    
    Residuals:
         Min       1Q   Median       3Q      Max 
    -1.93841 -0.25068  0.01932  0.26519  1.46998 
    
    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  3.3396868  0.8909170   3.749 0.000181 ***
    educ         0.1570594  0.0524127   2.997 0.002753 ** 
    reg661      -0.1029760  0.0425755  -2.419 0.015637 *  
    reg662      -0.0002287  0.0345230  -0.007 0.994716    
    reg663       0.0469556  0.0335252   1.401 0.161435    
    reg664      -0.0554084  0.0408927  -1.355 0.175529    
    reg665       0.0515041  0.0506274   1.017 0.309085    
    reg666       0.0699968  0.0534531   1.309 0.190466    
    reg667       0.0390596  0.0514309   0.759 0.447640    
    reg668      -0.1980371  0.0522335  -3.791 0.000153 ***
    black       -0.1232778  0.0514904  -2.394 0.016718 *  
    smsa         0.1007530  0.0313621   3.213 0.001329 ** 
    south       -0.1431945  0.0301873  -4.744 2.20e-06 ***
    smsa66       0.0150626  0.0211120   0.713 0.475616    
    exper        0.1188149  0.0228905   5.191 2.24e-07 ***
    expersq     -0.0023565  0.0003674  -6.414 1.64e-10 ***
    
    Diagnostic tests:
                      df1  df2 statistic  p-value    
    Weak instruments    2 2993     8.366 0.000238 ***
    Wu-Hausman          1 2993     2.978 0.084509 .  
    Sargan              1   NA     1.248 0.263905    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 0.4053 on 2994 degrees of freedom
    Multiple R-Squared: 0.1702,	Adjusted R-squared: 0.166 
    Wald test: 51.65 on 15 and 2994 DF,  p-value: < 2.2e-16 



### explanation
The results of the first stage model show that nearc2 has a weaker relationship with education compared to nearc4.
The results of the IV in this case leads to a stronger relationship between educ and wage, compared both to the OLS estimate and the IV with only nearc4.

## e


```R
iq_card <- lm(
    IQ ~
        nearc4,
    data = card
)

```


```R
coeftest(iq_card, vcov = vcovHC(iq_card, type = "HC0"))[, ] %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 2 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>100.61</td><td>0.63</td><td>158.99</td><td>0</td></tr>
	<tr><th scope=row>nearc4</th><td>  2.60</td><td>0.75</td><td>  3.47</td><td>0</td></tr>
</tbody>
</table>



### explanation
The result suggests that people living near a 4 year college tend to have a higher IQ.

## f


```R
IQ_multi_card <- lm(
    IQ ~
        nearc4 +
        reg661 +
        reg662 +
        reg669 +
        smsa66,
    data = card
)

```


```R
coeftest(IQ_multi_card, vcov = vcovHC(IQ_multi_card, type = "HC0"))[, ] %>% round(2)
```


<table class="dataframe">
<caption>A matrix: 6 × 4 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Estimate</th><th scope=col>Std. Error</th><th scope=col>t value</th><th scope=col>Pr(&gt;|t|)</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>99.38</td><td>0.71</td><td>139.50</td><td>0.00</td></tr>
	<tr><th scope=row>nearc4</th><td> 0.87</td><td>0.82</td><td>  1.06</td><td>0.29</td></tr>
	<tr><th scope=row>reg661</th><td> 4.77</td><td>1.42</td><td>  3.36</td><td>0.00</td></tr>
	<tr><th scope=row>reg662</th><td> 5.81</td><td>0.87</td><td>  6.70</td><td>0.00</td></tr>
	<tr><th scope=row>reg669</th><td> 1.84</td><td>1.14</td><td>  1.62</td><td>0.11</td></tr>
	<tr><th scope=row>smsa66</th><td> 1.35</td><td>0.79</td><td>  1.72</td><td>0.09</td></tr>
</tbody>
</table>



### explanation
nearc4 is not a statistically significant predictor anymore. This means that the results were counfounded by the added variables. In more practical words, this means that people living in these regions happened to also live near 4 year colleges, for example because these regions were bigger cities, and also people living in bigger cities happenned to have higher IQs.

# Question 3


```R
library(MASS)


simulator_xy <- function(n_sim = 1000 , beta = 1, pi_list = c(0, 0.25, 1, 2), n = 100){

    z <- rnorm(n, 0, 1)
    uv <- mvrnorm(n,
        mu = c(0, 0),
        Sigma = matrix(c(1, 0.9, 0.9, 1), nrow = 2)
    )
    u <- uv[, 1]
    v <- uv[, 2]

    result <- data.table()
    for (pi in pi_list){
        for (i in 1:n_sim) {
            z <- rnorm(n, 0, 1)
            uv <- mvrnorm(n,
                mu = c(0, 0),
                Sigma = matrix(c(1, 0.9, 0.9, 1), nrow = 2)
            )
            u <- uv[, 1]
            v <- uv[, 2]

            x <- z * pi + v 
            y <- x * beta + u
            
            stage_1_ols <- lm(x ~ z)
            x_pred <- predict(stage_1_ols)
            stage_2_iv <- lm(y ~ x_pred)

            stage_1_t_stat <- summary(stage_1_ols)$coefficients[2, 3]
            stage_1_p_value <- summary(stage_1_ols)$coefficients[2, 4]
            stage_2_t_stats <- summary(stage_2_iv)$coefficients[2, 3]
            stage_2_p_value <- summary(stage_2_iv)$coefficients[2, 4]
            stage_1_coef <- summary(stage_1_ols)$coefficients[2, 1]
            stage_2_coef <- summary(stage_2_iv)$coefficients[2, 1]
            stage_2_se <- summary(stage_2_iv)$coefficients[2, 2]

            result = rbind(result, data.table(
                pi = pi,
                stage_1_t_stat = stage_1_t_stat,
                stage_1_p_value = stage_1_p_value,
                stage_2_t_stats = stage_2_t_stats,
                stage_2_p_value = stage_2_p_value,
                stage_1_coef = stage_1_coef,
                stage_2_coef = stage_2_coef,
                stage_2_se = stage_2_se
            ))
        }
    }
    return(result)
}

result <- simulator_xy(n_sim = 1000 , beta = 1, pi_list = c(0, 0.25, 1, 2))
```


```R
head(result)
```


<table class="dataframe">
<caption>A data.table: 6 × 8</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>stage_1_t_stat</th><th scope=col>stage_1_p_value</th><th scope=col>stage_2_t_stats</th><th scope=col>stage_2_p_value</th><th scope=col>stage_1_coef</th><th scope=col>stage_2_coef</th><th scope=col>stage_2_se</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0</td><td> 0.09087531</td><td>0.9277772</td><td>-0.003753677</td><td>0.9970126</td><td> 0.008293969</td><td>-0.07919728</td><td>21.098584</td></tr>
	<tr><td>0</td><td>-0.84280314</td><td>0.4013908</td><td> 0.320131527</td><td>0.7495502</td><td>-0.071988366</td><td> 0.75781628</td><td> 2.367203</td></tr>
	<tr><td>0</td><td>-0.65351079</td><td>0.5149575</td><td> 0.453620313</td><td>0.6511050</td><td>-0.063640872</td><td> 1.34916572</td><td> 2.974218</td></tr>
	<tr><td>0</td><td> 0.04314344</td><td>0.9656750</td><td> 0.399395306</td><td>0.6904710</td><td> 0.004404761</td><td>17.91855190</td><td>44.864202</td></tr>
	<tr><td>0</td><td>-0.52971167</td><td>0.5975099</td><td> 0.636454292</td><td>0.5259641</td><td>-0.057790004</td><td> 2.27622429</td><td> 3.576414</td></tr>
	<tr><td>0</td><td>-0.07391714</td><td>0.9412270</td><td>-0.286444492</td><td>0.7751424</td><td>-0.006281663</td><td>-8.00766309</td><td>27.955375</td></tr>
</tbody>
</table>



## a


```R
result[, pi := factor(pi)]

# distribution of first stage t-stat grouped by pi
ggplot(data = result) +
    geom_histogram(data = result[pi == 0], aes(x = stage_1_t_stat, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 0.25], aes(x = stage_1_t_stat, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 1], aes(x = stage_1_t_stat, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 2], aes(x = stage_1_t_stat, fill=pi), bins = 75, alpha = 0.5) +
    theme_minimal() +
    labs(
        title = "Distribution of first stage t-stat",
        x = "t-stat",
        y = "Frequency"
    )
```


    
![png](PS3_files/PS3_43_0.png)
    



```R
# simulatiobn rejection frequency of pi = 0 in each case
result[, .(stage_1_reject = sum(abs(stage_1_t_stat) > 1.96)/1000), keyby = pi]
```


<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>stage_1_reject</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>0.056</td></tr>
	<tr><td>0.25</td><td>0.671</td></tr>
	<tr><td>1   </td><td>1.000</td></tr>
	<tr><td>2   </td><td>1.000</td></tr>
</tbody>
</table>



### explanation

In all cases, the T-stats are normally distributed, with higher pi values, we get higher means and higher variances for these distributions. When we use pi=0, we reject the null hypothesis around 5% of the time, as we would expect from a normal distribution with mean zero and variance 1. when we use pi=0.25 we reject it around 70% of the time. In cases of pi=1 or 2, we reject the null hypothesis nearly 100% of the time. 

## b


```R
# distribution of IV beta
ggplot() +
    geom_histogram(data = result[pi == 0], aes(x = stage_2_coef, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 0.25], aes(x = stage_2_coef, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 1], aes(x = stage_2_coef, fill=pi), bins = 75, alpha = 0.5) +
    geom_histogram(data = result[pi == 2], aes(x = stage_2_coef, fill=pi), bins = 75, alpha = 0.5) +
    theme_minimal() +
    labs(
        title = "Distribution of IV beta",
        x = "beta",
        y = "Frequency"
    ) +
    xlim(c(-1, 3))

```

    Warning message:
    “[1m[22mRemoved 178 rows containing non-finite values (`stat_bin()`).”
    Warning message:
    “[1m[22mRemoved 52 rows containing non-finite values (`stat_bin()`).”
    Warning message:
    “[1m[22mRemoved 1 rows containing missing values (`geom_bar()`).”
    Warning message:
    “[1m[22mRemoved 1 rows containing missing values (`geom_bar()`).”
    Warning message:
    “[1m[22mRemoved 1 rows containing missing values (`geom_bar()`).”
    Warning message:
    “[1m[22mRemoved 1 rows containing missing values (`geom_bar()`).”



    
![png](PS3_files/PS3_47_1.png)
    



```R
# finding 5% rejection of IV beta
result[, `:=`(beta_upper = stage_2_coef + 1.96 * stage_2_se, beta_lower = stage_2_coef - 1.96 * stage_2_se)]
result[, .(beta_equals_1_reject = 1 - mean(beta_lower < 1 & beta_upper > 1)), keyby = pi]
```


<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>beta_equals_1_reject</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>0.000</td></tr>
	<tr><td>0.25</td><td>0.001</td></tr>
	<tr><td>1   </td><td>0.001</td></tr>
	<tr><td>2   </td><td>0.000</td></tr>
</tbody>
</table>




```R
# mean of IV beta
result[, .(mean = mean(stage_2_coef)), keyby = pi]
#median of IV beta
result[, .(median = median(stage_2_coef)), keyby = pi]
```


<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>mean</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>1.7920847</td></tr>
	<tr><td>0.25</td><td>0.9359848</td></tr>
	<tr><td>1   </td><td>0.9852111</td></tr>
	<tr><td>2   </td><td>0.9990991</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>median</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>1.9049147</td></tr>
	<tr><td>0.25</td><td>0.9689817</td></tr>
	<tr><td>1   </td><td>0.9948681</td></tr>
	<tr><td>2   </td><td>1.0030035</td></tr>
</tbody>
</table>



### explanation
In all cases, we have nearly normal distributions. For pi = 0, we have a normal distribution with very fat tails, leading to the problem that despite our estiamtes being far, we cannot reject the equality of the estimate with 1. As we increase pi, the mean gets closer to the true value of beta = 1, and its variance decreases. In case of pi=0, the mean of the distribution is far from the true value. Using the asymptotic distribution, we nearly always fail to reject the H0 of beta=1. The means for each case get closer and closer to 1, as pi increases. However, the medians are closer in all cases (except when pi = 0, where we are basically fitting noise).

## c


```R
# only including cases where stage 1 is significant
stage_1_significat <- result[stage_1_p_value < 0.05]
stage_1_significat[, `:=`(beta_upper = stage_2_coef + 1.96 * stage_2_se, beta_lower = stage_2_coef - 1.96 * stage_2_se)]
stage_1_significat[, .(beta_equals_1_reject = 1-mean(beta_lower < 1 & beta_upper > 1)), keyby = pi]
stage_1_significat[, .(mean = mean(stage_2_coef)), keyby = pi]
stage_1_significat[, .(median = median(stage_2_coef)), keyby = pi]

```


<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>beta_equals_1</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>0.000000000</td></tr>
	<tr><td>0.25</td><td>0.001510574</td></tr>
	<tr><td>1   </td><td>0.001000000</td></tr>
	<tr><td>2   </td><td>0.000000000</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>mean</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>1.9280883</td></tr>
	<tr><td>0.25</td><td>1.0928021</td></tr>
	<tr><td>1   </td><td>0.9852111</td></tr>
	<tr><td>2   </td><td>0.9990991</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.table: 4 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>median</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0   </td><td>1.9270204</td></tr>
	<tr><td>0.25</td><td>1.1053759</td></tr>
	<tr><td>1   </td><td>0.9948681</td></tr>
	<tr><td>2   </td><td>1.0030035</td></tr>
</tbody>
</table>



### explanation
This makes the means farther away from the true value of beta = 1, however, the median for pi = 0.25 gets closer to the tru value of beta = 1.

## d


```R
# only including cases where stage 1 is significant
stage_1_f_significat <- result[stage_1_t_stat^2 > 10]
stage_1_f_significat[, `:=`(beta_upper = stage_2_coef + 1.96 * stage_2_se,
 beta_lower = stage_2_coef - 1.96 * stage_2_se)]
stage_1_f_significat[, .(beta_equals_1_reject = 1-mean(beta_lower < 1 & beta_upper > 1)), keyby = pi]
stage_1_f_significat[, .(mean = mean(stage_2_coef)), keyby = pi]
stage_1_f_significat[, .(median = median(stage_2_coef)), keyby = pi]

```


<table class="dataframe">
<caption>A data.table: 3 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>beta_equals_1_reject</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.25</td><td>0.004651163</td></tr>
	<tr><td>1   </td><td>0.001000000</td></tr>
	<tr><td>2   </td><td>0.000000000</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.table: 3 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>mean</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.25</td><td>1.2879864</td></tr>
	<tr><td>1   </td><td>0.9852111</td></tr>
	<tr><td>2   </td><td>0.9990991</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.table: 3 × 2</caption>
<thead>
	<tr><th scope=col>pi</th><th scope=col>median</th></tr>
	<tr><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.25</td><td>1.3077530</td></tr>
	<tr><td>1   </td><td>0.9948681</td></tr>
	<tr><td>2   </td><td>1.0030035</td></tr>
</tbody>
</table>



### explanation
This effectively excludes the cases where we have a pi of zero. For other values of pi, we get pretty good estimates, with higher pi values leading to better estimates.