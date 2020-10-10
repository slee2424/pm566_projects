Midterm Project - Diabetes Indicators
================

# Introduction

  - Diabetes is a complex metabolic syndrome and its involvement in
    cardiovascular disease is manifold with varying manifestations and
    different clinical symptoms and prognosis. Although being probably
    the most important risk factor, diabetes is often considered an
    “accompanying comorbidity” for cardiac or peripheral artery
    disease, hypertension, or stroke. An interesting study demonstrated
    that the risk of sudden cardiac death is at least 2-fold higher in
    patients who are diabetic compared with those who are nondiabetic,
    regardless of the extent of cardiac dysfunction or symptoms of heart
    failure. In contrast, the risk of nonsudden cardiac death was not
    signiﬁcantly different between the 2 groups.

  - Preventing sudden cardiac death by analyzing diabetes is worth
    trying as it is the leading cause of death in the United States. The
    object of this study is to find out if diabetes risk prediction is
    possible based on family history and genetic factors along with
    other risk factors. This study is focused on whether Diabetes
    Pedigree Function is truly associated with outcome and if so, what
    risk factors are significantly related with it. The association
    between Diabetes Pedigree Function and disease status would reveal
    to what extent does the genetic factors affects diabetes. The
    dataset did not separate type 1 diabetes and type 2 dabetes.
    Regardless of the type of diabetes, however, Diabetes Pedigree
    Function can be assessed as family history is one of the important
    risk factors for both type of diabetes. In the analysis, important
    indicators of diabetes will be evaluated together.

# Methods

  - Linear regression was performed for the association between Diabetes
    Pedigree Function and outcome, including the examination of the
    effect of lipotoxicity, glucose level, Blood Pressure, Skin
    Thickness, insulin level, BMI, and age. The dataset was downloaded
    from Haverd Dataverse and the reference is stated below. The data
    was cleaned by replacing extreme values to ’NA’s, shortening
    variable names, and creating new factor variable for better
    analysis. Skim function from skimr package was used to explore data.
    Dim, head and tail, summary, and table functions were used to check
    detailed observations.

  - Reference: P. Rodgers, Judith, 2020, “Diabetes Mellitus
    “Comorbidity” or an Important Risk Factor?",
    <https://doi.org/10.7910/DVN/JAW6AX>, Harvard Dataverse, V2,
    UNF:6:QOwrJ53n2F5fMC+wb4ADVA== \[fileUNF\]

  - Key question: Whether diabetes risk factors affects the
    outcome.

<!-- end list -->

``` r
library(tidyverse)
```

    ## -- Attaching packages ------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## √ ggplot2 3.3.2     √ purrr   0.3.4
    ## √ tibble  3.0.3     √ dplyr   1.0.2
    ## √ tidyr   1.1.1     √ stringr 1.4.0
    ## √ readr   1.3.1     √ forcats 0.5.0

    ## -- Conflicts ---------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(skimr)
library(olsrr)
```

    ## 
    ## Attaching package: 'olsrr'

    ## The following object is masked from 'package:datasets':
    ## 
    ##     rivers

# Data description

  - Lipotoxicity: lipotoxicity is a metabolic syndrome that results from
    the accumulation of lipid intermediates in non-adipose tissue,
    leading to cellular dysfunction and death.

  - Glucose: blood glucose level is the concentration of glucose present
    in the blood.

  - BloodPressure: the pressure of the blood in the circulatory system.

  - SkinThickness: skin thickness is primarily determined by collagen
    content and is increased in insulin-dependent diabetes mellitus
    (IDDM).

  - Insulin: insulin is an anabolic hormone that promotes glucose
    uptake.

  - BMI: body mass index (BMI) is a person’s weight in kilograms divided
    by the square of height in meters.

  - DiabetesPedigreeFunction: diabetes pedigree function provides “a
    synthesis of the diabetes mellitus history in relatives and the
    genetic relationship of those relatives to the subject.” It utilizes
    information from a person’s family history to predict how diabetes
    will affect that individual.

  - Age: age of the individual.

  - Outcome: diabetes diagosis.

<!-- end list -->

``` r
diabetes <- read_csv("data/diabetes.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Lipotoxicity = col_double(),
    ##   Glucose = col_double(),
    ##   BloodPressure = col_double(),
    ##   SkinThickness = col_double(),
    ##   Insulin = col_double(),
    ##   BMI = col_double(),
    ##   DiabetesPedigreeFunction = col_double(),
    ##   Age = col_double(),
    ##   Outcome = col_double()
    ## )

# Exploratory Data Analysis

  - The total dataset includes 768 observations with 9 vaiables.

  - Lipotoxicity is right skewed and ranged from 0 to 17 with mean of
    3.85.

  - Glucose level is not skewed to either side and ranged from 0 to 199
    with mean of 121.

  - Blood Pressure is also not skewed and ranged from 0 to 122 with mean
    of 69.1.

  - Skin Thickness right skewed and ranged from 0 to 99.

  - Insulin level is extremly right skewed and ranged from 0 to 846.

  - BMI is fairly normally distributed and ranged from 0 to 67.1.

  - Diabetes Pedigree Function is right skewed and ranged from 0.078 to
    2.42.

  - Age is right skewed and ranged from 21 to 81. Right skewed age data
    displays more than 50% of observations are in the age group of 20 to
    29. This data is more focused on young population.

  - Outcome has binary results with mean of 0.349 which means 34.9% of
    all observations have diabetes positive outcome.

<!-- end list -->

``` r
skim(diabetes)
```

|                                                  |          |
| :----------------------------------------------- | :------- |
| Name                                             | diabetes |
| Number of rows                                   | 768      |
| Number of columns                                | 9        |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |          |
| Column type frequency:                           |          |
| numeric                                          | 9        |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |          |
| Group variables                                  | None     |

Data summary

**Variable type:
numeric**

| skim\_variable           | n\_missing | complete\_rate |   mean |     sd |    p0 |   p25 |    p50 |    p75 |   p100 | hist  |
| :----------------------- | ---------: | -------------: | -----: | -----: | ----: | ----: | -----: | -----: | -----: | :---- |
| Lipotoxicity             |          0 |              1 |   3.85 |   3.37 |  0.00 |  1.00 |   3.00 |   6.00 |  17.00 | ▇▃▂▁▁ |
| Glucose                  |          0 |              1 | 120.89 |  31.97 |  0.00 | 99.00 | 117.00 | 140.25 | 199.00 | ▁▁▇▆▂ |
| BloodPressure            |          0 |              1 |  69.11 |  19.36 |  0.00 | 62.00 |  72.00 |  80.00 | 122.00 | ▁▁▇▇▁ |
| SkinThickness            |          0 |              1 |  20.54 |  15.95 |  0.00 |  0.00 |  23.00 |  32.00 |  99.00 | ▇▇▂▁▁ |
| Insulin                  |          0 |              1 |  79.80 | 115.24 |  0.00 |  0.00 |  30.50 | 127.25 | 846.00 | ▇▁▁▁▁ |
| BMI                      |          0 |              1 |  31.99 |   7.88 |  0.00 | 27.30 |  32.00 |  36.60 |  67.10 | ▁▃▇▂▁ |
| DiabetesPedigreeFunction |          0 |              1 |   0.47 |   0.33 |  0.08 |  0.24 |   0.37 |   0.63 |   2.42 | ▇▃▁▁▁ |
| Age                      |          0 |              1 |  33.24 |  11.76 | 21.00 | 24.00 |  29.00 |  41.00 |  81.00 | ▇▃▁▁▁ |
| Outcome                  |          0 |              1 |   0.35 |   0.48 |  0.00 |  0.00 |   0.00 |   1.00 |   1.00 | ▇▁▁▁▅ |

  - 0 values in all risk factors are speculated as an absence of the
    specific test outcome because there were significant different in
    values between all 0s and the next minimum values of each variable.
    All of them are edited to “NA”s.

<!-- end list -->

``` r
db <-
  diabetes %>%
  mutate(Lipotoxicity = replace(Lipotoxicity, Lipotoxicity==0, NA))%>%
  mutate(Glucose = replace(Glucose, Glucose==0, NA)) %>%
  mutate(BloodPressure = replace(BloodPressure, BloodPressure==0, NA)) %>%
  mutate(SkinThickness = replace(SkinThickness, SkinThickness==0, NA)) %>%
  mutate(Insulin = replace(Insulin, Insulin==0, NA)) %>%
  mutate(BMI = replace(BMI, BMI==0, NA))
```

  - Variable names are renamed into lowercase letters with shorter
    length.

<!-- end list -->

``` r
db <- 
  db %>%
  rename(
    lip = Lipotoxicity,
    glu = Glucose,
    bp = BloodPressure,
    st = SkinThickness,
    ins = Insulin,
    bmi = BMI,
    dpf = DiabetesPedigreeFunction,
    age = Age,
    outcome = Outcome
    )
```

  - Create age groups to compare proportion of the diabetes by age
    groups.

<!-- end list -->

``` r
db$age_group <- ifelse(db$age < 30, "20-29", 
                         ifelse(db$age >= 30 & db$age < 40, "30-39",
                                ifelse(db$age >= 40 & db$age < 50, "40-49", "50+")))
```

``` r
table(diabetes$Lipotoxicity)
table(db$Lipotoxicity)

table(diabetes$BloodPressure)
table(db$BloodPressure)

table(diabetes$SkinThickness)
table(db$SkinThickness)

table(diabetes$Insulin)
table(db$Insulin)

table(diabetes$BMI)
table(db$BMI)

table(diabetes$DiabetesPedigreeFunction)
table(db$BMI)
```

# Preliminary results (regression model & data visualization)

### Disease outcome by age\_groups.

  - Age is significant risk factor of diabetes

<!-- end list -->

``` r
db <-
  db %>%
  mutate(outcome_f = factor(outcome))

db %>%
  ggplot()+
  geom_bar(mapping = aes(x = age_group, fill = outcome_f, stat="identity"))+
  labs(title = "Diabetes outcome by age groups")
```

    ## Warning: Ignoring unknown aesthetics: stat

![](Midterm_project_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
model_agegroup <- lm(outcome ~ age_group, data=db)
summary(model_agegroup)
```

    ## 
    ## Call:
    ## lm(formula = outcome ~ age_group, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -0.5508 -0.2121 -0.2121  0.5169  0.7879 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     0.21212    0.02290   9.265  < 2e-16 ***
    ## age_group30-39  0.24848    0.04222   5.886 5.93e-09 ***
    ## age_group40-49  0.33873    0.04778   7.089 3.09e-12 ***
    ## age_group50+    0.27102    0.05345   5.071 4.97e-07 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4556 on 764 degrees of freedom
    ## Multiple R-squared:  0.09104,    Adjusted R-squared:  0.08747 
    ## F-statistic: 25.51 on 3 and 764 DF,  p-value: 9.863e-16

### Preliminary regression model

  - linear regression and multiple regression was performed.
  - No significant colliniarity was detected by examining variace
    inflation factor.
  - Model 2 should be used for analysis as it has greater R-squared
    value while remaining statistical significance (p \< 0.05).

<!-- end list -->

``` r
model1 <- lm(outcome ~ dpf, data=db)

summary(model1)
```

    ## 
    ## Call:
    ## lm(formula = outcome ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -0.8137 -0.3375 -0.2849  0.5963  0.7471 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.23087    0.02953   7.819 1.76e-14 ***
    ## dpf          0.25025    0.05122   4.886 1.25e-06 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.47 on 766 degrees of freedom
    ## Multiple R-squared:  0.03022,    Adjusted R-squared:  0.02896 
    ## F-statistic: 23.87 on 1 and 766 DF,  p-value: 1.255e-06

``` r
model1 %>%
  ggplot(mapping = aes(y=outcome, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
model2 <- lm(outcome ~ dpf + lip + glu + bp + st + ins + bmi + age, data=db)

summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = outcome ~ dpf + lip + glu + bp + st + ins + bmi + 
    ##     age, data = db)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.10824 -0.26481 -0.03969  0.27008  1.05149 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -1.169e+00  1.620e-01  -7.214 3.81e-12 ***
    ## dpf          1.544e-01  6.529e-02   2.365   0.0186 *  
    ## lip          1.126e-02  9.101e-03   1.238   0.2167    
    ## glu          5.904e-03  8.887e-04   6.644 1.28e-10 ***
    ## bp           1.183e-03  1.879e-03   0.630   0.5293    
    ## st           2.039e-03  2.698e-03   0.756   0.4504    
    ## ins          7.306e-07  2.243e-04   0.003   0.9974    
    ## bmi          9.089e-03  4.414e-03   2.059   0.0403 *  
    ## age          6.858e-03  2.896e-03   2.368   0.0185 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.3827 on 327 degrees of freedom
    ##   (432 observations deleted due to missingness)
    ## Multiple R-squared:  0.3557, Adjusted R-squared:  0.3399 
    ## F-statistic: 22.57 on 8 and 327 DF,  p-value: < 2.2e-16

``` r
model2 %>%
  ggplot(mapping = aes(y=outcome, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

``` r
model2 %>% ols_vif_tol()
```

    ##   Variables Tolerance      VIF
    ## 1       dpf 0.9550433 1.047073
    ## 2       lip 0.5324636 1.878063
    ## 3       glu 0.5840286 1.712245
    ## 4        bp 0.8105058 1.233798
    ## 5        st 0.5716614 1.749287
    ## 6       ins 0.6161025 1.623107
    ## 7       bmi 0.5532702 1.807435
    ## 8       age 0.4765168 2.098562

  - Check difference between each risk factors by disease status (1:
    diagnosed diabetes, 0: No diabetes): all indicators in diabetes
    group has higher values.

<!-- end list -->

``` r
db_ot <- 
  db %>% 
  group_by(outcome)
  
db_table <-
  db_ot %>%
  summarise(
    DiabetesPedigreeFunction=mean(dpf),
    Lipotoxicity=mean(lip, na.rm = TRUE),
    Glucose=mean(glu, na.rm = TRUE),
    BloodPressur=mean(bp, na.rm = TRUE),
    SkinThickness=mean(st, na.rm = TRUE),
    Insulin=mean(ins, na.rm = TRUE),
    BMI=mean(bmi, na.rm = TRUE),
    Age=mean(age, na.rm = TRUE),
    )
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
knitr::kable(db_table, align= "c", digits = 4, caption = "Mean of each risk factors by outcome group")
```

| outcome | DiabetesPedigreeFunction | Lipotoxicity | Glucose  | BloodPressur | SkinThickness | Insulin  |   BMI   |   Age   |
| :-----: | :----------------------: | :----------: | :------: | :----------: | :-----------: | :------: | :-----: | :-----: |
|    0    |          0.4297          |    3.8618    | 110.6439 |   70.8773    |    27.2355    | 130.2879 | 30.8597 | 31.1900 |
|    1    |          0.5505          |    5.6696    | 142.3195 |   75.3214    |    33.0000    | 206.8462 | 35.4068 | 37.0672 |

Mean of each risk factors by outcome
group

#### regression model for the association between dpf and other risk factors.

  - Lipotoxicity vs Diabetes Pedigree Function: no association (p \>
    0.05).

<!-- end list -->

``` r
model_lip <- lm(lip ~ dpf, data=db)
summary(model_lip)
```

    ## 
    ## Call:
    ## lm(formula = lip ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.5195 -2.5084 -0.5098  2.4856 12.5287 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.52537    0.22418  20.186   <2e-16 ***
    ## dpf         -0.06621    0.40053  -0.165    0.869    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.22 on 655 degrees of freedom
    ##   (111 observations deleted due to missingness)
    ## Multiple R-squared:  4.172e-05,  Adjusted R-squared:  -0.001485 
    ## F-statistic: 0.02733 on 1 and 655 DF,  p-value: 0.8688

``` r
model_lip %>%
  ggplot(mapping = aes(y=lip, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

  - Glucose level vs Diabetes Pedigree Function: The association is
    statistically significant (p \< 0.001).

<!-- end list -->

``` r
model_glu <- lm(glu ~ dpf, data=db)
summary(model_glu)
```

    ## 
    ## Call:
    ## lm(formula = glu ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -79.133 -22.119  -4.673  18.984  79.283 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  115.722      1.907  60.689  < 2e-16 ***
    ## dpf           12.625      3.303   3.822 0.000143 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 30.27 on 761 degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## Multiple R-squared:  0.01884,    Adjusted R-squared:  0.01755 
    ## F-statistic: 14.61 on 1 and 761 DF,  p-value: 0.000143

``` r
model_glu %>%
  ggplot(mapping = aes(y=glu, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

  - Blood Pressure vs Diabetes Pedigree Function: No association (p \>
    0.05).

<!-- end list -->

``` r
model_bp <- lm(bp ~ dpf, data=db)
summary(model_bp)
```

    ## 
    ## Call:
    ## lm(formula = bp ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -48.397  -8.407  -0.400   7.587  49.567 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  72.4546     0.7967  90.944   <2e-16 ***
    ## dpf          -0.1039     1.3704  -0.076     0.94    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.39 on 731 degrees of freedom
    ##   (35 observations deleted due to missingness)
    ## Multiple R-squared:  7.865e-06,  Adjusted R-squared:  -0.00136 
    ## F-statistic: 0.00575 on 1 and 731 DF,  p-value: 0.9396

``` r
model_bp %>%
  ggplot(mapping = aes(y=bp, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

  - Skin Thickness vs Diabetes Pedigree Function: The association is
    statistically significant (p \< 0.01).

<!-- end list -->

``` r
model_st <- lm(st ~ dpf, data=db)
summary(model_st)
```

    ## 
    ## Call:
    ## lm(formula = st ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -23.617  -7.809  -0.206   7.020  69.603 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  27.3984     0.7917  34.606  < 2e-16 ***
    ## dpf           3.4763     1.2932   2.688  0.00741 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 10.42 on 539 degrees of freedom
    ##   (227 observations deleted due to missingness)
    ## Multiple R-squared:  0.01323,    Adjusted R-squared:  0.0114 
    ## F-statistic: 7.226 on 1 and 539 DF,  p-value: 0.007408

``` r
model_st %>%
  ggplot(mapping = aes(y=st, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

  - Insulin level vs Diabetes Pedigree Function: The association is
    statistically significant (p \< 0.01).

<!-- end list -->

``` r
model_ins <- lm(ins ~ dpf, data=db)
summary(model_ins)
```

    ## 
    ## Call:
    ## lm(formula = ins ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -225.35  -76.23  -29.48   33.46  696.09 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   132.30      10.72  12.338  < 2e-16 ***
    ## dpf            44.23      16.99   2.604  0.00957 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 117.9 on 392 degrees of freedom
    ##   (374 observations deleted due to missingness)
    ## Multiple R-squared:  0.017,  Adjusted R-squared:  0.0145 
    ## F-statistic:  6.78 on 1 and 392 DF,  p-value: 0.009567

``` r
model_ins %>%
  ggplot(mapping = aes(y=ins, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

  - BMI vs Diabetes Pedigree Function: The association is statistically
    significant (p \< 0.001).

<!-- end list -->

``` r
model_bmi <- lm(bmi ~ dpf, data=db)
summary(model_bmi)
```

    ## 
    ## Call:
    ## lm(formula = bmi ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -14.752  -4.987  -0.484   4.156  35.144 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  30.9145     0.4351  71.045  < 2e-16 ***
    ## dpf           3.2659     0.7556   4.322 1.75e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 6.845 on 755 degrees of freedom
    ##   (11 observations deleted due to missingness)
    ## Multiple R-squared:  0.02414,    Adjusted R-squared:  0.02285 
    ## F-statistic: 18.68 on 1 and 755 DF,  p-value: 1.754e-05

``` r
model_bmi %>%
  ggplot(mapping = aes(y=bmi, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

  - Age vs Diabetes Pedigree Function: No association (p \> 0.05).

<!-- end list -->

``` r
model_age <- lm(age ~ dpf, data=db)
summary(model_age)
```

    ## 
    ## Call:
    ## lm(formula = age ~ dpf, data = db)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -13.741  -9.279  -4.085   7.436  47.773 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  32.6788     0.7389  44.228   <2e-16 ***
    ## dpf           1.1912     1.2817   0.929    0.353    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.76 on 766 degrees of freedom
    ## Multiple R-squared:  0.001126,   Adjusted R-squared:  -0.0001777 
    ## F-statistic: 0.8638 on 1 and 766 DF,  p-value: 0.353

``` r
model_age %>%
  ggplot(mapping = aes(y=age, x=dpf))+
  geom_point()+
  geom_smooth(method=lm)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](Midterm_project_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

### Mean value differences of each risk factors that has association with Diabetes Pedigree Function for high DPF vs low DPF.

``` r
db_dpf <- 
  db %>% 
  group_by(dpf > mean(dpf))

  
db_table2 <-
  db_dpf %>%
  summarise(
    DiabetesPedigreeFunction=mean(dpf),
    Glucose=mean(glu, na.rm = TRUE),
    SkinThickness=mean(st, na.rm = TRUE),
    Insulin=mean(ins, na.rm = TRUE),
    BMI=mean(bmi, na.rm = TRUE)
    )
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
knitr::kable(db_table2, align= "c", digits = 4, caption = "Mean of each risk factors by DPF level")
```

| dpf \> mean(dpf) | DiabetesPedigreeFunction | Glucose  | SkinThickness | Insulin  |   BMI   |
| :--------------: | :----------------------: | :------: | :-----------: | :------: | :-----: |
|      FALSE       |          0.2711          | 119.9275 |    28.5871    | 146.2714 | 31.8477 |
|       TRUE       |          0.7938          | 124.4932 |    29.9134    | 166.1359 | 33.4284 |

Mean of each risk factors by DPF level

# Conclusion

  - According to CDC, the list of diabetes risk factors are: family
    history, age, prediabetes, overweight, physical inactivity, and
    race. The fact that age is the most significant risk factor of
    diabetes is well-known. Based on the analysis performed, age is
    proved to be a significant risk factor as prevalence of diabetes
    increases over age groups. The preliminary regression model
    supported that Diabetes Pedigree Function has significant
    association with onset of the disease. Also, all other risk factors
    examined in the analysis has association with diabetes as
    coressponding values are higher with the presence of the disease.
    The association between Diabetes Pedigree Function and glucose
    level, skin thickness, insulin level, and BMI are proven to be
    positive. Based on this result, further study could lead to the
    development of a diabetes risk prediction model and, furthermore, a
    prediction model for diabetes induced cardiac disease prevention.
