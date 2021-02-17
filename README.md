### **NAME : KOMAL WANI**

##### **TASK 1 : PREDICTION USING SUPERVISED ML**

##### **PROBLEM - PREDICT THE PERCENTAGE OF A STUDENT BASED ON THE NO. OF STUDY HOURS. WHAT WILL BE PREDICTED SCORE IF A STUDENT STUDIES FOR 9.25 HOURS/DAY ?**

### **IMPORTING THE DATA**

```
> data = read.delim(file.choose(),header=T)

> dim(data)

[1] 25  2
```

###### Data has 25 observations and 2 variables

```
> head(data,5)
```

|    | Hours | Scores|
|----|----   |-----  |
|1   | 2.5   | 21    |
|    |       |       |
|2   | 5.1   | 47    |
|    |       |       |
|3   | 3.2   | 27    |
|    |       |       |
|4   | 8.5   | 75    |
|    |       |       |
|5   | 3.5   | 30    |

###### First 5 observations of the data.

#### Independent variable = x = Hours

#### Dependent variable = y = Scores
 
### **CORRELATION BETWEEN HOURS OF STUDY AND SCORES**

```
> cor(data$Hours,data$Scores)

[1] 0.976190
```

###### High positive correlation between two variables.
 
### **FOR SIMPLE LINEAR REGRESSION**

### **SPLITTING DATA INTO TRAIN AND TEST DATA**

```
> smp_siz = floor(0.75*nrow(data))

> smp_siz  

[1] 18
```

###### Gives us the sample size for train data.

```
> set.seed(123)

> train_ind = sample(seq_len(nrow(data)),size=smp_siz)

> train_ind

[1] 15 19 14  3 10 18 11  5 23  6  9 21 24 20 22 25 17  1

> train = data[train_ind,]

> train
```

|  |Hours |Scores|
|--|----  |----- |
|15| 1.1  |  17  |
|  |      |      |
|19| 6.1  |  67  |
|  |      |      |
|14| 3.3  |  42  |
|  |      |      |
|3 | 3.2  |  27  |
|  |      |      |
|10| 2.7  |  25  |
|  |      |      |
|18| 1.9  |  24  |
|  |      |      |
|11| 7.7  |  85  |
|  |      |      |
|5 | 3.5  |  30  |
|  |      |      |
|23| 3.8  |  35  |
|  |      |      |
|6 | 1.5  |  20  |
|  |      |      |
|9 | 8.3  |  81  |
|  |      |      |
|21| 2.7  |  30  |
|  |      |      |
|24| 6.9  |  76  |
|  |      |      |
|20| 7.4  |  69  |
|  |      |      |
|22| 4.8  |  54  |
|  |      |      |
|25| 7.8  |  86  |
|  |      |      |
|17| 2.5  |  30  |
|  |      |      |
|1 | 2.5  |  21  |

```
> dim(train)

[1] 18  2
```

###### Train data has 18 observations.

```
> test = data[-train_ind,]

> test
```

|  | Hours  | Scores|
|--|-----   |-----  |
|2 |   5.1  |   47  |
|  |        |       |
|4 |   8.5  |   75  |
|  |        |       |
|7 |   9.2  |   88  | 
|  |        |       |
|8 |   5.5  |   60  |
|  |        |       |
|12|   5.9  |   62  |
|  |        |       |
|13|   4.5  |   41  |
|  |        |       |
|16|   8.9  |   95  |

```
> dim(test)

[1] 7 2
```

###### Test data has 7 observations.
 
### **SIMPLE LINEAR REGRESSION MODEL ON TRAIN DATA**

```
> x = train$Hours

> x

[1] 1.1 6.1 3.3 3.2 2.7 1.9 7.7 3.5 3.8 1.5 8.3 2.7 6.9 7.4 4.8 7.8 2.5 2.5

> y = train$Scores

> y

[1] 17 67 42 27 25 24 85 30 35 20 81 30 76 69 54 86 30 21
```

> model_train = lm(y~x,data=train)

> model_train

Call:

lm(formula = y ~ x, data = train)

Coefficients:

|(Intercept)|  x    |
|--------   |-----  |
|  1.612    | 10.167|  

summary(model_train) 

Call:

lm(formula = y ~ x, data = train)

Residuals:

|  Min |   1Q  |  Median |  3Q  |  Max  | 
|----  |---    |----     |---   |---    |
|-7.848| -5.185|  3.020  | 4.049|  6.836| 

Coefficients:

                         Estimate Std.                Error                   t value                Pr(>|t|)

     Intercept             1.6125                    2.6477                   0.609                  0.551    
                                                                                                     
         x                10.1670                    0.5395                  18.847                 2.39e-12 ***

 Signif. codes:     0 ‘***’      0.001 ‘**’       0.01 ‘*’      0.05 ‘.’       0.1 ‘ ’       1

Residual standard error: 5.346 on 16 degrees of freedom

 Multiple R-squared:  0.9569,            Adjusted R-squared:  0.9542 

 F-statistic: 355.2 on 1 and 16 DF,       p-value: 2.386e-12


###### p-value < 0.05 = Model is statistically significant, F-statistic is also high.

###### R-squared = 0.95 which tells that the model is well fit. And the model explains 95% of the total data variation.
 
### **USING ABOVE MODEL FOR PREDICTING SCORES ON TEST DATA**

```
> predict_y = predict(model_train,newdata=data.frame(x = c(5.1,8.5,9.2,5.5,5.9,4.5,8.9)))

> predict_y
```

   1    |    2    |    3    |    4    |    5    |    6    |    7 
----    |-----    |-----    |---      |---      |---      |---
53.46415| 88.03194| 95.14884| 57.53095| 61.59775| 47.36395| 92.09874 


### **ACTUAL VS PREDICTED SCORES**

```
> df = data.frame(test,predict_y)

> names(df) = c("Hours","Actual Scores","Predicted Scores")

> df
```

|  | Hours |  Actual Scores| Predicted Scores |
|--|-----  |--------       |-------           |
|2 |   5.1 |           47  |          53.46415|
|  |       |               |                  |
|4 |   8.5 |           75  |          88.03194|
|  |       |               |                  |
|7 |   9.2 |           88  |          95.14884|
|  |       |               |                  |
|8 |   5.5 |           60  |          57.53095|
|  |       |               |                  |
|12|   5.9 |           62  |          61.59775|
|  |       |               |                  |
|13|   4.5 |           41  |          47.36395|
|  |       |               |                  |
|16|   8.9 |           95  |          92.09874|
 
### PREDICTING SCORE IF A STUDENT STUDIES FOR 9.25 HOURS/DAY

```
> y_pred = predict(model_train,newdata=data.frame(x=9.25))

> y_pred

   1 

95.65719
```

###### Predicted score if a student studies for 9.25 hours/day is 95.66%

