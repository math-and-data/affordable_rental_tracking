Affordable Housing Project - CityCampNC 2015
Gjeltema
6/13/2015
========================================================

We merge the available rental data from Wake county.


```r
# install.packages('dplyr') install.packages('ggplot2') library('magrittr')
# library('stringr')
library("dplyr")
```

```
## Warning: package 'dplyr' was built under R version 3.1.3
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library("ggplot2")

aptlist <- read.csv("2011 APT list.csv", header = TRUE)
sflist <- read.csv("2011 SF list.csv", header = TRUE)
mhlist <- read.csv("2011 MH list.csv", header = TRUE)
mflist <- read.csv("2011 MF list.csv", header = TRUE)
duplexlist <- read.csv("2011 Duplex list.csv", header = TRUE)
condolist <- read.csv("2011 Condo List.csv", header = TRUE)
thlist <- read.csv("2011 TH list.csv", header = TRUE)
head(aptlist)
```

```
##   Form.ID Billing.Year Num.Units Fees          Address  Dwelling.Type
## 1     438         2011         8  100   1409 SAWYER RD Apartment Bldg
## 2    8984         2011       180 1820  5411 REUNION PT Apartment Bldg
## 3    5322         2011        24  260    611 PEYTON ST Apartment Bldg
## 4     773         2011        10  120    1721 POOLE RD Apartment Bldg
## 5    5294         2011         9  110   3021 MEDLIN DR Apartment Bldg
## 6    3482         2011       318 3200 4801 FIVELEAF LA Apartment Bldg
```

```r
head(condolist)
```

```
##   Form.ID Num.Units Fees                   Address Dwelling.Type
## 1    1104         1   30 11381-101 CLUBHAVEN PLACE   Condominium
## 2    1104         1   10 11381-102 CLUBHAVEN PLACE   Condominium
## 3    9638         1   30   319-210 FAYETTEVILLE ST   Condominium
## 4    9638         1   10   319-308 FAYETTEVILLE ST   Condominium
## 5    9638         1   10  319-401 FAYETTEVILLE  ST   Condominium
## 6    9638         1   10   319-414 FAYETTEVILLE ST   Condominium
##   Condo.Complex.Name
## 1        Avera Place
## 2        Avera Place
## 3      Hudson Condos
## 4      Hudson Condos
## 5      Hudson Condos
## 6      Hudson Condos
```

```r
names(aptlist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(mflist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(mhlist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(sflist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(thlist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(duplexlist)
```

```
## [1] "Form.ID"       "Billing.Year"  "Num.Units"     "Fees"         
## [5] "Address"       "Dwelling.Type"
```

```r
names(condolist)  # has an extra column
```

```
## [1] "Form.ID"            "Num.Units"          "Fees"              
## [4] "Address"            "Dwelling.Type"      "Condo.Complex.Name"
```

```r

full_rentals <- bind_rows(aptlist, sflist) %>% bind_rows(duplexlist) %>% bind_rows(mflist) %>% 
    bind_rows(mhlist) %>% bind_rows(thlist) %>% mutate(Condo.Complex.Name = NA) %>% 
    bind_rows(condolist)
```

```
## Warning: Unequal factor levels: coercing to character
## Warning: Unequal factor levels: coercing to character
```

```r


str(full_rentals)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	15397 obs. of  7 variables:
##  $ Form.ID           : int  438 8984 5322 773 5294 3482 5014 7671 5322 7538 ...
##  $ Billing.Year      : int  2011 2011 2011 2011 2011 2011 2011 2011 2011 2011 ...
##  $ Num.Units         : int  8 180 24 10 9 318 4 6 16 4 ...
##  $ Fees              : int  100 1820 260 120 110 3200 60 80 180 60 ...
##  $ Address           : chr  "1409 SAWYER RD" "5411 REUNION PT" "611 PEYTON ST" "1721 POOLE RD" ...
##  $ Dwelling.Type     : chr  "Apartment Bldg" "Apartment Bldg" "Apartment Bldg" "Apartment Bldg" ...
##  $ Condo.Complex.Name: Factor w/ 158 levels "","107 Sunpoint Condos Phs 7",..: NA NA NA NA NA NA NA NA NA NA ...
```

```r
full_rentals <- full_rentals %>% mutate(Form.ID = as.factor(Form.ID), Billing.Year = as.ordered(Billing.Year), 
    Dwelling.Type = as.factor(Dwelling.Type)) %>% select(-Fees)

write.csv(full_rentals, "rental_data.csv")
```


