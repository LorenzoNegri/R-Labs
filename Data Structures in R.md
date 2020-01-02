# Looking at data structures - dataframes in R

Dataframes are the fundamental data structure in R. They are essentially tables consisting of variables and observations. If we can represent your dataset as a dataframe, we're ready to start answering questions about it using R.

Let's load `mpg` dataframe contained in the `tidyverse` package:


```R
library(tidyverse)
head(mpg) # visualize first lines of dataframe
cat("\n Number of rows of dataframe | Number of feautures:\n", dim(mpg),"\n"); cat("\n")
summary(mpg) # visualize descriptive stats of dataframe
```


<table>
<caption>A tibble: 6 × 11</caption>
<thead>
	<tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>audi</td><td>a4</td><td>1.8</td><td>1999</td><td>4</td><td>auto(l5)  </td><td>f</td><td>18</td><td>29</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>1.8</td><td>1999</td><td>4</td><td>manual(m5)</td><td>f</td><td>21</td><td>29</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.0</td><td>2008</td><td>4</td><td>manual(m6)</td><td>f</td><td>20</td><td>31</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.0</td><td>2008</td><td>4</td><td>auto(av)  </td><td>f</td><td>21</td><td>30</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.8</td><td>1999</td><td>6</td><td>auto(l5)  </td><td>f</td><td>16</td><td>26</td><td>p</td><td>compact</td></tr>
	<tr><td>audi</td><td>a4</td><td>2.8</td><td>1999</td><td>6</td><td>manual(m5)</td><td>f</td><td>18</td><td>26</td><td>p</td><td>compact</td></tr>
</tbody>
</table>



    
     Number of rows of dataframe | Number of feautures:
     234 11 
    
    


     manufacturer          model               displ            year     
     Length:234         Length:234         Min.   :1.600   Min.   :1999  
     Class :character   Class :character   1st Qu.:2.400   1st Qu.:1999  
     Mode  :character   Mode  :character   Median :3.300   Median :2004  
                                           Mean   :3.472   Mean   :2004  
                                           3rd Qu.:4.600   3rd Qu.:2008  
                                           Max.   :7.000   Max.   :2008  
          cyl           trans               drv                 cty       
     Min.   :4.000   Length:234         Length:234         Min.   : 9.00  
     1st Qu.:4.000   Class :character   Class :character   1st Qu.:14.00  
     Median :6.000   Mode  :character   Mode  :character   Median :17.00  
     Mean   :5.889                                         Mean   :16.86  
     3rd Qu.:8.000                                         3rd Qu.:19.00  
     Max.   :8.000                                         Max.   :35.00  
          hwy             fl               class          
     Min.   :12.00   Length:234         Length:234        
     1st Qu.:18.00   Class :character   Class :character  
     Median :24.00   Mode  :character   Mode  :character  
     Mean   :23.44                                        
     3rd Qu.:27.00                                        
     Max.   :44.00                                        


This dataframe contains data about the fuel economy of cars, collected between 1999 and 2008. Specifically, it's a table containing 234 rows, and 11 columns. It's called a "tibble" `tibble`, which is just a slightly modernised version of R's original `data.frame`.

We can create our own tibbles:


```R
mytibble <- tibble(
  x = 1:4, 
  y = x^2, 
  z = y + 0.1
)
mytibble
```


<table>
<caption>A tibble: 4 × 3</caption>
<thead>
	<tr><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td> 1</td><td> 1.1</td></tr>
	<tr><td>2</td><td> 4</td><td> 4.1</td></tr>
	<tr><td>3</td><td> 9</td><td> 9.1</td></tr>
	<tr><td>4</td><td>16</td><td>16.1</td></tr>
</tbody>
</table>



And also create "tribbles": they are just "transposed tibbles":


```R
mytribble <- tribble(
  ~x, ~y, ~z,
  1, 4.2,"a",
  3, 9.6,"b",
  4,16.8,"c"  
)
mytribble
```


<table>
<caption>A tibble: 3 × 3</caption>
<thead>
	<tr><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td> 4.2</td><td>a</td></tr>
	<tr><td>3</td><td> 9.6</td><td>b</td></tr>
	<tr><td>4</td><td>16.8</td><td>c</td></tr>
</tbody>
</table>



That might be easier, depending on the situation.

## Getting variables from a dataframe

If we want to see how many different types of car there are in the dataset, we can try using `unique`


```R
unique(mpg$model)
```


<ol class=list-inline>
	<li>'a4'</li>
	<li>'a4 quattro'</li>
	<li>'a6 quattro'</li>
	<li>'c1500 suburban 2wd'</li>
	<li>'corvette'</li>
	<li>'k1500 tahoe 4wd'</li>
	<li>'malibu'</li>
	<li>'caravan 2wd'</li>
	<li>'dakota pickup 4wd'</li>
	<li>'durango 4wd'</li>
	<li>'ram 1500 pickup 4wd'</li>
	<li>'expedition 2wd'</li>
	<li>'explorer 4wd'</li>
	<li>'f150 pickup 4wd'</li>
	<li>'mustang'</li>
	<li>'civic'</li>
	<li>'sonata'</li>
	<li>'tiburon'</li>
	<li>'grand cherokee 4wd'</li>
	<li>'range rover'</li>
	<li>'navigator 2wd'</li>
	<li>'mountaineer 4wd'</li>
	<li>'altima'</li>
	<li>'maxima'</li>
	<li>'pathfinder 4wd'</li>
	<li>'grand prix'</li>
	<li>'forester awd'</li>
	<li>'impreza awd'</li>
	<li>'4runner 4wd'</li>
	<li>'camry'</li>
	<li>'camry solara'</li>
	<li>'corolla'</li>
	<li>'land cruiser wagon 4wd'</li>
	<li>'toyota tacoma 4wd'</li>
	<li>'gti'</li>
	<li>'jetta'</li>
	<li>'new beetle'</li>
	<li>'passat'</li>
</ol>



There are 38 different types of cars in the dataset.

Other selection methods:


```R
# select specific row
mpg[194,]
```


<table>
<caption>A tibble: 1 × 11</caption>
<thead>
	<tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>1999</td><td>4</td><td>auto(l3)</td><td>f</td><td>24</td><td>30</td><td>r</td><td>compact</td></tr>
</tbody>
</table>




```R
# select row and column
mpg[194,2]
```


<table>
<caption>A tibble: 1 × 1</caption>
<thead>
	<tr><th scope=col>model</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>corolla</td></tr>
</tbody>
</table>




```R
# select a range of rows and columns
mpg[194:198,1:4]
```


<table>
<caption>A tibble: 5 × 4</caption>
<thead>
	<tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>1999</td></tr>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>1999</td></tr>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>1999</td></tr>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>2008</td></tr>
	<tr><td>toyota</td><td>corolla</td><td>1.8</td><td>2008</td></tr>
</tbody>
</table>



## Tables and barcharts in R

How many of each brand of car we have in dataframe?

One way to answer this is to make a table. Making a table of counts of each type is easy


```R
table(mpg$manufacturer)
```


    
          audi  chevrolet      dodge       ford      honda    hyundai       jeep 
            18         19         37         25          9         14          8 
    land rover    lincoln    mercury     nissan    pontiac     subaru     toyota 
             4          3          4         13          5         14         34 
    volkswagen 
            27 


This shows us that there are 18 Audis in the dataset, 19 Chevrolets, and so on. But we might like to know the *proportion* of each type of car. We can pass the table to the R function `prop.table` to convert all these numbers into proportions:


```R
prop.table(table(mpg$manufacturer))
```


    
          audi  chevrolet      dodge       ford      honda    hyundai       jeep 
    0.07692308 0.08119658 0.15811966 0.10683761 0.03846154 0.05982906 0.03418803 
    land rover    lincoln    mercury     nissan    pontiac     subaru     toyota 
    0.01709402 0.01282051 0.01709402 0.05555556 0.02136752 0.05982906 0.14529915 
    volkswagen 
    0.11538462 


So, now we know that about 15.8% of the cars are Dodges, and 10.7% are Fords. It might be nicer still to represent this information as a bar chart, so we don't have to read all those numbers. This is where we get `ggplot` package.


```R
ggplot(mpg,aes(manufacturer)) +
        geom_bar() +
        theme(text = element_text(size = 18), axis.text.x = element_text(angle = 90))
```


![png](output_25_0.png)

