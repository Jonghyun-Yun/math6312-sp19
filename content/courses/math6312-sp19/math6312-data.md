+++
title = "MATH 6312: Data Sets"
author = ["Jonghyun Yun"]
lastmod = 2020-04-13T20:42:30-05:00
linkTitle = "Data Sets"
categories = ["teaching", "data__sets"]
draft = false
type = "docs"
toc = true
[menu."math6312-sp19"]
  identifier = "math-6312-data-sets"
  parent = "Data Mining"
  weight = 30
+++

## Brain data {#brain-data}

-   y: `BrainWeight` are measured for 28 animals.
-   X: `BodyWeight` are measured for 28 animals.

<!--listend-->

<a id="code-snippet--read-brain"></a>
```R
brain = data.frame(fread("http://www1.aucegypt.edu/faculty/hadi/RABE5/Data5/P184.txt"), row.names=1)
```


## Credit card data {#credit-card-data}

The `credit` data set displayed in Figure 3.6 of ESL records `balance` (average credit card debt for a number of individuals) as well as several predictors: `age`, `cards` (number of credit cards), `education` (years of education), `income` (in thousands of dollars), `limit` (credit limit), `rating` (credit rating), `gender`, `student` (student vs non-student), `ethnicity`, and `married` (marital status).

<a id="code-snippet--read-credit"></a>
```R
credit = read.table("https://math6312.rbind.io/courses/math6312-sp19/data/credit.csv",sep=",",head=T,row.names=1)
credit$X = NULL
```


## Breakfast cereals {#creals}

A data set contains breakfast cereals produced by three different American manufacturers: General Mills (G), Kellogg (K), and Quaker (Q).

<a id="code-snippet--read-cereals"></a>
```R
cereal = read.table("https://math6312.rbind.io/courses/math6312-sp19/data/T11-9.txt", row.names=1)
```


## Advertising {#advertising}

The Advertising data set consists of the `sales` of that product in 200 different markets, along with advertising budgets for the product in each of those markets for three different media: `TV`, `radio`, and `newspaper`.

<a id="code-snippet--read-advertising"></a>
```R
advertising = fread("https://raw.githubusercontent.com/lneisenman/isl/master/data/Advertising.csv", drop = 1)
```


## Prostate cancer {#prostate_cancer}

Prostate cancer is cancer that begins in tissues of the prostate gland. Located just below the bladder and in front of the rectum, the prostate is the male sex gland responsible for the production of semen. `Prostate cancer data` come from a study that examined the correlation between the level of prostate specific antigen and a number of clinical measures in men who were about to receive a radical prostatectomy.

The data frame has the following variables:

-   `lcavol`: log(cancer volume)
-   `lweight`: log(prostate weight)
-   `age`: age
-   `lbph`: log(benign prostatic hyperplasia amount)
-   `svi`: seminal vesicle invasion
-   `lcp`: log(capsular penetration)
-   `gleason`: Gleason score
-   `pgg45`: percentage Gleason scores 4 or 5
-   `lpsa`: log(prostate specific antigen)

<!--listend-->

<a id="code-snippet--read-prostate"></a>
```R
prostate = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/prostate.data", drop=1)
prostate$train = NULL
```


## Salary survey {#salary-survey}

The salary survey data was developed from a salary survey of omputer professionals in a large corporation. The response variable is salary (`S`) and the predicators are:

-   `X`: experience in years
-   `E`: education coded as
    -   `1` for completion of a high school diploma
    -   `2` for completion of a bachelor degree
    -   `3` for completion of an advanced degree
-   `M` management coded as `1` for a person with management responsibility; 0 otherwise.

A linear regression on `S` will be fitted using `E` and `X`. If `E` is used in raw form, we would be assuming that each step up in education is worth a fixed increment to salary. This interpretation is possible but may be too restrictive.

<a id="code-snippet--read-salary"></a>
```R
salary.survey = fread("http://www1.aucegypt.edu/faculty/hadi/RABE5/Data5/P130.txt", data.table = F)
```


## South African heart disease {#south-African-heart-disease}

This dataset is a retrospective sample of males in a heart-disease high-risk region of the Western Cape, South Africa. There are roughly two controls per case of CHD (coronary heart disease). Many of the CHD positive men have undergone blood pressure reduction treatment and other programs to reduce their risk factors after their CHD event. Descriptions of variables in this data set can be found [here](http://web.stanford.edu/~hastie/ElemStatLearn/datasets/SAheart.info.txt).

<a id="code-snippet--read-saheart"></a>
```R
saheart = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/SAheart.data", drop = 1, data.table=F)
saheart$famhist = as.factor(saheart$famhist)
saheart$chd = as.factor(saheart$chd)
drops = c("typea","adiposity")
saheart = saheart[,!(names(saheart) %in% drops)]
```


## Vowel data {#vowel-data}

The vowel data set is used to classify 11 spoken vowels using 10 feature variables. Detailed description of the data can be found [here](https://web.stanford.edu/~hastie/ElemStatLearn/datasets/vowel.info.txt). The data includes training and test set.

<a id="code-snippet--read-vowel"></a>
```R
vowel.train = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/vowel.train", drop = 1, data.table = F)
vowel.test = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/vowel.test", drop = 1, data.table = F)
```


## Wage data {#wage-data}

This data set contains wage and other data for a group of 3000 male workers in the Mid-Atlantic region. A data frame with 3000 observations on the following 11 variables.

-   `year`: Year that wage information was recorded
-   `age`: Age of worker
-   `maritl`: A factor with levels 1. Never Married 2. Married 3. Widowed 4. Divorced and 5.: Separated indicating marital status
-   `race`: A factor with levels 1. White 2. Black 3. Asian and 4. Other indicating race
-   `education`: A factor with levels 1. < HS Grad 2. HS Grad 3. Some College 4. College Grad and 5. Advanced Degree indicating education level
-   `region`: Region of the country (mid-atlantic only)
-   `jobclass`: A factor with levels 1. Industrial and 2. Information indicating type of job
-   `health`: A factor with levels 1. <=Good and 2. >=Very Good indicating health level of worker
-   `health_ins`: A factor with levels 1. Yes and 2. No indicating whether worker has health insurance
-   `logwage`: Log of workers wage
-   `wage`: Workers raw wage

<!--listend-->

<a id="code-snippet--read-wage"></a>
```R
Wage = fread("https://math6312.rbind.io/courses/math6312-sp19/data/wage.csv", data.table = 0, drop = 1)
```


### Source {#source}

Data was manually assembled by Steve Miller, of Open BI (www.openbi.com), from the March 2011 Supplement to Current Population Survey data. <http://thedataweb.rm.census.gov/TheDataWeb>


## Spam data {#spam-data}

Description of the spam data can be found in [here](https://web.stanford.edu/~hastie/ElemStatLearn/datasets/spam.info.txt). We read in the list of spam words and delete garbage characters.

<a id="code-snippet--read-spam"></a>
```R
spam.words = read.table("ftp://ftp.ics.uci.edu/pub/machine-learning-databases/spambase/spambase.names",skip=33,sep=":",comment.char="|",stringsAsFactors=F)
  spam.words = spam.words[[1]]
  for( si in 1:length(spam.words) ){
    spam.words[si] = sub( "word_freq_", "", spam.words[si] )
    spam.words[si] = sub( "char_freq_", "", spam.words[si] )
    spam.words[si] = sub( "capital_run_length_", "CRL", spam.words[si] )
  }

spam.data = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/spam.data", data.table = 0)
spam.testidx = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/spam.traintest", data.table = 0)
```


## LPGA data {#lpga-data}

The Ladies Professional Golf Association (LPGA) data contains 7 predictors and the response (Prize Money Per Round). You can find names of variables and players from the data sets (training and test).

<a id="code-snippet--read-lpga"></a>
```R
lpga.train = fread("https://math6312.rbind.io/courses/math6312-sp19/data/lpga.train.csv")
lpga.test = fread("https://math6312.rbind.io/courses/math6312-sp19/data/lpga.test.csv")

v.name = c('Player',
           'Rounds',
           'Driving_Distance',
           'Fairway_Accuracy',
           'Green_in_Regulation_Percentage',
           'Average_Putts_Per_Game',
           'Average_Sandshots_Per_Game',
           'Sand_Save_Percentage',
           'Prize_Money_Per_Round')

colnames(lpga.train) = v.name
colnames(lpga.test) = v.name
```


## Housing data {#housing-data}

The Census Bureau divides the country up into geographic regions, smaller than counties, called _tracts_ of a few thousand people each, and reports much of its data at the level of tracts. This data set, drawn from the 2011 American Community Survey, contains information on the housing stock and economic circumstances of every tract in California and Pennsylvania. For each tract, the data file records a large number of variables.

-   A geographic ID code, a code for the state, a code for the county, and a code for the tract
-   The population, latitude and longitude of the tract
-   Its name
-   The median value of the housing units in the tract
-   The total number of units and the number of vacant units
-   The median number of rooms per unit
-   The mean number of people per household which owns its home, the mean number of people per renting household
-   The median and mean income of households (in dollars, from all sources)
-   The percentage of housing units built in 2005 or later; built in 2000--2004; built in the 1990s; in the 1980s; in the 1970s; in the 1960s; in the 1950s; in the 1940s; and in 1939 or earlier
-   The percentage of housing units with 0 bedrooms; with 1 bedroom; with 2; with 3; with 4; with 5 or more bedrooms
-   The percentage of households which own their home, and the percentage which rent

Remember that these are not values for individual houses or families, but summaries of all of the houses and families in the tract.

<a id="code-snippet--read-housing"></a>
```R
x = fread("http://www.stat.cmu.edu/~cshalizi/uADA/13/hw/01/calif_penn_2011.csv", drop = 1, data.table = F)
names(x) <- tolower(names(x))
```


## Wine quality data {#wine-quality}

The two datasets are related to red and white variants of the Portuguese "Vinho Verde" wine. For more details, consult: <https://archive.ics.uci.edu/ml/datasets/wine+quality>. Due to privacy and logistic issues, only physicochemical (inputs) and sensory (the output) variables are available (e.g. there is no data about grape types, wine brand, wine selling price, etc.).


### Attribute Information: {#attribute-information}

-   Input variables (based on physicochemical tests):
    -   1 - fixed acidity
    -   2 - volatile acidity
    -   3 - citric acid
    -   4 - residual sugar
    -   5 - chlorides
    -   6 - free sulfur dioxide
    -   7 - total sulfur dioxide
    -   8 - density
    -   9 - pH
    -   10 - sulphates
    -   11 - alcohol

-   Output variable (based on sensory data):
    -   12 - quality (score between 0 and 10)

<!--listend-->

<a id="code-snippet--read-wine"></a>
```R
red = fread("https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv")
white = fread("https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv")
```


## NCI microarray {#nci-microarray}

The data contains expression levels on 6830 genes from 64 cancer cell lines. Cancer type is also recorded.

<a id="code-snippet--read-nci"></a>
```R
cancer = read.table("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/nci.info.txt", skip = 20, stringsAsFactors = F)
nci = data.table::fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/nci.data")
colnames(nci) = c(cancer[,1]) # assign the cancer type.
```


## Orange juice {#orange-juice}

The data contains 1070 purchases where the customer either purchased Citrus Hill or Minute Maid Orange Juice. A number of characteristics of the customer and product are recorded.

A data frame with 1070 observations on the following 18 variables:

-   `Purchase`: A factor with levels CH and MM indicating whether the customer purchased Citrus Hill or Minute Maid Orange Juice
-   `WeekofPurchase`: Week of purchase
-   `StoreID`: Store ID
-   `PriceCH`: Price charged for CH
-   `PriceMM`: Price charged for MM
-   `DiscCH`: Discount offered for CH
-   `DiscMM`: Discount offered for MM
-   `SpecialCH`: Indicator of special on CH
-   `SpecialMM`: Indicator of special on MM
-   `LoyalCH`: Customer brand loyalty for CH
-   `SalePriceMM`: Sale price for MM
-   `SalePriceCH`: Sale price for CH
-   `PriceDiff`: Sale price of MM less sale price of CH
-   `Store7`: A factor with levels No and Yes indicating whether the sale is at Store 7
-   `PctDiscMM`: Percentage discount for MM
-   `PctDiscCH`: Percentage discount for CH
-   `ListPriceDiff`: List price of MM less list price of CH
-   `STORE`: Which of 5 possible stores the sale occured at

<!--listend-->

<a id="code-snippet--read-oj"></a>
```R
oj = fread("https://math6312.rbind.io/courses/math6312-sp19/data/oj.csv", data.table = 0)
```


## Amazon employee Access {#amazon-employee-access}

When an employee at any company starts work, they first need to obtain the computer access necessary to fulfill their role. This access may allow an employee to read/manipulate resources through various applications or web portals. It is assumed that employees fulfilling the functions of a given role will access the same or similar resources. It is often the case that employees figure out the access they need as they encounter roadblocks during their daily work (e.g. not able to log into a reporting portal). A knowledgeable supervisor then takes time to manually grant the needed access in order to overcome access obstacles. As employees move throughout a company, this access discovery/recovery cycle wastes a nontrivial amount of time and money.

There is a considerable amount of data regarding an employee’s role within an organization and the resources to which they have access. Given the data related to current employees and their provisioned access, models can be built that automatically determine access privileges as employees enter and leave roles within a company. These auto-access models seek to minimize the human involvement required to grant or revoke employee access.


### Column Descriptions {#column-descriptions}

The data consists of real historical data collected from 2010 & 2011. Employees are manually allowed or denied access to resources over time. Each row has the ACTION (ground truth), RESOURCE, and information about the employee's role at the time of approval.

-   `ACTION`: ACTION is 1 if the resource was approved, 0 if the resource was not
-   `RESOURCE`: An ID for each resource
-   `MGR_ID`: The EMPLOYEE ID of the manager of the current EMPLOYEE ID record; an employee may have only one manager at a time
-   `ROLE_ROLLUP_1`: Company role grouping category id 1 (e.g. US Engineering)
-   `ROLE_ROLLUP_2`: Company role grouping category id 2 (e.g. US Retail)
-   `ROLE_DEPTNAME`: Company role department description (e.g. Retail)
-   `ROLE_TITLE`: Company role business title description (e.g. Senior Engineering Retail Manager)
-   `ROLE_FAMILY_DESC`: Company role family extended description (e.g. Retail Manager, Software Engineering)
-   `ROLE_FAMILY`: Company role family description (e.g. Retail Manager)
-   `ROLE_CODE`: Company role code; this code is unique to each role (e.g. Manager)

<!--listend-->

<a id="code-snippet--read-amazon"></a>
```R
dat = read_csv("https://math6312.rbind.io/courses/math6312-sp19/data/amazon_access.csv")
```


## Handwritten digit data {#handwritten-digit-data}

Normalized handwritten digits are automatically scanned from envelopes by the U.S. Postal Service. The original scanned digits are binary and of different sizes and orientations; the images have been deslanted and size normalized, resulting in 16 x 16 grayscale images (Le Cun et al., 1990).

There are 7291 training observations and 2007 test observations, distributed as follows:

|       | 0    | 1    | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | Total |
|-------|------|------|-----|-----|-----|-----|-----|-----|-----|-----|-------|
| Train | 1194 | 1005 | 731 | 658 | 652 | 556 | 664 | 645 | 542 | 644 | 7291  |
| Test  | 359  | 264  | 198 | 166 | 200 | 160 | 170 | 147 | 166 | 177 | 2007  |

<a id="code-snippet--read-zip"></a>
```R
ztrain = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/zip.train.gz")
ztest = fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/zip.test.gz")

names(ztrain) = names(ztest) = c("y", paste("X.", 1:256,sep=""))

ztrain$y = as.factor(ztrain$y)
ztest$y = as.factor(ztest$y)
```


### Source {#source}

<https://web.stanford.edu/~hastie/StatLearnSparsity%5Ffiles/DATA/zipcode.html>


## Iris data set {#iris-data}

This famous (Fisher's or Anderson's) iris data set gives the measurements in centimeters of the variables sepal length and width and petal length and width, respectively, for 50 flowers from each of 3 species of iris. The species are Iris setosa, versicolor, and virginica.

Running a command below will load the Iris data set to data frame `iris`.

<a id="code-snippet--read-iris"></a>
```R
data(iris)
```
