---
title: Manipulating and Joining Data 
nav: true
--- 

# <a name="00-id"></a> Manipulating and joining data (#00-id)

Using `dplyr`

## Content:
 - [Filtering subjects based on criteria](#01-id)
 - [Select](#02-id)
 - [Making new variables](#03-id)
 - [Grouping subjects based on criteria](#04-id)
 - [Producing summary statistics for groups of subjects](#05-id)
 - [Putting together multiple dataframes with common subjects](#06-id)

## <a name="01-id"></a> Filtering subjects based on criteria


```R
library(tidyverse)
```

How to access rows (subjects) and columns (variables) in a dataframe? For example, we can get the first row of the mpg dataset, like this:


```R
mpg[1,]
```


<table>
<caption>A tibble: 1 × 11</caption>
<thead>
	<tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>audi</td><td>a4</td><td>1.8</td><td>1999</td><td>4</td><td>auto(l5)</td><td>f</td><td>18</td><td>29</td><td>p</td><td>compact</td></tr>
</tbody>
</table>



This way is fine most of the time, but sometimes we would like to be more precise with filtering examples. So, I'm going to use the package `dplyr` in the `tidyverse` package. Also, I am going to use the flights dataset, here. This dataset is in the `nycflights13` package.


```R
library(nycflights13)
head(flights)
```


<table>
<caption>A tibble: 6 × 27</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>...</th><th scope=col>minute</th><th scope=col>time_hour</th><th scope=col>name.x</th><th scope=col>name.y</th><th scope=col>lat</th><th scope=col>lon</th><th scope=col>alt</th><th scope=col>tz</th><th scope=col>dst</th><th scope=col>tzone</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>...</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2013</td><td>1</td><td>1</td><td>517</td><td>515</td><td> 2</td><td> 830</td><td> 819</td><td> 11</td><td>UA</td><td>...</td><td>15</td><td>2013-01-01 05:00:00</td><td>United Air Lines Inc. </td><td>Newark Liberty Intl</td><td>40.69250</td><td>-74.16867</td><td>18</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
	<tr><td>2013</td><td>1</td><td>1</td><td>533</td><td>529</td><td> 4</td><td> 850</td><td> 830</td><td> 20</td><td>UA</td><td>...</td><td>29</td><td>2013-01-01 05:00:00</td><td>United Air Lines Inc. </td><td>La Guardia         </td><td>40.77725</td><td>-73.87261</td><td>22</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
	<tr><td>2013</td><td>1</td><td>1</td><td>542</td><td>540</td><td> 2</td><td> 923</td><td> 850</td><td> 33</td><td>AA</td><td>...</td><td>40</td><td>2013-01-01 05:00:00</td><td>American Airlines Inc.</td><td>John F Kennedy Intl</td><td>40.63975</td><td>-73.77893</td><td>13</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
	<tr><td>2013</td><td>1</td><td>1</td><td>544</td><td>545</td><td>-1</td><td>1004</td><td>1022</td><td>-18</td><td>B6</td><td>...</td><td>45</td><td>2013-01-01 05:00:00</td><td>JetBlue Airways       </td><td>John F Kennedy Intl</td><td>40.63975</td><td>-73.77893</td><td>13</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
	<tr><td>2013</td><td>1</td><td>1</td><td>554</td><td>600</td><td>-6</td><td> 812</td><td> 837</td><td>-25</td><td>DL</td><td>...</td><td> 0</td><td>2013-01-01 06:00:00</td><td>Delta Air Lines Inc.  </td><td>La Guardia         </td><td>40.77725</td><td>-73.87261</td><td>22</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
	<tr><td>2013</td><td>1</td><td>1</td><td>554</td><td>558</td><td>-4</td><td> 740</td><td> 728</td><td> 12</td><td>UA</td><td>...</td><td>58</td><td>2013-01-01 05:00:00</td><td>United Air Lines Inc. </td><td>Newark Liberty Intl</td><td>40.69250</td><td>-74.16867</td><td>18</td><td>-5</td><td>A</td><td>America/New_York</td></tr>
</tbody>
</table>



The first function of `dplyr` I'll use is `filter()`. This lets us filter the subjects that we want, that is, filter the rows: the ***flights***.

Let's start by filtering for just the flights in January (Month 1): 


```R
print(filter(flights, month == 1))
```

    [90m# A tibble: 27,004 x 27[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      517            515         2      830            819
    [90m 2[39m  [4m2[24m013     1     1      533            529         4      850            830
    [90m 3[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 4[39m  [4m2[24m013     1     1      544            545        -[31m1[39m     [4m1[24m004           [4m1[24m022
    [90m 5[39m  [4m2[24m013     1     1      554            600        -[31m6[39m      812            837
    [90m 6[39m  [4m2[24m013     1     1      554            558        -[31m4[39m      740            728
    [90m 7[39m  [4m2[24m013     1     1      555            600        -[31m5[39m      913            854
    [90m 8[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      709            723
    [90m 9[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      838            846
    [90m10[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m# ... with 26,994 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

This gives us 27004 rows (refer to the top of the output).

How about the first day of January?


```R
print(filter(flights, month == 1, day == 1))
```

    [90m# A tibble: 842 x 27[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      517            515         2      830            819
    [90m 2[39m  [4m2[24m013     1     1      533            529         4      850            830
    [90m 3[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 4[39m  [4m2[24m013     1     1      544            545        -[31m1[39m     [4m1[24m004           [4m1[24m022
    [90m 5[39m  [4m2[24m013     1     1      554            600        -[31m6[39m      812            837
    [90m 6[39m  [4m2[24m013     1     1      554            558        -[31m4[39m      740            728
    [90m 7[39m  [4m2[24m013     1     1      555            600        -[31m5[39m      913            854
    [90m 8[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      709            723
    [90m 9[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      838            846
    [90m10[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m# ... with 832 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

### magrittr

As we build up more functions in our data manipulation tool bag, we are going to end up with lots of nested functions. Instead, we can use the `magrittr` or "pipe" symbol `%>%`. Creating a *pipeline* of functions.

The command `%>%` can be read as "then". For example, to print out a filtered result of the first of January, we can rewrite the above command as: 


```R
print(flights %>% filter(month == 1, day == 1))
```

    [90m# A tibble: 842 x 27[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      517            515         2      830            819
    [90m 2[39m  [4m2[24m013     1     1      533            529         4      850            830
    [90m 3[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 4[39m  [4m2[24m013     1     1      544            545        -[31m1[39m     [4m1[24m004           [4m1[24m022
    [90m 5[39m  [4m2[24m013     1     1      554            600        -[31m6[39m      812            837
    [90m 6[39m  [4m2[24m013     1     1      554            558        -[31m4[39m      740            728
    [90m 7[39m  [4m2[24m013     1     1      555            600        -[31m5[39m      913            854
    [90m 8[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      709            723
    [90m 9[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      838            846
    [90m10[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m# ... with 832 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

Now let's print out a filtered result for all American Airlines flights (AA)


```R
print(flights %>% filter(carrier == "AA"))
```

    [90m# A tibble: 32,729 x 27[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 2[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m 3[39m  [4m2[24m013     1     1      559            600        -[31m1[39m      941            910
    [90m 4[39m  [4m2[24m013     1     1      606            610        -[31m4[39m      858            910
    [90m 5[39m  [4m2[24m013     1     1      623            610        13      920            915
    [90m 6[39m  [4m2[24m013     1     1      628            630        -[31m2[39m     [4m1[24m137           [4m1[24m140
    [90m 7[39m  [4m2[24m013     1     1      629            630        -[31m1[39m      824            810
    [90m 8[39m  [4m2[24m013     1     1      635            635         0     [4m1[24m028            940
    [90m 9[39m  [4m2[24m013     1     1      656            700        -[31m4[39m      854            850
    [90m10[39m  [4m2[24m013     1     1      656            659        -[31m3[39m      949            959
    [90m# ... with 32,719 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

No I'll filter *dep_delay* (departure delay time) by a range of 120 to 240 minutes:


```R
print(filter(flights, dep_delay %in% 120:240))
```

    [90m# A tibble: 8,364 x 27[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      957            733       144     [4m1[24m056            853
    [90m 2[39m  [4m2[24m013     1     1     [4m1[24m114            900       134     [4m1[24m447           [4m1[24m222
    [90m 3[39m  [4m2[24m013     1     1     [4m1[24m540           [4m1[24m338       122     [4m2[24m020           [4m1[24m825
    [90m 4[39m  [4m2[24m013     1     1     [4m1[24m856           [4m1[24m645       131     [4m2[24m212           [4m2[24m005
    [90m 5[39m  [4m2[24m013     1     1     [4m1[24m934           [4m1[24m725       129     [4m2[24m126           [4m1[24m855
    [90m 6[39m  [4m2[24m013     1     1     [4m1[24m938           [4m1[24m703       155     [4m2[24m109           [4m1[24m823
    [90m 7[39m  [4m2[24m013     1     1     [4m1[24m942           [4m1[24m705       157     [4m2[24m124           [4m1[24m830
    [90m 8[39m  [4m2[24m013     1     1     [4m2[24m006           [4m1[24m630       216     [4m2[24m230           [4m1[24m848
    [90m 9[39m  [4m2[24m013     1     1     [4m2[24m009           [4m1[24m808       121     [4m2[24m145           [4m1[24m942
    [90m10[39m  [4m2[24m013     1     1     [4m2[24m221           [4m2[24m000       141     [4m2[24m331           [4m2[24m124
    [90m# ... with 8,354 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

How many records(flights) in the data.frame have a departure delay time in a range of 120 to 240 for the carrier AA?


```R
flights %>% filter(carrier == "AA", dep_delay %in% 120:240) %>% nrow()
```


620


What proportion of records in the data.frame for the carrier AA as that departure delay time range?


```R
n1 = flights %>% filter(carrier == "AA", dep_delay %in% 120:240) %>% nrow()
round(n1/nrow(population),3)
```


0.153


## <a name="02-id"></a>Select ### [return](#00-id)

The `ncyflights13` dataset is too wide to fit onto the screen. In all the examples of filtering above we couldn’t even see all of the variables – there were always 12 columns which didn’t fit on the screen. It’d be good if we could make our dataframe a bit narrower, so that we can fit the information we’re interested in (and nothing else) onto the screen. This is what select does – it’s essentially a filter, but for columns rather than rows.

So let’s say I was just interested in departure times and arrival times for all American Airways flights on the 1st of January, then I could just add to our filter example from before:


```R
print(flights %>% 
        filter(carrier == "AA", month == 1, day == 1) %>% 
        select(flight, dep_time, arr_time))
```

    [90m# A tibble: 94 x 3[39m
       flight dep_time arr_time
        [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m
    [90m 1[39m   [4m1[24m141      542      923
    [90m 2[39m    301      558      753
    [90m 3[39m    707      559      941
    [90m 4[39m   [4m1[24m895      606      858
    [90m 5[39m   [4m1[24m837      623      920
    [90m 6[39m    413      628     [4m1[24m137
    [90m 7[39m    303      629      824
    [90m 8[39m    711      635     [4m1[24m028
    [90m 9[39m    305      656      854
    [90m10[39m   [4m1[24m815      656      949
    [90m# ... with 84 more rows[39m
    

We can get fancy with select: if I wanted to grab just the variables from flights that have something to do with “time”, then I could use the contains command:


```R
print(flights %>% select(contains("time")))
```

    [90m# A tibble: 336,776 x 6[39m
       dep_time sched_dep_time arr_time sched_arr_time air_time time_hour          
          [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m    [3m[90m<dbl>[39m[23m [3m[90m<dttm>[39m[23m             
    [90m 1[39m      517            515      830            819      227 2013-01-01 [90m05:00:00[39m
    [90m 2[39m      533            529      850            830      227 2013-01-01 [90m05:00:00[39m
    [90m 3[39m      542            540      923            850      160 2013-01-01 [90m05:00:00[39m
    [90m 4[39m      544            545     [4m1[24m004           [4m1[24m022      183 2013-01-01 [90m05:00:00[39m
    [90m 5[39m      554            600      812            837      116 2013-01-01 [90m06:00:00[39m
    [90m 6[39m      554            558      740            728      150 2013-01-01 [90m05:00:00[39m
    [90m 7[39m      555            600      913            854      158 2013-01-01 [90m06:00:00[39m
    [90m 8[39m      557            600      709            723       53 2013-01-01 [90m06:00:00[39m
    [90m 9[39m      557            600      838            846      140 2013-01-01 [90m06:00:00[39m
    [90m10[39m      558            600      753            745      138 2013-01-01 [90m06:00:00[39m
    [90m# ... with 336,766 more rows[39m
    


```R
print(select(flights, year:day))
```

    [90m# A tibble: 336,776 x 3[39m
        year month   day
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1
    [90m 2[39m  [4m2[24m013     1     1
    [90m 3[39m  [4m2[24m013     1     1
    [90m 4[39m  [4m2[24m013     1     1
    [90m 5[39m  [4m2[24m013     1     1
    [90m 6[39m  [4m2[24m013     1     1
    [90m 7[39m  [4m2[24m013     1     1
    [90m 8[39m  [4m2[24m013     1     1
    [90m 9[39m  [4m2[24m013     1     1
    [90m10[39m  [4m2[24m013     1     1
    [90m# ... with 336,766 more rows[39m
    

Selecting all columns in the `flights` data.frame that contain double type variables. 


```R
print(select_if(flights, is.double))
```

    [90m# A tibble: 336,776 x 11[39m
       dep_delay arr_delay air_time distance  hour minute time_hour             lat
           [3m[90m<dbl>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<dbl>[39m[23m    [3m[90m<dbl>[39m[23m [3m[90m<dbl>[39m[23m  [3m[90m<dbl>[39m[23m [3m[90m<dttm>[39m[23m              [3m[90m<dbl>[39m[23m
    [90m 1[39m         2        11      227     [4m1[24m400     5     15 2013-01-01 [90m05:00:00[39m  40.7
    [90m 2[39m         4        20      227     [4m1[24m416     5     29 2013-01-01 [90m05:00:00[39m  40.8
    [90m 3[39m         2        33      160     [4m1[24m089     5     40 2013-01-01 [90m05:00:00[39m  40.6
    [90m 4[39m        -[31m1[39m       -[31m18[39m      183     [4m1[24m576     5     45 2013-01-01 [90m05:00:00[39m  40.6
    [90m 5[39m        -[31m6[39m       -[31m25[39m      116      762     6      0 2013-01-01 [90m06:00:00[39m  40.8
    [90m 6[39m        -[31m4[39m        12      150      719     5     58 2013-01-01 [90m05:00:00[39m  40.7
    [90m 7[39m        -[31m5[39m        19      158     [4m1[24m065     6      0 2013-01-01 [90m06:00:00[39m  40.7
    [90m 8[39m        -[31m3[39m       -[31m14[39m       53      229     6      0 2013-01-01 [90m06:00:00[39m  40.8
    [90m 9[39m        -[31m3[39m        -[31m8[39m      140      944     6      0 2013-01-01 [90m06:00:00[39m  40.6
    [90m10[39m        -[31m2[39m         8      138      733     6      0 2013-01-01 [90m06:00:00[39m  40.8
    [90m# ... with 336,766 more rows, and 3 more variables: lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m,
    #   tz [3m[90m<dbl>[90m[23m[39m
    

## <a name="03-id"></a>Making new variables

What if we wanted to add new columns, in the dataset `flights`? 

We have the departure delay `dep_delay`, which is the difference between the scheduled departure time (`sched_dep_time`) and the departure time (`dep_time`); let's assume that we do not have that and we would like to calculate it. We can do that with:


```R
print(flights %>% mutate(delay = dep_time - sched_dep_time))
```

    [90m# A tibble: 336,776 x 28[39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      517            515         2      830            819
    [90m 2[39m  [4m2[24m013     1     1      533            529         4      850            830
    [90m 3[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 4[39m  [4m2[24m013     1     1      544            545        -[31m1[39m     [4m1[24m004           [4m1[24m022
    [90m 5[39m  [4m2[24m013     1     1      554            600        -[31m6[39m      812            837
    [90m 6[39m  [4m2[24m013     1     1      554            558        -[31m4[39m      740            728
    [90m 7[39m  [4m2[24m013     1     1      555            600        -[31m5[39m      913            854
    [90m 8[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      709            723
    [90m 9[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      838            846
    [90m10[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m# ... with 336,766 more rows, and 20 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m, delay [3m[90m<int>[90m[23m[39m
    

If we want to add the calculated column into the dataset:
flights <- flights %>% mutate(delay = dep_time - sched_dep_time)
## <a name="04-id"></a>Grouping subjects based on criteria

We can now do something quite powerful like clump variables together into groups, and then summarise these groups.

I have an hypothesis about flight delays in New York City: I reckon they increase in winter. Due to snow, ice, weather reasons in general. I suspect that in the winter months, the delays in December, January, February, are worse than in the summer months. To investigate this, we'll need to group flights by month, which can be done like this: 


```R
by_month <- group_by(flights,month)
print(by_month)
```

    [90m# A tibble: 336,776 x 27[39m
    [90m# Groups:   month [12][39m
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m     [3m[90m<dbl>[39m[23m    [3m[90m<int>[39m[23m          [3m[90m<int>[39m[23m
    [90m 1[39m  [4m2[24m013     1     1      517            515         2      830            819
    [90m 2[39m  [4m2[24m013     1     1      533            529         4      850            830
    [90m 3[39m  [4m2[24m013     1     1      542            540         2      923            850
    [90m 4[39m  [4m2[24m013     1     1      544            545        -[31m1[39m     [4m1[24m004           [4m1[24m022
    [90m 5[39m  [4m2[24m013     1     1      554            600        -[31m6[39m      812            837
    [90m 6[39m  [4m2[24m013     1     1      554            558        -[31m4[39m      740            728
    [90m 7[39m  [4m2[24m013     1     1      555            600        -[31m5[39m      913            854
    [90m 8[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      709            723
    [90m 9[39m  [4m2[24m013     1     1      557            600        -[31m3[39m      838            846
    [90m10[39m  [4m2[24m013     1     1      558            600        -[31m2[39m      753            745
    [90m# ... with 336,766 more rows, and 19 more variables: arr_delay [3m[90m<dbl>[90m[23m,
    #   carrier [3m[90m<chr>[90m[23m, flight [3m[90m<int>[90m[23m, tailnum [3m[90m<chr>[90m[23m, origin [3m[90m<chr>[90m[23m, dest [3m[90m<chr>[90m[23m,
    #   air_time [3m[90m<dbl>[90m[23m, distance [3m[90m<dbl>[90m[23m, hour [3m[90m<dbl>[90m[23m, minute [3m[90m<dbl>[90m[23m, time_hour [3m[90m<dttm>[90m[23m,
    #   name.x [3m[90m<chr>[90m[23m, name.y [3m[90m<chr>[90m[23m, lat [3m[90m<dbl>[90m[23m, lon [3m[90m<dbl>[90m[23m, alt [3m[90m<dbl>[90m[23m, tz [3m[90m<dbl>[90m[23m,
    #   dst [3m[90m<chr>[90m[23m, tzone [3m[90m<chr>[90m[23m[39m
    

This data frame is the same as the original, apart from the second line: `Groups: month [12]`. That tells me that a group has been created for each month, but to explore this further, we'll have to summarise and that's in the next section.

About `group_by`:

- We can group by multiple variables: `by_day <- group_by(flights,year,month,day)` will create a dataframe with 365 groups for each day of the year, and
- We can ungroup a grouped dataframe using, `ungroup()`. That'll be handy in the next section.


## <a name="05-id"></a>Producing summary statistics for groups of subjects

To test the hypothesis about flight delays in winter, I will need to create a summary statistic about delays for each month. We can do it by calculating the mean flight departure delay for each calendar month like this:


```R
summarise(by_month, delay = mean(dep_delay, na.rm = TRUE))
```


<table>
<caption>A tibble: 12 × 2</caption>
<thead>
	<tr><th scope=col>month</th><th scope=col>delay</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td> 1</td><td>10.036665</td></tr>
	<tr><td> 2</td><td>10.816843</td></tr>
	<tr><td> 3</td><td>13.227076</td></tr>
	<tr><td> 4</td><td>13.938038</td></tr>
	<tr><td> 5</td><td>12.986859</td></tr>
	<tr><td> 6</td><td>20.846332</td></tr>
	<tr><td> 7</td><td>21.727787</td></tr>
	<tr><td> 8</td><td>12.611040</td></tr>
	<tr><td> 9</td><td> 6.722476</td></tr>
	<tr><td>10</td><td> 6.243988</td></tr>
	<tr><td>11</td><td> 5.435362</td></tr>
	<tr><td>12</td><td>16.576688</td></tr>
</tbody>
</table>



This has takenthe grouped dataframe `by_month`, and for each group has computed the mean of the values in the `dep_delay` column for that group. The `na.rm = TRUE` argument tells the mean function to remove (`rm` in unix-talk) all values that are not available `NA`. Basically, some rows in this dataframe do not have an entry in the `dep_delay column`, so R puts the symbol `NA` there instead. Trying to calculate the mean with *NA*'s doesn't work.

But looking at the results, I don't think the hypothesis was correct. December has slightly worse delays than the preceeding months, but January and February really weren't so bad, and by far the worst months are June and July. We can make a nice plot of the trends over the entire year using `ggplot`. We'll group by day this time instead of month:


```R
by_day <- group_by(flights,year,month,day)
summarise(by_day, delay = mean(dep_delay, na.rm = TRUE)) %>%
ungroup() %>% 
mutate(day_num = seq_along(delay)) %>% 
ggplot(aes(day_num,delay)) + 
geom_point() + 
geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'
    


![png](output_52_1.png)


I've added a slightly tricky intermediate step here to create a column `day_num` counting the days of the year: `ungroup() %>% mutate(day_num = seq_along(delay))` ungroups `by_day`, and then creates a sequence along the column delay - essentially counting the row numbers.

I think that delays increases along with number passengers. The more the passengers that are flying, the more delays we have. Guided by this exploration, June, July, and December would be the busiest months for flying, and that there are longer delays when there are more flights. A simple modification to our `summarise` command will allow us to explore this relationship:


```R
summarise(by_day, delay = mean(dep_delay, na.rm = TRUE), num_flights = n()) %>%
ggplot(aes(num_flights,delay)) + 
geom_point() + 
geom_smooth(method='lm') #method='lm' to visualize a linear relationship
```


![png](output_55_0.png)


The `num_flights = n()` bit produces a second summary statistic for each group, which is just the number of items in that group. Note that I don't need to index days of the year now, so I can lose the `ungroup` bit.

It looks like there's some relationship between number of flights and delays, but it's not particularly strong. Again, some more investigation is needed.

## <a name="06-id"></a>Putting together multiple dataframes with common subjects

Now let's look at the dataframe `airlines` in the `nycflights13` package:


```R
airlines
```


<table>
<caption>A tibble: 16 × 2</caption>
<thead>
	<tr><th scope=col>carrier</th><th scope=col>name</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>9E</td><td>Endeavor Air Inc.          </td></tr>
	<tr><td>AA</td><td>American Airlines Inc.     </td></tr>
	<tr><td>AS</td><td>Alaska Airlines Inc.       </td></tr>
	<tr><td>B6</td><td>JetBlue Airways            </td></tr>
	<tr><td>DL</td><td>Delta Air Lines Inc.       </td></tr>
	<tr><td>EV</td><td>ExpressJet Airlines Inc.   </td></tr>
	<tr><td>F9</td><td>Frontier Airlines Inc.     </td></tr>
	<tr><td>FL</td><td>AirTran Airways Corporation</td></tr>
	<tr><td>HA</td><td>Hawaiian Airlines Inc.     </td></tr>
	<tr><td>MQ</td><td>Envoy Air                  </td></tr>
	<tr><td>OO</td><td>SkyWest Airlines Inc.      </td></tr>
	<tr><td>UA</td><td>United Air Lines Inc.      </td></tr>
	<tr><td>US</td><td>US Airways Inc.            </td></tr>
	<tr><td>VX</td><td>Virgin America             </td></tr>
	<tr><td>WN</td><td>Southwest Airlines Co.     </td></tr>
	<tr><td>YV</td><td>Mesa Airlines Inc.         </td></tr>
</tbody>
</table>



So there are two dataframes:

- `flights` - each row is a flight, with a variable `carrier`, which gives the abbreviation for the carrier of each flight
- `airlines` - each row is an airline company, which also has a variable called `carrier`.

I would like you to add the full names to the flights dataset and then saving it.


```R
flights <- left_join(flights, airlines, by = "carrier")
```

I've made a ***left join*** of the dataset airlines into flights by the common feature `carrier`. 


```R
print(flights %>% select(year, month, day, carrier, name))
```

    [90m# A tibble: 336,776 x 5[39m
        year month   day carrier name                    
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<chr>[39m[23m   [3m[90m<chr>[39m[23m                   
    [90m 1[39m  [4m2[24m013     1     1 UA      United Air Lines Inc.   
    [90m 2[39m  [4m2[24m013     1     1 UA      United Air Lines Inc.   
    [90m 3[39m  [4m2[24m013     1     1 AA      American Airlines Inc.  
    [90m 4[39m  [4m2[24m013     1     1 B6      JetBlue Airways         
    [90m 5[39m  [4m2[24m013     1     1 DL      Delta Air Lines Inc.    
    [90m 6[39m  [4m2[24m013     1     1 UA      United Air Lines Inc.   
    [90m 7[39m  [4m2[24m013     1     1 B6      JetBlue Airways         
    [90m 8[39m  [4m2[24m013     1     1 EV      ExpressJet Airlines Inc.
    [90m 9[39m  [4m2[24m013     1     1 B6      JetBlue Airways         
    [90m10[39m  [4m2[24m013     1     1 AA      American Airlines Inc.  
    [90m# ... with 336,766 more rows[39m
    

I've used `select` to show only come features and check that we've added the extended name of the carrier.

We can do the same with `airports` dataset:


```R
print(airports)
```

    [90m# A tibble: 1,458 x 8[39m
       faa   name                       lat    lon   alt    tz dst   tzone          
       [3m[90m<chr>[39m[23m [3m[90m<chr>[39m[23m                    [3m[90m<dbl>[39m[23m  [3m[90m<dbl>[39m[23m [3m[90m<dbl>[39m[23m [3m[90m<dbl>[39m[23m [3m[90m<chr>[39m[23m [3m[90m<chr>[39m[23m          
    [90m 1[39m 04G   Lansdowne Airport         41.1  -[31m80[39m[31m.[39m[31m6[39m  [4m1[24m044    -[31m5[39m A     America/New_Yo~
    [90m 2[39m 06A   Moton Field Municipal A~  32.5  -[31m85[39m[31m.[39m[31m7[39m   264    -[31m6[39m A     America/Chicago
    [90m 3[39m 06C   Schaumburg Regional       42.0  -[31m88[39m[31m.[39m[31m1[39m   801    -[31m6[39m A     America/Chicago
    [90m 4[39m 06N   Randall Airport           41.4  -[31m74[39m[31m.[39m[31m4[39m   523    -[31m5[39m A     America/New_Yo~
    [90m 5[39m 09J   Jekyll Island Airport     31.1  -[31m81[39m[31m.[39m[31m4[39m    11    -[31m5[39m A     America/New_Yo~
    [90m 6[39m 0A9   Elizabethton Municipal ~  36.4  -[31m82[39m[31m.[39m[31m2[39m  [4m1[24m593    -[31m5[39m A     America/New_Yo~
    [90m 7[39m 0G6   Williams County Airport   41.5  -[31m84[39m[31m.[39m[31m5[39m   730    -[31m5[39m A     America/New_Yo~
    [90m 8[39m 0G7   Finger Lakes Regional A~  42.9  -[31m76[39m[31m.[39m[31m8[39m   492    -[31m5[39m A     America/New_Yo~
    [90m 9[39m 0P2   Shoestring Aviation Air~  39.8  -[31m76[39m[31m.[39m[31m6[39m  [4m1[24m000    -[31m5[39m U     America/New_Yo~
    [90m10[39m 0S9   Jefferson County Intl     48.1 -[31m123[39m[31m.[39m    108    -[31m8[39m A     America/Los_An~
    [90m# ... with 1,448 more rows[39m
    


```R
print(flights %>% select(year, month, day, carrier, origin))
```

    [90m# A tibble: 336,776 x 5[39m
        year month   day carrier origin
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<chr>[39m[23m   [3m[90m<chr>[39m[23m 
    [90m 1[39m  [4m2[24m013     1     1 UA      EWR   
    [90m 2[39m  [4m2[24m013     1     1 UA      LGA   
    [90m 3[39m  [4m2[24m013     1     1 AA      JFK   
    [90m 4[39m  [4m2[24m013     1     1 B6      JFK   
    [90m 5[39m  [4m2[24m013     1     1 DL      LGA   
    [90m 6[39m  [4m2[24m013     1     1 UA      EWR   
    [90m 7[39m  [4m2[24m013     1     1 B6      EWR   
    [90m 8[39m  [4m2[24m013     1     1 EV      LGA   
    [90m 9[39m  [4m2[24m013     1     1 B6      JFK   
    [90m10[39m  [4m2[24m013     1     1 AA      LGA   
    [90m# ... with 336,766 more rows[39m
    

And join by `origin` tht is the same as `faa` for the `airports` dataset.


```R
flights <- flights %>% left_join(airports, by = c("origin" = "faa")) 
```


```R
print(flights %>% select(year, month, day, carrier, origin, name.y))
```

    [90m# A tibble: 336,776 x 6[39m
        year month   day carrier origin name.y             
       [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<int>[39m[23m [3m[90m<chr>[39m[23m   [3m[90m<chr>[39m[23m  [3m[90m<chr>[39m[23m              
    [90m 1[39m  [4m2[24m013     1     1 UA      EWR    Newark Liberty Intl
    [90m 2[39m  [4m2[24m013     1     1 UA      LGA    La Guardia         
    [90m 3[39m  [4m2[24m013     1     1 AA      JFK    John F Kennedy Intl
    [90m 4[39m  [4m2[24m013     1     1 B6      JFK    John F Kennedy Intl
    [90m 5[39m  [4m2[24m013     1     1 DL      LGA    La Guardia         
    [90m 6[39m  [4m2[24m013     1     1 UA      EWR    Newark Liberty Intl
    [90m 7[39m  [4m2[24m013     1     1 B6      EWR    Newark Liberty Intl
    [90m 8[39m  [4m2[24m013     1     1 EV      LGA    La Guardia         
    [90m 9[39m  [4m2[24m013     1     1 B6      JFK    John F Kennedy Intl
    [90m10[39m  [4m2[24m013     1     1 AA      LGA    La Guardia         
    [90m# ... with 336,766 more rows[39m
    
