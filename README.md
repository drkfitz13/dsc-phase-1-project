## Final Project Submission

Please fill out:
* Student name: Drake Fitzsimmons
* Student pace: Full-time
* Scheduled project review date/time: 3/23/21
* Instructor name: James Irving
* Blog post URL:


## Business Problem

Microsoft is looking for recommendations on what movies do the best at the box office. They will use this information to make informed decisions on starting a new movie studio. They would like actionable insights on past movie performance to help decide what types of films to create. This project will review data provided by the client from IMDb and Box OFfice Mojo to create recommendations that Microsoft can use in their new business venture.

## Obtain and Review Data

First, we must import libraries that we will use to analyze the data.


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

Next, let's read the data provided to us for the analysis. We will use 3 data sources to simplify the analysis.


```python
df_basics = pd.read_csv('./zippedData/imdb.title.basics.csv.gz')
df_ratings = pd.read_csv('./zippedData/imdb.title.ratings.csv.gz')
df_gross = pd.read_csv('./zippedData/bom.movie_gross.csv.gz')
```

Using pandas, the csv files are read. Let's display information to determine what we we have.


```python
print('\n'+'---'*25)
print('df_basics preview')
display(df_basics)
print('\n'+'---'*25)
print('df_ratings preview')
display(df_ratings)
print('\n'+'---'*25)
print('df_gross preview')
display(df_gross)
print('\n')
```

    
    ---------------------------------------------------------------------------
    df_basics preview
    


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>146139</th>
      <td>tt9916538</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>2019</td>
      <td>123.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>146140</th>
      <td>tt9916622</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
    <tr>
      <th>146141</th>
      <td>tt9916706</td>
      <td>Dankyavar Danka</td>
      <td>Dankyavar Danka</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Comedy</td>
    </tr>
    <tr>
      <th>146142</th>
      <td>tt9916730</td>
      <td>6 Gunn</td>
      <td>6 Gunn</td>
      <td>2017</td>
      <td>116.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>146143</th>
      <td>tt9916754</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
  </tbody>
</table>
<p>146144 rows × 6 columns</p>
</div>


    
    ---------------------------------------------------------------------------
    df_ratings preview
    


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt10356526</td>
      <td>8.3</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt10384606</td>
      <td>8.9</td>
      <td>559</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt1042974</td>
      <td>6.4</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt1043726</td>
      <td>4.2</td>
      <td>50352</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt1060240</td>
      <td>6.5</td>
      <td>21</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>73851</th>
      <td>tt9805820</td>
      <td>8.1</td>
      <td>25</td>
    </tr>
    <tr>
      <th>73852</th>
      <td>tt9844256</td>
      <td>7.5</td>
      <td>24</td>
    </tr>
    <tr>
      <th>73853</th>
      <td>tt9851050</td>
      <td>4.7</td>
      <td>14</td>
    </tr>
    <tr>
      <th>73854</th>
      <td>tt9886934</td>
      <td>7.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>73855</th>
      <td>tt9894098</td>
      <td>6.3</td>
      <td>128</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 3 columns</p>
</div>


    
    ---------------------------------------------------------------------------
    df_gross preview
    


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3382</th>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3383</th>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3384</th>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3385</th>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3386</th>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 5 columns</p>
</div>


    
    
    

We see that the dataframes are different sizes and have different columns, but there are some common columns on which we can combine the dataframes. We will explore the data further while we clean it for easier use.

## Data Cleaning

### Combining Basics & Ratings
First, the dataframes must be combined in order to compare across different variables. We know that there is a common column between df_basics and df_ratings. Let's start there.


```python
df_basics_ratings = df_basics.merge(df_ratings,how='inner',on='tconst')
df_basics_ratings
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.0</td>
      <td>77</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>4517</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
      <td>6.1</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>73851</th>
      <td>tt9913084</td>
      <td>Diabolik sono io</td>
      <td>Diabolik sono io</td>
      <td>2019</td>
      <td>75.0</td>
      <td>Documentary</td>
      <td>6.2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>73852</th>
      <td>tt9914286</td>
      <td>Sokagin Çocuklari</td>
      <td>Sokagin Çocuklari</td>
      <td>2019</td>
      <td>98.0</td>
      <td>Drama,Family</td>
      <td>8.7</td>
      <td>136</td>
    </tr>
    <tr>
      <th>73853</th>
      <td>tt9914642</td>
      <td>Albatross</td>
      <td>Albatross</td>
      <td>2017</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>8.5</td>
      <td>8</td>
    </tr>
    <tr>
      <th>73854</th>
      <td>tt9914942</td>
      <td>La vida sense la Sara Amat</td>
      <td>La vida sense la Sara Amat</td>
      <td>2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>73855</th>
      <td>tt9916160</td>
      <td>Drømmeland</td>
      <td>Drømmeland</td>
      <td>2019</td>
      <td>72.0</td>
      <td>Documentary</td>
      <td>6.5</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
<p>73856 rows × 8 columns</p>
</div>



df_basics_ratings is a new dataframe that combined df_basics and df_ratings. The total rows in df_basics_ratings is equal to the total number of rows in df_ratings. Presumably every row in df_ratings had a corresponding row in df_basics and all rows unique to df_basics have been removed. Let's check.


```python
print(df_basics_ratings.isna().sum())
df_dup_check = df_basics_ratings[df_basics_ratings.duplicated('tconst')]
df_dup_check
```

    tconst                0
    primary_title         0
    original_title        0
    start_year            0
    runtime_minutes    7620
    genres              804
    averagerating         0
    numvotes              0
    dtype: int64
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
df_dup_check = df_basics[df_basics.duplicated('tconst')]
df_dup_check
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
df_dup_check = df_ratings[df_ratings.duplicated('tconst')]
df_dup_check
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



We can see that there are no null values in the averagerating and numvotes columns. Also there are no duplicate tconst values in df_basics_ratings, df_basics, or df_ratings. This confirms that we removed rows from df_basics that did not have a corresponding tconst in df_ratings.

### Combining Revenue
Next we must combine df_gross to our newly created df_basics_ratings dataframe. Let's find a common column to combine with.


```python
print('\n'+'---'*25)
print('df_basics_ratings preview')
display(df_basics)
print('\n'+'---'*25)
print('df_gross preview')
display(df_gross)
```

    
    ---------------------------------------------------------------------------
    df_basics_ratings preview
    


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>146139</th>
      <td>tt9916538</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>Kuambil Lagi Hatiku</td>
      <td>2019</td>
      <td>123.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>146140</th>
      <td>tt9916622</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>Rodolpho Teóphilo - O Legado de um Pioneiro</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
    <tr>
      <th>146141</th>
      <td>tt9916706</td>
      <td>Dankyavar Danka</td>
      <td>Dankyavar Danka</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Comedy</td>
    </tr>
    <tr>
      <th>146142</th>
      <td>tt9916730</td>
      <td>6 Gunn</td>
      <td>6 Gunn</td>
      <td>2017</td>
      <td>116.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>146143</th>
      <td>tt9916754</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>Chico Albuquerque - Revelações</td>
      <td>2013</td>
      <td>NaN</td>
      <td>Documentary</td>
    </tr>
  </tbody>
</table>
<p>146144 rows × 6 columns</p>
</div>


    
    ---------------------------------------------------------------------------
    df_gross preview
    


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3382</th>
      <td>The Quake</td>
      <td>Magn.</td>
      <td>6200.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3383</th>
      <td>Edward II (2018 re-release)</td>
      <td>FM</td>
      <td>4800.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3384</th>
      <td>El Pacto</td>
      <td>Sony</td>
      <td>2500.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3385</th>
      <td>The Swan</td>
      <td>Synergetic</td>
      <td>2400.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3386</th>
      <td>An Actor Prepares</td>
      <td>Grav.</td>
      <td>1700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>3387 rows × 5 columns</p>
</div>


Common columns include title and year. Unfortunately in df_basics_ratings, there are two versions of title: primary_title and original_title. Ultimately we will have to check to see if titles in df_gross match with primary_title or original_title. But first let's see if there are any duplicates in df_gross that we should deal with.


```python
df_dup_check = None
df_dup_check = df_gross[df_gross.duplicated('title', keep = False)]
df_dup_check
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>317</th>
      <td>Bluebeard</td>
      <td>Strand</td>
      <td>33500.0</td>
      <td>5200</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3045</th>
      <td>Bluebeard</td>
      <td>WGUSA</td>
      <td>43100.0</td>
      <td>NaN</td>
      <td>2017</td>
    </tr>
  </tbody>
</table>
</div>



There are two movies with the title Bluebeard made by different studios in different years. There is no need to worry though, because we can merge df_gross with df_basics_ratings using two criteria, title and year. Since the years are different there shouldn't be a problem.

Let's merge df_gross with df_basics_ratings.


```python
df_combined = df_basics_ratings.merge(df_gross,how='inner',
                                      left_on = ['primary_title','start_year'],
                                      right_on = ['title','year'])
df_combined
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0315642</td>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Wazir</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0337692</td>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>On the Road</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0359950</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0365907</td>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>A Walk Among the Tombstones</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1842</th>
      <td>tt8290698</td>
      <td>The Spy Gone North</td>
      <td>Gongjak</td>
      <td>2018</td>
      <td>137.0</td>
      <td>Drama</td>
      <td>7.2</td>
      <td>1620</td>
      <td>The Spy Gone North</td>
      <td>CJ</td>
      <td>501000.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1843</th>
      <td>tt8404272</td>
      <td>How Long Will I Love U</td>
      <td>Chao shi kong tong ju</td>
      <td>2018</td>
      <td>101.0</td>
      <td>Romance</td>
      <td>6.5</td>
      <td>607</td>
      <td>How Long Will I Love U</td>
      <td>WGUSA</td>
      <td>747000.0</td>
      <td>82100000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1844</th>
      <td>tt8427036</td>
      <td>Helicopter Eela</td>
      <td>Helicopter Eela</td>
      <td>2018</td>
      <td>135.0</td>
      <td>Drama</td>
      <td>5.4</td>
      <td>673</td>
      <td>Helicopter Eela</td>
      <td>Eros</td>
      <td>72000.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1845</th>
      <td>tt9078374</td>
      <td>Last Letter</td>
      <td>Ni hao, Zhihua</td>
      <td>2018</td>
      <td>114.0</td>
      <td>Drama,Romance</td>
      <td>6.4</td>
      <td>322</td>
      <td>Last Letter</td>
      <td>CL</td>
      <td>181000.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1846</th>
      <td>tt9151704</td>
      <td>Burn the Stage: The Movie</td>
      <td>Burn the Stage: The Movie</td>
      <td>2018</td>
      <td>84.0</td>
      <td>Documentary,Music</td>
      <td>8.8</td>
      <td>2067</td>
      <td>Burn the Stage: The Movie</td>
      <td>Trafalgar</td>
      <td>4200000.0</td>
      <td>16100000</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>1847 rows × 13 columns</p>
</div>



Great, now we have a combined dataframe. But we only combined based on primary_title. Let's ensure that there weren't any movies which the original title would match with a movie in df_gross.


```python
df_unequal_titles = df_basics_ratings.loc[df_basics_ratings['primary_title'] != df_basics_ratings['original_title']]
df_unequal_titles
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
      <td>7.2</td>
      <td>43</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>6.5</td>
      <td>119</td>
    </tr>
    <tr>
      <th>8</th>
      <td>tt0154039</td>
      <td>So Much for Justice!</td>
      <td>Oda az igazság</td>
      <td>2010</td>
      <td>100.0</td>
      <td>History</td>
      <td>4.6</td>
      <td>64</td>
    </tr>
    <tr>
      <th>10</th>
      <td>tt0162942</td>
      <td>Children of the Green Dragon</td>
      <td>A zöld sárkány gyermekei</td>
      <td>2010</td>
      <td>89.0</td>
      <td>Drama</td>
      <td>6.9</td>
      <td>120</td>
    </tr>
    <tr>
      <th>12</th>
      <td>tt0176694</td>
      <td>The Tragedy of Man</td>
      <td>Az ember tragédiája</td>
      <td>2011</td>
      <td>160.0</td>
      <td>Animation,Drama,History</td>
      <td>7.8</td>
      <td>584</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>73804</th>
      <td>tt9875852</td>
      <td>The House Elf</td>
      <td>Domovoy</td>
      <td>2019</td>
      <td>90.0</td>
      <td>Comedy,Family,Fantasy</td>
      <td>5.8</td>
      <td>50</td>
    </tr>
    <tr>
      <th>73824</th>
      <td>tt9894394</td>
      <td>Upin &amp; Ipin: The Lone Gibbon Kris</td>
      <td>Upin &amp; Ipin: Keris Siamang Tunggal</td>
      <td>2019</td>
      <td>100.0</td>
      <td>Animation</td>
      <td>8.1</td>
      <td>301</td>
    </tr>
    <tr>
      <th>73829</th>
      <td>tt9899840</td>
      <td>Auntie Frog</td>
      <td>Khaleh Ghurbagheh</td>
      <td>2018</td>
      <td>81.0</td>
      <td>Adventure,Comedy,Family</td>
      <td>6.2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>73830</th>
      <td>tt9899850</td>
      <td>The Agitation</td>
      <td>Ashoftegi</td>
      <td>2019</td>
      <td>NaN</td>
      <td>Drama,Thriller</td>
      <td>4.9</td>
      <td>14</td>
    </tr>
    <tr>
      <th>73831</th>
      <td>tt9899860</td>
      <td>Watching This Movie Is a Crime</td>
      <td>Didan in film jorm ast</td>
      <td>2019</td>
      <td>100.0</td>
      <td>Drama,Thriller</td>
      <td>8.1</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
<p>12173 rows × 8 columns</p>
</div>



There are 12,173 movies where the primary_title does not equal the original_title. Let's see which of these movies match with df_gross.


```python
df_unequal_titles = df_unequal_titles.merge(df_gross,how='inner',
                                      left_on = ['original_title','start_year'],
                                      right_on = ['title','year'])
df_unequal_titles
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt1139592</td>
      <td>Jeepers Creepers III</td>
      <td>Jeepers Creepers 3</td>
      <td>2017</td>
      <td>100.0</td>
      <td>Action,Horror,Mystery</td>
      <td>4.1</td>
      <td>14586</td>
      <td>Jeepers Creepers 3</td>
      <td>Scre.</td>
      <td>2300000.0</td>
      <td>NaN</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt1433813</td>
      <td>Hubble</td>
      <td>Hubble 3D</td>
      <td>2010</td>
      <td>44.0</td>
      <td>Documentary</td>
      <td>7.7</td>
      <td>3945</td>
      <td>Hubble 3D</td>
      <td>WB</td>
      <td>52400000.0</td>
      <td>21500000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt1477076</td>
      <td>Saw 3D: The Final Chapter</td>
      <td>Saw 3D</td>
      <td>2010</td>
      <td>90.0</td>
      <td>Crime,Horror,Mystery</td>
      <td>5.6</td>
      <td>83532</td>
      <td>Saw 3D</td>
      <td>LGF</td>
      <td>45700000.0</td>
      <td>90400000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt1620719</td>
      <td>Fearless</td>
      <td>Dabangg</td>
      <td>2010</td>
      <td>126.0</td>
      <td>Action,Comedy,Crime</td>
      <td>6.3</td>
      <td>25201</td>
      <td>Dabangg</td>
      <td>Eros</td>
      <td>1300000.0</td>
      <td>1700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt1629376</td>
      <td>Seven Sins Forgiven</td>
      <td>7 Khoon Maaf</td>
      <td>2011</td>
      <td>137.0</td>
      <td>Drama,Mystery,Thriller</td>
      <td>6.1</td>
      <td>4948</td>
      <td>7 Khoon Maaf</td>
      <td>UTV</td>
      <td>270000.0</td>
      <td>94700</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>5</th>
      <td>tt3148502</td>
      <td>The Spectacle</td>
      <td>Tamasha</td>
      <td>2015</td>
      <td>139.0</td>
      <td>Comedy,Drama,Romance</td>
      <td>7.2</td>
      <td>19901</td>
      <td>Tamasha</td>
      <td>UTV</td>
      <td>2100000.0</td>
      <td>NaN</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>6</th>
      <td>tt3173910</td>
      <td>She Smiles, She's Snared!</td>
      <td>Hasee Toh Phasee</td>
      <td>2014</td>
      <td>141.0</td>
      <td>Comedy,Romance</td>
      <td>6.8</td>
      <td>12406</td>
      <td>Hasee Toh Phasee</td>
      <td>Relbig.</td>
      <td>646000.0</td>
      <td>NaN</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>7</th>
      <td>tt3381068</td>
      <td>Gnome Alone</td>
      <td>Legend</td>
      <td>2015</td>
      <td>94.0</td>
      <td>Horror</td>
      <td>3.5</td>
      <td>303</td>
      <td>Legend</td>
      <td>Uni.</td>
      <td>1900000.0</td>
      <td>41100000</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>8</th>
      <td>tt3595298</td>
      <td>Found a Treasure Called Love</td>
      <td>Prem Ratan Dhan Payo</td>
      <td>2015</td>
      <td>164.0</td>
      <td>Action,Drama,Family</td>
      <td>4.6</td>
      <td>15753</td>
      <td>Prem Ratan Dhan Payo</td>
      <td>FIP</td>
      <td>4400000.0</td>
      <td>48200000</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>9</th>
      <td>tt3698118</td>
      <td>Gangnam Blues</td>
      <td>Gangnam 1970</td>
      <td>2015</td>
      <td>135.0</td>
      <td>Action,Drama</td>
      <td>6.3</td>
      <td>1291</td>
      <td>Gangnam 1970</td>
      <td>CJ</td>
      <td>18000.0</td>
      <td>NaN</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>10</th>
      <td>tt3833746</td>
      <td>The Visit: An Alien Encounter</td>
      <td>The Visit</td>
      <td>2015</td>
      <td>83.0</td>
      <td>Documentary</td>
      <td>6.0</td>
      <td>661</td>
      <td>The Visit</td>
      <td>Uni.</td>
      <td>65200000.0</td>
      <td>33200000</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>11</th>
      <td>tt4410000</td>
      <td>Luis and the Aliens</td>
      <td>Luis &amp; the Aliens</td>
      <td>2018</td>
      <td>86.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.0</td>
      <td>1355</td>
      <td>Luis &amp; the Aliens</td>
      <td>VPD</td>
      <td>170000.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>12</th>
      <td>tt4500922</td>
      <td>The Death Cure</td>
      <td>Maze Runner: The Death Cure</td>
      <td>2018</td>
      <td>143.0</td>
      <td>Action,Sci-Fi,Thriller</td>
      <td>6.2</td>
      <td>91286</td>
      <td>Maze Runner: The Death Cure</td>
      <td>Fox</td>
      <td>58000000.0</td>
      <td>230200000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>13</th>
      <td>tt5162658</td>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Oro</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>14</th>
      <td>tt6217804</td>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>15</th>
      <td>tt6588966</td>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Hichki</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>16</th>
      <td>tt7690670</td>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Superfly</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>17</th>
      <td>tt8549902</td>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>Oolong Courtyard</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
</div>



There are 18 movies that match based on original_title and not primary_title. We should include these in the combined dataframe.


```python
dataframes = [df_combined, df_unequal_titles]
df_combined = pd.concat(dataframes)
df_combined
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0315642</td>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Wazir</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0337692</td>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>On the Road</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0359950</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0365907</td>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>A Walk Among the Tombstones</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0369610</td>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Jurassic World</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>tt5162658</td>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Oro</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>14</th>
      <td>tt6217804</td>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>15</th>
      <td>tt6588966</td>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Hichki</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>16</th>
      <td>tt7690670</td>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Superfly</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>17</th>
      <td>tt8549902</td>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>Oolong Courtyard</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>1865 rows × 13 columns</p>
</div>



Now we have all the data we need combined into 1 dataframe. Let's clean up our column names and then dig into any data we need to clean up.

### Removing Unused Columns
There are columns which are now duplicates and columns that don't really mean much to us. Let's remove tconst because it doesn't have relevant meaning as well as title and year because they are just duplicate columns now.


```python
# del df_combined['tconst']
# del df_combined['title']
# del df_combined['year']
df_combined.drop(columns=['tconst','title','year'],inplace=True)
df_combined
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1865 rows × 10 columns</p>
</div>



Great, that's easier to work with. Now let's take a deeper look into what data we actually have.

### Removing Duplicates
Now that we have all our data in one place let's see if we have any duplicate rows.


```python
df_dup_check = None
df_dup_check = df_combined[df_combined.duplicated('primary_title', keep = False)]
df_dup_check
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>140</th>
      <td>The Bounty Hunter</td>
      <td>The Bounty Hunter</td>
      <td>2010</td>
      <td>110.0</td>
      <td>Action,Comedy,Romance</td>
      <td>5.6</td>
      <td>112444</td>
      <td>Sony</td>
      <td>67099999.0</td>
      <td>69300000</td>
    </tr>
    <tr>
      <th>141</th>
      <td>The Bounty Hunter</td>
      <td>The Bounty Hunter</td>
      <td>2010</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.3</td>
      <td>29</td>
      <td>Sony</td>
      <td>67099999.0</td>
      <td>69300000</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Big Eyes</td>
      <td>Big Eyes</td>
      <td>2014</td>
      <td>106.0</td>
      <td>Biography,Crime,Drama</td>
      <td>7.0</td>
      <td>77447</td>
      <td>Wein.</td>
      <td>14500000.0</td>
      <td>14800000</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Big Eyes</td>
      <td>Big Eyes</td>
      <td>2014</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>7.2</td>
      <td>43</td>
      <td>Wein.</td>
      <td>14500000.0</td>
      <td>14800000</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Burlesque</td>
      <td>Burlesque</td>
      <td>2010</td>
      <td>119.0</td>
      <td>Drama,Music,Musical</td>
      <td>6.4</td>
      <td>71021</td>
      <td>SGem</td>
      <td>39400000.0</td>
      <td>50100000</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Burlesque</td>
      <td>Burlesque</td>
      <td>2010</td>
      <td>NaN</td>
      <td>Drama</td>
      <td>7.0</td>
      <td>45</td>
      <td>SGem</td>
      <td>39400000.0</td>
      <td>50100000</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Jai Ho</td>
      <td>Jai Ho</td>
      <td>2014</td>
      <td>135.0</td>
      <td>Action,Drama</td>
      <td>5.3</td>
      <td>15531</td>
      <td>Eros</td>
      <td>1300000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Jai Ho</td>
      <td>Jai Ho</td>
      <td>2014</td>
      <td>60.0</td>
      <td>Biography,Documentary,Music</td>
      <td>7.6</td>
      <td>34</td>
      <td>Eros</td>
      <td>1300000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>225</th>
      <td>Leap Year</td>
      <td>Leap Year</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Romance</td>
      <td>6.5</td>
      <td>86125</td>
      <td>Uni.</td>
      <td>25900000.0</td>
      <td>6700000</td>
    </tr>
    <tr>
      <th>226</th>
      <td>Leap Year</td>
      <td>Año bisiesto</td>
      <td>2010</td>
      <td>94.0</td>
      <td>Drama,Romance</td>
      <td>5.9</td>
      <td>2211</td>
      <td>Uni.</td>
      <td>25900000.0</td>
      <td>6700000</td>
    </tr>
    <tr>
      <th>272</th>
      <td>The Tempest</td>
      <td>The Tempest</td>
      <td>2010</td>
      <td>110.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>7073</td>
      <td>Mira.</td>
      <td>278000.0</td>
      <td>68700</td>
    </tr>
    <tr>
      <th>273</th>
      <td>The Tempest</td>
      <td>The Tempest</td>
      <td>2010</td>
      <td>131.0</td>
      <td>Drama</td>
      <td>7.8</td>
      <td>94</td>
      <td>Mira.</td>
      <td>278000.0</td>
      <td>68700</td>
    </tr>
    <tr>
      <th>319</th>
      <td>Cyrus</td>
      <td>Cyrus</td>
      <td>2010</td>
      <td>87.0</td>
      <td>Crime,Horror,Mystery</td>
      <td>4.7</td>
      <td>944</td>
      <td>FoxS</td>
      <td>7500000.0</td>
      <td>2500000</td>
    </tr>
    <tr>
      <th>320</th>
      <td>Cyrus</td>
      <td>Cyrus</td>
      <td>2010</td>
      <td>91.0</td>
      <td>Comedy,Drama,Romance</td>
      <td>6.3</td>
      <td>32457</td>
      <td>FoxS</td>
      <td>7500000.0</td>
      <td>2500000</td>
    </tr>
    <tr>
      <th>335</th>
      <td>I'm Still Here</td>
      <td>I'm Still Here</td>
      <td>2010</td>
      <td>108.0</td>
      <td>Comedy,Drama,Music</td>
      <td>6.2</td>
      <td>18606</td>
      <td>Magn.</td>
      <td>409000.0</td>
      <td>160000</td>
    </tr>
    <tr>
      <th>336</th>
      <td>I'm Still Here</td>
      <td>I'm Still Here</td>
      <td>2010</td>
      <td>60.0</td>
      <td>NaN</td>
      <td>7.1</td>
      <td>14</td>
      <td>Magn.</td>
      <td>409000.0</td>
      <td>160000</td>
    </tr>
    <tr>
      <th>497</th>
      <td>A Better Life</td>
      <td>A Better Life</td>
      <td>2011</td>
      <td>98.0</td>
      <td>Drama,Romance</td>
      <td>7.2</td>
      <td>14602</td>
      <td>Sum.</td>
      <td>1800000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>498</th>
      <td>A Better Life</td>
      <td>Une vie meilleure</td>
      <td>2011</td>
      <td>110.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>1519</td>
      <td>Sum.</td>
      <td>1800000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>547</th>
      <td>Abduction</td>
      <td>Abduction</td>
      <td>2011</td>
      <td>106.0</td>
      <td>Action,Mystery,Thriller</td>
      <td>5.1</td>
      <td>72552</td>
      <td>LGF</td>
      <td>28100000.0</td>
      <td>54000000</td>
    </tr>
    <tr>
      <th>548</th>
      <td>Abduction</td>
      <td>Abduction</td>
      <td>2011</td>
      <td>84.0</td>
      <td>Horror,Thriller</td>
      <td>5.2</td>
      <td>10</td>
      <td>LGF</td>
      <td>28100000.0</td>
      <td>54000000</td>
    </tr>
    <tr>
      <th>616</th>
      <td>The Artist</td>
      <td>The Artist</td>
      <td>2011</td>
      <td>100.0</td>
      <td>Comedy,Drama,Romance</td>
      <td>7.9</td>
      <td>217184</td>
      <td>Wein.</td>
      <td>44700000.0</td>
      <td>88800000</td>
    </tr>
    <tr>
      <th>617</th>
      <td>The Artist</td>
      <td>The Artist</td>
      <td>2011</td>
      <td>100.0</td>
      <td>Thriller</td>
      <td>6.8</td>
      <td>6</td>
      <td>Wein.</td>
      <td>44700000.0</td>
      <td>88800000</td>
    </tr>
    <tr>
      <th>743</th>
      <td>Gone</td>
      <td>Gone</td>
      <td>2012</td>
      <td>94.0</td>
      <td>Action,Adventure,Drama</td>
      <td>5.9</td>
      <td>39482</td>
      <td>LG/S</td>
      <td>11700000.0</td>
      <td>6400000</td>
    </tr>
    <tr>
      <th>744</th>
      <td>Gone</td>
      <td>Gone</td>
      <td>2012</td>
      <td>50.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>7</td>
      <td>LG/S</td>
      <td>11700000.0</td>
      <td>6400000</td>
    </tr>
    <tr>
      <th>774</th>
      <td>The Darkness</td>
      <td>The Darkness</td>
      <td>2016</td>
      <td>92.0</td>
      <td>Horror,Thriller</td>
      <td>4.4</td>
      <td>12435</td>
      <td>BH Tilt</td>
      <td>10800000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>775</th>
      <td>The Darkness</td>
      <td>Las tinieblas</td>
      <td>2016</td>
      <td>92.0</td>
      <td>Thriller</td>
      <td>5.7</td>
      <td>196</td>
      <td>BH Tilt</td>
      <td>10800000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>781</th>
      <td>Spotlight</td>
      <td>Spotlight</td>
      <td>2015</td>
      <td>129.0</td>
      <td>Crime,Drama</td>
      <td>8.1</td>
      <td>365110</td>
      <td>ORF</td>
      <td>45100000.0</td>
      <td>53200000</td>
    </tr>
    <tr>
      <th>782</th>
      <td>Spotlight</td>
      <td>Spotlight</td>
      <td>2015</td>
      <td>99.0</td>
      <td>Drama</td>
      <td>8.0</td>
      <td>12</td>
      <td>ORF</td>
      <td>45100000.0</td>
      <td>53200000</td>
    </tr>
    <tr>
      <th>786</th>
      <td>The Call</td>
      <td>The Call</td>
      <td>2013</td>
      <td>94.0</td>
      <td>Crime,Drama,Horror</td>
      <td>6.7</td>
      <td>103780</td>
      <td>TriS</td>
      <td>51900000.0</td>
      <td>16700000</td>
    </tr>
    <tr>
      <th>787</th>
      <td>The Call</td>
      <td>Lokroep</td>
      <td>2013</td>
      <td>25.0</td>
      <td>Documentary</td>
      <td>7.9</td>
      <td>12</td>
      <td>TriS</td>
      <td>51900000.0</td>
      <td>16700000</td>
    </tr>
    <tr>
      <th>932</th>
      <td>The Walk</td>
      <td>The Walk</td>
      <td>2015</td>
      <td>89.0</td>
      <td>Crime,Thriller</td>
      <td>6.8</td>
      <td>126</td>
      <td>TriS</td>
      <td>10100000.0</td>
      <td>51000000</td>
    </tr>
    <tr>
      <th>933</th>
      <td>The Walk</td>
      <td>The Walk</td>
      <td>2015</td>
      <td>123.0</td>
      <td>Adventure,Biography,Drama</td>
      <td>7.3</td>
      <td>109714</td>
      <td>TriS</td>
      <td>10100000.0</td>
      <td>51000000</td>
    </tr>
    <tr>
      <th>993</th>
      <td>True Story</td>
      <td>True Story</td>
      <td>2015</td>
      <td>99.0</td>
      <td>Crime,Drama,Mystery</td>
      <td>6.3</td>
      <td>52063</td>
      <td>FoxS</td>
      <td>4700000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>994</th>
      <td>True Story</td>
      <td>True Story</td>
      <td>2015</td>
      <td>93.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>21</td>
      <td>FoxS</td>
      <td>4700000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1024</th>
      <td>Homefront</td>
      <td>Homefront</td>
      <td>2013</td>
      <td>100.0</td>
      <td>Action,Thriller</td>
      <td>6.5</td>
      <td>98549</td>
      <td>ORF</td>
      <td>20200000.0</td>
      <td>22900000</td>
    </tr>
    <tr>
      <th>1025</th>
      <td>Homefront</td>
      <td>The Things We Leave Behind</td>
      <td>2013</td>
      <td>111.0</td>
      <td>Drama</td>
      <td>7.7</td>
      <td>17</td>
      <td>ORF</td>
      <td>20200000.0</td>
      <td>22900000</td>
    </tr>
    <tr>
      <th>1069</th>
      <td>Coco</td>
      <td>Coco</td>
      <td>2017</td>
      <td>105.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.4</td>
      <td>277194</td>
      <td>BV</td>
      <td>209700000.0</td>
      <td>597400000</td>
    </tr>
    <tr>
      <th>1070</th>
      <td>Coco</td>
      <td>Coco</td>
      <td>2017</td>
      <td>98.0</td>
      <td>Horror</td>
      <td>7.4</td>
      <td>35</td>
      <td>BV</td>
      <td>209700000.0</td>
      <td>597400000</td>
    </tr>
    <tr>
      <th>1294</th>
      <td>Extraction</td>
      <td>Extraction</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Action,Crime,Thriller</td>
      <td>5.1</td>
      <td>16</td>
      <td>LGP</td>
      <td>16800.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1295</th>
      <td>Extraction</td>
      <td>Extraction</td>
      <td>2015</td>
      <td>92.0</td>
      <td>Action,Adventure,Crime</td>
      <td>4.1</td>
      <td>8564</td>
      <td>LGP</td>
      <td>16800.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>The Forest</td>
      <td>The Forest</td>
      <td>2016</td>
      <td>93.0</td>
      <td>Horror,Mystery,Thriller</td>
      <td>4.8</td>
      <td>36423</td>
      <td>Focus</td>
      <td>26600000.0</td>
      <td>11000000</td>
    </tr>
    <tr>
      <th>1307</th>
      <td>The Forest</td>
      <td>The Forest</td>
      <td>2016</td>
      <td>109.0</td>
      <td>Drama,Fantasy,Horror</td>
      <td>6.5</td>
      <td>222</td>
      <td>Focus</td>
      <td>26600000.0</td>
      <td>11000000</td>
    </tr>
    <tr>
      <th>1421</th>
      <td>Stronger</td>
      <td>Stronger</td>
      <td>2017</td>
      <td>119.0</td>
      <td>Biography,Drama</td>
      <td>7.0</td>
      <td>33464</td>
      <td>RAtt.</td>
      <td>4200000.0</td>
      <td>4200000</td>
    </tr>
    <tr>
      <th>1422</th>
      <td>Stronger</td>
      <td>Stronger</td>
      <td>2017</td>
      <td>125.0</td>
      <td>Drama</td>
      <td>6.8</td>
      <td>22</td>
      <td>RAtt.</td>
      <td>4200000.0</td>
      <td>4200000</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>Denial</td>
      <td>Denial</td>
      <td>2016</td>
      <td>109.0</td>
      <td>Biography,Drama</td>
      <td>6.7</td>
      <td>16593</td>
      <td>BST</td>
      <td>4099999.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>Denial</td>
      <td>Denial</td>
      <td>2016</td>
      <td>93.0</td>
      <td>Documentary</td>
      <td>7.4</td>
      <td>25</td>
      <td>BST</td>
      <td>4099999.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>Dancer</td>
      <td>Dancer</td>
      <td>2016</td>
      <td>85.0</td>
      <td>Biography,Documentary</td>
      <td>7.9</td>
      <td>2231</td>
      <td>IFC</td>
      <td>71900.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1612</th>
      <td>Dancer</td>
      <td>Dancer</td>
      <td>2016</td>
      <td>55.0</td>
      <td>Action</td>
      <td>8.6</td>
      <td>11</td>
      <td>IFC</td>
      <td>71900.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1615</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Wein.</td>
      <td>7200000.0</td>
      <td>7700000</td>
    </tr>
    <tr>
      <th>1787</th>
      <td>The Negotiation</td>
      <td>The Negotiation</td>
      <td>2018</td>
      <td>114.0</td>
      <td>Action,Crime,Thriller</td>
      <td>6.5</td>
      <td>926</td>
      <td>CJ</td>
      <td>111000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1788</th>
      <td>The Negotiation</td>
      <td>The Negotiation</td>
      <td>2018</td>
      <td>89.0</td>
      <td>Documentary,History,War</td>
      <td>7.6</td>
      <td>43</td>
      <td>CJ</td>
      <td>111000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



As you can see, there are quiet a few movies with the same title. But it's possible that some movies would have the same title. It's unlikely that the a studio would produce 2 movies with the same title in the same year. Let's make a new column to include title, year, and studio. 


```python
df_combined['title_year_studio'] = df_combined['primary_title'].astype(str)\
                                    + df_combined['start_year'].astype(str)\
                                    + df_combined['studio'].astype(str)
df_combined
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>title_year_studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
      <td>Wazir2016Relbig.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
      <td>On the Road2012IFC</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
      <td>The Secret Life of Walter Mitty2013Fox</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
      <td>A Walk Among the Tombstones2014Uni.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>Jurassic World2015Uni.</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
      <td>Gold2017Sony</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
      <td>Boo 2! A Madea Halloween2017LGF</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
      <td>Hiccup2018Yash</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
      <td>SuperFly2018Sony</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
      <td>Oolong Courtyard: KungFu School2018CL</td>
    </tr>
  </tbody>
</table>
<p>1865 rows × 11 columns</p>
</div>



Now, using our new column, let's check for duplicates.


```python
df_dup_check = None
df_combined.sort_values(by='numvotes')
df_combined.sort_values(by='primary_title')
df_dup_check = df_combined[df_combined.duplicated('title_year_studio', keep = False)]
df_dup_check
#sort values - by can be a list, inplace = True
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>title_year_studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>140</th>
      <td>The Bounty Hunter</td>
      <td>The Bounty Hunter</td>
      <td>2010</td>
      <td>110.0</td>
      <td>Action,Comedy,Romance</td>
      <td>5.6</td>
      <td>112444</td>
      <td>Sony</td>
      <td>67099999.0</td>
      <td>69300000</td>
      <td>The Bounty Hunter2010Sony</td>
    </tr>
    <tr>
      <th>141</th>
      <td>The Bounty Hunter</td>
      <td>The Bounty Hunter</td>
      <td>2010</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.3</td>
      <td>29</td>
      <td>Sony</td>
      <td>67099999.0</td>
      <td>69300000</td>
      <td>The Bounty Hunter2010Sony</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Big Eyes</td>
      <td>Big Eyes</td>
      <td>2014</td>
      <td>106.0</td>
      <td>Biography,Crime,Drama</td>
      <td>7.0</td>
      <td>77447</td>
      <td>Wein.</td>
      <td>14500000.0</td>
      <td>14800000</td>
      <td>Big Eyes2014Wein.</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Big Eyes</td>
      <td>Big Eyes</td>
      <td>2014</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>7.2</td>
      <td>43</td>
      <td>Wein.</td>
      <td>14500000.0</td>
      <td>14800000</td>
      <td>Big Eyes2014Wein.</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Burlesque</td>
      <td>Burlesque</td>
      <td>2010</td>
      <td>119.0</td>
      <td>Drama,Music,Musical</td>
      <td>6.4</td>
      <td>71021</td>
      <td>SGem</td>
      <td>39400000.0</td>
      <td>50100000</td>
      <td>Burlesque2010SGem</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Burlesque</td>
      <td>Burlesque</td>
      <td>2010</td>
      <td>NaN</td>
      <td>Drama</td>
      <td>7.0</td>
      <td>45</td>
      <td>SGem</td>
      <td>39400000.0</td>
      <td>50100000</td>
      <td>Burlesque2010SGem</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Jai Ho</td>
      <td>Jai Ho</td>
      <td>2014</td>
      <td>135.0</td>
      <td>Action,Drama</td>
      <td>5.3</td>
      <td>15531</td>
      <td>Eros</td>
      <td>1300000.0</td>
      <td>NaN</td>
      <td>Jai Ho2014Eros</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Jai Ho</td>
      <td>Jai Ho</td>
      <td>2014</td>
      <td>60.0</td>
      <td>Biography,Documentary,Music</td>
      <td>7.6</td>
      <td>34</td>
      <td>Eros</td>
      <td>1300000.0</td>
      <td>NaN</td>
      <td>Jai Ho2014Eros</td>
    </tr>
    <tr>
      <th>225</th>
      <td>Leap Year</td>
      <td>Leap Year</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Romance</td>
      <td>6.5</td>
      <td>86125</td>
      <td>Uni.</td>
      <td>25900000.0</td>
      <td>6700000</td>
      <td>Leap Year2010Uni.</td>
    </tr>
    <tr>
      <th>226</th>
      <td>Leap Year</td>
      <td>Año bisiesto</td>
      <td>2010</td>
      <td>94.0</td>
      <td>Drama,Romance</td>
      <td>5.9</td>
      <td>2211</td>
      <td>Uni.</td>
      <td>25900000.0</td>
      <td>6700000</td>
      <td>Leap Year2010Uni.</td>
    </tr>
    <tr>
      <th>272</th>
      <td>The Tempest</td>
      <td>The Tempest</td>
      <td>2010</td>
      <td>110.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>7073</td>
      <td>Mira.</td>
      <td>278000.0</td>
      <td>68700</td>
      <td>The Tempest2010Mira.</td>
    </tr>
    <tr>
      <th>273</th>
      <td>The Tempest</td>
      <td>The Tempest</td>
      <td>2010</td>
      <td>131.0</td>
      <td>Drama</td>
      <td>7.8</td>
      <td>94</td>
      <td>Mira.</td>
      <td>278000.0</td>
      <td>68700</td>
      <td>The Tempest2010Mira.</td>
    </tr>
    <tr>
      <th>319</th>
      <td>Cyrus</td>
      <td>Cyrus</td>
      <td>2010</td>
      <td>87.0</td>
      <td>Crime,Horror,Mystery</td>
      <td>4.7</td>
      <td>944</td>
      <td>FoxS</td>
      <td>7500000.0</td>
      <td>2500000</td>
      <td>Cyrus2010FoxS</td>
    </tr>
    <tr>
      <th>320</th>
      <td>Cyrus</td>
      <td>Cyrus</td>
      <td>2010</td>
      <td>91.0</td>
      <td>Comedy,Drama,Romance</td>
      <td>6.3</td>
      <td>32457</td>
      <td>FoxS</td>
      <td>7500000.0</td>
      <td>2500000</td>
      <td>Cyrus2010FoxS</td>
    </tr>
    <tr>
      <th>335</th>
      <td>I'm Still Here</td>
      <td>I'm Still Here</td>
      <td>2010</td>
      <td>108.0</td>
      <td>Comedy,Drama,Music</td>
      <td>6.2</td>
      <td>18606</td>
      <td>Magn.</td>
      <td>409000.0</td>
      <td>160000</td>
      <td>I'm Still Here2010Magn.</td>
    </tr>
    <tr>
      <th>336</th>
      <td>I'm Still Here</td>
      <td>I'm Still Here</td>
      <td>2010</td>
      <td>60.0</td>
      <td>NaN</td>
      <td>7.1</td>
      <td>14</td>
      <td>Magn.</td>
      <td>409000.0</td>
      <td>160000</td>
      <td>I'm Still Here2010Magn.</td>
    </tr>
    <tr>
      <th>497</th>
      <td>A Better Life</td>
      <td>A Better Life</td>
      <td>2011</td>
      <td>98.0</td>
      <td>Drama,Romance</td>
      <td>7.2</td>
      <td>14602</td>
      <td>Sum.</td>
      <td>1800000.0</td>
      <td>NaN</td>
      <td>A Better Life2011Sum.</td>
    </tr>
    <tr>
      <th>498</th>
      <td>A Better Life</td>
      <td>Une vie meilleure</td>
      <td>2011</td>
      <td>110.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>1519</td>
      <td>Sum.</td>
      <td>1800000.0</td>
      <td>NaN</td>
      <td>A Better Life2011Sum.</td>
    </tr>
    <tr>
      <th>547</th>
      <td>Abduction</td>
      <td>Abduction</td>
      <td>2011</td>
      <td>106.0</td>
      <td>Action,Mystery,Thriller</td>
      <td>5.1</td>
      <td>72552</td>
      <td>LGF</td>
      <td>28100000.0</td>
      <td>54000000</td>
      <td>Abduction2011LGF</td>
    </tr>
    <tr>
      <th>548</th>
      <td>Abduction</td>
      <td>Abduction</td>
      <td>2011</td>
      <td>84.0</td>
      <td>Horror,Thriller</td>
      <td>5.2</td>
      <td>10</td>
      <td>LGF</td>
      <td>28100000.0</td>
      <td>54000000</td>
      <td>Abduction2011LGF</td>
    </tr>
    <tr>
      <th>616</th>
      <td>The Artist</td>
      <td>The Artist</td>
      <td>2011</td>
      <td>100.0</td>
      <td>Comedy,Drama,Romance</td>
      <td>7.9</td>
      <td>217184</td>
      <td>Wein.</td>
      <td>44700000.0</td>
      <td>88800000</td>
      <td>The Artist2011Wein.</td>
    </tr>
    <tr>
      <th>617</th>
      <td>The Artist</td>
      <td>The Artist</td>
      <td>2011</td>
      <td>100.0</td>
      <td>Thriller</td>
      <td>6.8</td>
      <td>6</td>
      <td>Wein.</td>
      <td>44700000.0</td>
      <td>88800000</td>
      <td>The Artist2011Wein.</td>
    </tr>
    <tr>
      <th>743</th>
      <td>Gone</td>
      <td>Gone</td>
      <td>2012</td>
      <td>94.0</td>
      <td>Action,Adventure,Drama</td>
      <td>5.9</td>
      <td>39482</td>
      <td>LG/S</td>
      <td>11700000.0</td>
      <td>6400000</td>
      <td>Gone2012LG/S</td>
    </tr>
    <tr>
      <th>744</th>
      <td>Gone</td>
      <td>Gone</td>
      <td>2012</td>
      <td>50.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>7</td>
      <td>LG/S</td>
      <td>11700000.0</td>
      <td>6400000</td>
      <td>Gone2012LG/S</td>
    </tr>
    <tr>
      <th>774</th>
      <td>The Darkness</td>
      <td>The Darkness</td>
      <td>2016</td>
      <td>92.0</td>
      <td>Horror,Thriller</td>
      <td>4.4</td>
      <td>12435</td>
      <td>BH Tilt</td>
      <td>10800000.0</td>
      <td>NaN</td>
      <td>The Darkness2016BH Tilt</td>
    </tr>
    <tr>
      <th>775</th>
      <td>The Darkness</td>
      <td>Las tinieblas</td>
      <td>2016</td>
      <td>92.0</td>
      <td>Thriller</td>
      <td>5.7</td>
      <td>196</td>
      <td>BH Tilt</td>
      <td>10800000.0</td>
      <td>NaN</td>
      <td>The Darkness2016BH Tilt</td>
    </tr>
    <tr>
      <th>781</th>
      <td>Spotlight</td>
      <td>Spotlight</td>
      <td>2015</td>
      <td>129.0</td>
      <td>Crime,Drama</td>
      <td>8.1</td>
      <td>365110</td>
      <td>ORF</td>
      <td>45100000.0</td>
      <td>53200000</td>
      <td>Spotlight2015ORF</td>
    </tr>
    <tr>
      <th>782</th>
      <td>Spotlight</td>
      <td>Spotlight</td>
      <td>2015</td>
      <td>99.0</td>
      <td>Drama</td>
      <td>8.0</td>
      <td>12</td>
      <td>ORF</td>
      <td>45100000.0</td>
      <td>53200000</td>
      <td>Spotlight2015ORF</td>
    </tr>
    <tr>
      <th>786</th>
      <td>The Call</td>
      <td>The Call</td>
      <td>2013</td>
      <td>94.0</td>
      <td>Crime,Drama,Horror</td>
      <td>6.7</td>
      <td>103780</td>
      <td>TriS</td>
      <td>51900000.0</td>
      <td>16700000</td>
      <td>The Call2013TriS</td>
    </tr>
    <tr>
      <th>787</th>
      <td>The Call</td>
      <td>Lokroep</td>
      <td>2013</td>
      <td>25.0</td>
      <td>Documentary</td>
      <td>7.9</td>
      <td>12</td>
      <td>TriS</td>
      <td>51900000.0</td>
      <td>16700000</td>
      <td>The Call2013TriS</td>
    </tr>
    <tr>
      <th>932</th>
      <td>The Walk</td>
      <td>The Walk</td>
      <td>2015</td>
      <td>89.0</td>
      <td>Crime,Thriller</td>
      <td>6.8</td>
      <td>126</td>
      <td>TriS</td>
      <td>10100000.0</td>
      <td>51000000</td>
      <td>The Walk2015TriS</td>
    </tr>
    <tr>
      <th>933</th>
      <td>The Walk</td>
      <td>The Walk</td>
      <td>2015</td>
      <td>123.0</td>
      <td>Adventure,Biography,Drama</td>
      <td>7.3</td>
      <td>109714</td>
      <td>TriS</td>
      <td>10100000.0</td>
      <td>51000000</td>
      <td>The Walk2015TriS</td>
    </tr>
    <tr>
      <th>993</th>
      <td>True Story</td>
      <td>True Story</td>
      <td>2015</td>
      <td>99.0</td>
      <td>Crime,Drama,Mystery</td>
      <td>6.3</td>
      <td>52063</td>
      <td>FoxS</td>
      <td>4700000.0</td>
      <td>NaN</td>
      <td>True Story2015FoxS</td>
    </tr>
    <tr>
      <th>994</th>
      <td>True Story</td>
      <td>True Story</td>
      <td>2015</td>
      <td>93.0</td>
      <td>Drama</td>
      <td>6.6</td>
      <td>21</td>
      <td>FoxS</td>
      <td>4700000.0</td>
      <td>NaN</td>
      <td>True Story2015FoxS</td>
    </tr>
    <tr>
      <th>1024</th>
      <td>Homefront</td>
      <td>Homefront</td>
      <td>2013</td>
      <td>100.0</td>
      <td>Action,Thriller</td>
      <td>6.5</td>
      <td>98549</td>
      <td>ORF</td>
      <td>20200000.0</td>
      <td>22900000</td>
      <td>Homefront2013ORF</td>
    </tr>
    <tr>
      <th>1025</th>
      <td>Homefront</td>
      <td>The Things We Leave Behind</td>
      <td>2013</td>
      <td>111.0</td>
      <td>Drama</td>
      <td>7.7</td>
      <td>17</td>
      <td>ORF</td>
      <td>20200000.0</td>
      <td>22900000</td>
      <td>Homefront2013ORF</td>
    </tr>
    <tr>
      <th>1069</th>
      <td>Coco</td>
      <td>Coco</td>
      <td>2017</td>
      <td>105.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.4</td>
      <td>277194</td>
      <td>BV</td>
      <td>209700000.0</td>
      <td>597400000</td>
      <td>Coco2017BV</td>
    </tr>
    <tr>
      <th>1070</th>
      <td>Coco</td>
      <td>Coco</td>
      <td>2017</td>
      <td>98.0</td>
      <td>Horror</td>
      <td>7.4</td>
      <td>35</td>
      <td>BV</td>
      <td>209700000.0</td>
      <td>597400000</td>
      <td>Coco2017BV</td>
    </tr>
    <tr>
      <th>1294</th>
      <td>Extraction</td>
      <td>Extraction</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Action,Crime,Thriller</td>
      <td>5.1</td>
      <td>16</td>
      <td>LGP</td>
      <td>16800.0</td>
      <td>NaN</td>
      <td>Extraction2015LGP</td>
    </tr>
    <tr>
      <th>1295</th>
      <td>Extraction</td>
      <td>Extraction</td>
      <td>2015</td>
      <td>92.0</td>
      <td>Action,Adventure,Crime</td>
      <td>4.1</td>
      <td>8564</td>
      <td>LGP</td>
      <td>16800.0</td>
      <td>NaN</td>
      <td>Extraction2015LGP</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>The Forest</td>
      <td>The Forest</td>
      <td>2016</td>
      <td>93.0</td>
      <td>Horror,Mystery,Thriller</td>
      <td>4.8</td>
      <td>36423</td>
      <td>Focus</td>
      <td>26600000.0</td>
      <td>11000000</td>
      <td>The Forest2016Focus</td>
    </tr>
    <tr>
      <th>1307</th>
      <td>The Forest</td>
      <td>The Forest</td>
      <td>2016</td>
      <td>109.0</td>
      <td>Drama,Fantasy,Horror</td>
      <td>6.5</td>
      <td>222</td>
      <td>Focus</td>
      <td>26600000.0</td>
      <td>11000000</td>
      <td>The Forest2016Focus</td>
    </tr>
    <tr>
      <th>1421</th>
      <td>Stronger</td>
      <td>Stronger</td>
      <td>2017</td>
      <td>119.0</td>
      <td>Biography,Drama</td>
      <td>7.0</td>
      <td>33464</td>
      <td>RAtt.</td>
      <td>4200000.0</td>
      <td>4200000</td>
      <td>Stronger2017RAtt.</td>
    </tr>
    <tr>
      <th>1422</th>
      <td>Stronger</td>
      <td>Stronger</td>
      <td>2017</td>
      <td>125.0</td>
      <td>Drama</td>
      <td>6.8</td>
      <td>22</td>
      <td>RAtt.</td>
      <td>4200000.0</td>
      <td>4200000</td>
      <td>Stronger2017RAtt.</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>Denial</td>
      <td>Denial</td>
      <td>2016</td>
      <td>109.0</td>
      <td>Biography,Drama</td>
      <td>6.7</td>
      <td>16593</td>
      <td>BST</td>
      <td>4099999.0</td>
      <td>NaN</td>
      <td>Denial2016BST</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>Denial</td>
      <td>Denial</td>
      <td>2016</td>
      <td>93.0</td>
      <td>Documentary</td>
      <td>7.4</td>
      <td>25</td>
      <td>BST</td>
      <td>4099999.0</td>
      <td>NaN</td>
      <td>Denial2016BST</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>Dancer</td>
      <td>Dancer</td>
      <td>2016</td>
      <td>85.0</td>
      <td>Biography,Documentary</td>
      <td>7.9</td>
      <td>2231</td>
      <td>IFC</td>
      <td>71900.0</td>
      <td>NaN</td>
      <td>Dancer2016IFC</td>
    </tr>
    <tr>
      <th>1612</th>
      <td>Dancer</td>
      <td>Dancer</td>
      <td>2016</td>
      <td>55.0</td>
      <td>Action</td>
      <td>8.6</td>
      <td>11</td>
      <td>IFC</td>
      <td>71900.0</td>
      <td>NaN</td>
      <td>Dancer2016IFC</td>
    </tr>
    <tr>
      <th>1787</th>
      <td>The Negotiation</td>
      <td>The Negotiation</td>
      <td>2018</td>
      <td>114.0</td>
      <td>Action,Crime,Thriller</td>
      <td>6.5</td>
      <td>926</td>
      <td>CJ</td>
      <td>111000.0</td>
      <td>NaN</td>
      <td>The Negotiation2018CJ</td>
    </tr>
    <tr>
      <th>1788</th>
      <td>The Negotiation</td>
      <td>The Negotiation</td>
      <td>2018</td>
      <td>89.0</td>
      <td>Documentary,History,War</td>
      <td>7.6</td>
      <td>43</td>
      <td>CJ</td>
      <td>111000.0</td>
      <td>NaN</td>
      <td>The Negotiation2018CJ</td>
    </tr>
  </tbody>
</table>
</div>



There are still quite a few duplicate rows. When looking at the rows, it seems like the entry with the most votes also has the most complete data set. Let's remove the duplicates that have less votes.


```python
df_combined.sort_values(by='numvotes', ascending=False)
df_combined.drop_duplicates('title_year_studio', keep='first', inplace=True)
df_combined
#sort inplace=true
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>title_year_studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
      <td>Wazir2016Relbig.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
      <td>On the Road2012IFC</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
      <td>The Secret Life of Walter Mitty2013Fox</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
      <td>A Walk Among the Tombstones2014Uni.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
      <td>Jurassic World2015Uni.</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
      <td>Gold2017Sony</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
      <td>Boo 2! A Madea Halloween2017LGF</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
      <td>Hiccup2018Yash</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
      <td>SuperFly2018Sony</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
      <td>Oolong Courtyard: KungFu School2018CL</td>
    </tr>
  </tbody>
</table>
<p>1840 rows × 11 columns</p>
</div>



Let's remove the title_year_studio column we made. It's no longer needed.


```python
del df_combined['title_year_studio']
df_combined
#use drop
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1,019.4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1840 rows × 10 columns</p>
</div>



Now that we have all our data without duplicates, let's address any missing data.

### Missing Data
Let's see if there is any missing data which we should address before making comparisons.


```python
df_combined.info()
df_combined.isna().sum()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1840 entries, 0 to 17
    Data columns (total 10 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   primary_title    1840 non-null   object 
     1   original_title   1840 non-null   object 
     2   start_year       1840 non-null   int64  
     3   runtime_minutes  1839 non-null   float64
     4   genres           1840 non-null   object 
     5   averagerating    1840 non-null   float64
     6   numvotes         1840 non-null   int64  
     7   studio           1838 non-null   object 
     8   domestic_gross   1830 non-null   float64
     9   foreign_gross    1263 non-null   object 
    dtypes: float64(3), int64(2), object(5)
    memory usage: 158.1+ KB
    




    primary_title        0
    original_title       0
    start_year           0
    runtime_minutes      1
    genres               0
    averagerating        0
    numvotes             0
    studio               2
    domestic_gross      10
    foreign_gross      577
    dtype: int64



We see that there is missing data in runtime_minutes, studio, domestic_gross and foreign_gross columns. Let's address these issues one at a time.

#### Missing Runtime
Let's see which movie has a null runtime.


```python
df_combined[df_combined['runtime_minutes'].isna()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1294</th>
      <td>Extraction</td>
      <td>Extraction</td>
      <td>2015</td>
      <td>NaN</td>
      <td>Action,Crime,Thriller</td>
      <td>5.1</td>
      <td>16</td>
      <td>LGP</td>
      <td>16800.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



We don't have very many details about this movie, but there were only 16 votes on IMDB and it only made $16,800, so it's safe to say that we can remove this movie from our dataset.


```python
df_combined.drop(index=1294,inplace=True)
```

Let's check if our drop worked.


```python
df_combined[df_combined['runtime_minutes'].isna()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



We can see that Extraction is no longer included in our dataframe.

#### Missing Studio
As we did above with runtime, let's look at movies with missing studios.


```python
df_combined[df_combined['studio'].isna()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>931</th>
      <td>Keith Lemon: The Film</td>
      <td>Keith Lemon: The Film</td>
      <td>2012</td>
      <td>85.0</td>
      <td>Comedy</td>
      <td>2.6</td>
      <td>3950</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4000000</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>Secret Superstar</td>
      <td>Secret Superstar</td>
      <td>2017</td>
      <td>150.0</td>
      <td>Drama,Music</td>
      <td>8.0</td>
      <td>16563</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>122000000</td>
    </tr>
  </tbody>
</table>
</div>



We see that there are two films that were only foreign releases. Although these films likely won't make a huge impact on our data analysis, it won't hurt to keep them. We will use No Studio as an indicator that we don't have info about the studio. 


```python
df_combined['studio'].fillna('No Studio',inplace=True)
df_combined[df_combined['studio'] == 'No Studio']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>931</th>
      <td>Keith Lemon: The Film</td>
      <td>Keith Lemon: The Film</td>
      <td>2012</td>
      <td>85.0</td>
      <td>Comedy</td>
      <td>2.6</td>
      <td>3950</td>
      <td>No Studio</td>
      <td>NaN</td>
      <td>4000000</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>Secret Superstar</td>
      <td>Secret Superstar</td>
      <td>2017</td>
      <td>150.0</td>
      <td>Drama,Music</td>
      <td>8.0</td>
      <td>16563</td>
      <td>No Studio</td>
      <td>NaN</td>
      <td>122000000</td>
    </tr>
  </tbody>
</table>
</div>



Now we no longer have NaN for Studios

#### Missing Domestic Gross

Let's see which movie has a null domestic gross.


```python
df_combined[df_combined['domestic_gross'].isna()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>306</th>
      <td>It's a Wonderful Afterlife</td>
      <td>It's a Wonderful Afterlife</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>1361</td>
      <td>UTV</td>
      <td>NaN</td>
      <td>1300000</td>
    </tr>
    <tr>
      <th>466</th>
      <td>Dark Tide</td>
      <td>Dark Tide</td>
      <td>2012</td>
      <td>94.0</td>
      <td>Action,Adventure,Drama</td>
      <td>4.3</td>
      <td>7682</td>
      <td>WHE</td>
      <td>NaN</td>
      <td>432000</td>
    </tr>
    <tr>
      <th>513</th>
      <td>Celine: Through the Eyes of the World</td>
      <td>Celine: Through the Eyes of the World</td>
      <td>2010</td>
      <td>120.0</td>
      <td>Documentary,Music</td>
      <td>7.9</td>
      <td>349</td>
      <td>Sony</td>
      <td>NaN</td>
      <td>119000</td>
    </tr>
    <tr>
      <th>571</th>
      <td>White Lion</td>
      <td>White Lion</td>
      <td>2010</td>
      <td>88.0</td>
      <td>Drama,Family</td>
      <td>6.7</td>
      <td>828</td>
      <td>Scre.</td>
      <td>NaN</td>
      <td>99600</td>
    </tr>
    <tr>
      <th>621</th>
      <td>The Tall Man</td>
      <td>The Tall Man</td>
      <td>2012</td>
      <td>106.0</td>
      <td>Crime,Drama,Horror</td>
      <td>5.9</td>
      <td>36331</td>
      <td>Imag.</td>
      <td>NaN</td>
      <td>5200000</td>
    </tr>
    <tr>
      <th>844</th>
      <td>Force</td>
      <td>Force</td>
      <td>2011</td>
      <td>137.0</td>
      <td>Action,Thriller</td>
      <td>6.4</td>
      <td>6348</td>
      <td>FoxS</td>
      <td>NaN</td>
      <td>4800000</td>
    </tr>
    <tr>
      <th>931</th>
      <td>Keith Lemon: The Film</td>
      <td>Keith Lemon: The Film</td>
      <td>2012</td>
      <td>85.0</td>
      <td>Comedy</td>
      <td>2.6</td>
      <td>3950</td>
      <td>No Studio</td>
      <td>NaN</td>
      <td>4000000</td>
    </tr>
    <tr>
      <th>1011</th>
      <td>Jessabelle</td>
      <td>Jessabelle</td>
      <td>2014</td>
      <td>90.0</td>
      <td>Horror,Thriller</td>
      <td>5.4</td>
      <td>20552</td>
      <td>LGF</td>
      <td>NaN</td>
      <td>7000000</td>
    </tr>
    <tr>
      <th>1145</th>
      <td>Viral</td>
      <td>Viral</td>
      <td>2016</td>
      <td>85.0</td>
      <td>Drama,Horror,Sci-Fi</td>
      <td>5.5</td>
      <td>7150</td>
      <td>W/Dim.</td>
      <td>NaN</td>
      <td>552000</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>Secret Superstar</td>
      <td>Secret Superstar</td>
      <td>2017</td>
      <td>150.0</td>
      <td>Drama,Music</td>
      <td>8.0</td>
      <td>16563</td>
      <td>No Studio</td>
      <td>NaN</td>
      <td>122000000</td>
    </tr>
  </tbody>
</table>
</div>



All movies with NaN for domestic gross have foreign gross. We will assume these movies only had international releases and we will set NaN to 0. Let's first check to see if there are other movies with domestic gross = 0.


```python
df_combined[df_combined['domestic_gross']==0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



Great, we don't. So we don't have to worry about setting NaN to 0.


```python
df_combined['domestic_gross'].fillna(0,inplace=True)
df_combined[df_combined['domestic_gross'] == 0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>306</th>
      <td>It's a Wonderful Afterlife</td>
      <td>It's a Wonderful Afterlife</td>
      <td>2010</td>
      <td>100.0</td>
      <td>Comedy,Drama,Fantasy</td>
      <td>5.4</td>
      <td>1361</td>
      <td>UTV</td>
      <td>0.0</td>
      <td>1300000</td>
    </tr>
    <tr>
      <th>466</th>
      <td>Dark Tide</td>
      <td>Dark Tide</td>
      <td>2012</td>
      <td>94.0</td>
      <td>Action,Adventure,Drama</td>
      <td>4.3</td>
      <td>7682</td>
      <td>WHE</td>
      <td>0.0</td>
      <td>432000</td>
    </tr>
    <tr>
      <th>513</th>
      <td>Celine: Through the Eyes of the World</td>
      <td>Celine: Through the Eyes of the World</td>
      <td>2010</td>
      <td>120.0</td>
      <td>Documentary,Music</td>
      <td>7.9</td>
      <td>349</td>
      <td>Sony</td>
      <td>0.0</td>
      <td>119000</td>
    </tr>
    <tr>
      <th>571</th>
      <td>White Lion</td>
      <td>White Lion</td>
      <td>2010</td>
      <td>88.0</td>
      <td>Drama,Family</td>
      <td>6.7</td>
      <td>828</td>
      <td>Scre.</td>
      <td>0.0</td>
      <td>99600</td>
    </tr>
    <tr>
      <th>621</th>
      <td>The Tall Man</td>
      <td>The Tall Man</td>
      <td>2012</td>
      <td>106.0</td>
      <td>Crime,Drama,Horror</td>
      <td>5.9</td>
      <td>36331</td>
      <td>Imag.</td>
      <td>0.0</td>
      <td>5200000</td>
    </tr>
    <tr>
      <th>844</th>
      <td>Force</td>
      <td>Force</td>
      <td>2011</td>
      <td>137.0</td>
      <td>Action,Thriller</td>
      <td>6.4</td>
      <td>6348</td>
      <td>FoxS</td>
      <td>0.0</td>
      <td>4800000</td>
    </tr>
    <tr>
      <th>931</th>
      <td>Keith Lemon: The Film</td>
      <td>Keith Lemon: The Film</td>
      <td>2012</td>
      <td>85.0</td>
      <td>Comedy</td>
      <td>2.6</td>
      <td>3950</td>
      <td>No Studio</td>
      <td>0.0</td>
      <td>4000000</td>
    </tr>
    <tr>
      <th>1011</th>
      <td>Jessabelle</td>
      <td>Jessabelle</td>
      <td>2014</td>
      <td>90.0</td>
      <td>Horror,Thriller</td>
      <td>5.4</td>
      <td>20552</td>
      <td>LGF</td>
      <td>0.0</td>
      <td>7000000</td>
    </tr>
    <tr>
      <th>1145</th>
      <td>Viral</td>
      <td>Viral</td>
      <td>2016</td>
      <td>85.0</td>
      <td>Drama,Horror,Sci-Fi</td>
      <td>5.5</td>
      <td>7150</td>
      <td>W/Dim.</td>
      <td>0.0</td>
      <td>552000</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>Secret Superstar</td>
      <td>Secret Superstar</td>
      <td>2017</td>
      <td>150.0</td>
      <td>Drama,Music</td>
      <td>8.0</td>
      <td>16563</td>
      <td>No Studio</td>
      <td>0.0</td>
      <td>122000000</td>
    </tr>
  </tbody>
</table>
</div>



Mission accomplished!

#### Missing Foreign Gross

Finally, let's set all NaN under foreign_gross = 0. But first let's check if there are other foreign_gross = 0 which we should differentiate.


```python
df_combined[df_combined['foreign_gross']==0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



Looks good, now we can make our change.


```python
df_combined['foreign_gross'].fillna(0,inplace=True)
df_combined[df_combined['foreign_gross'] == 0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Crime,Drama</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>The Stanford Prison Experiment</td>
      <td>The Stanford Prison Experiment</td>
      <td>2015</td>
      <td>122.0</td>
      <td>Biography,Drama,History</td>
      <td>6.9</td>
      <td>32591</td>
      <td>IFC</td>
      <td>661000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Being Flynn</td>
      <td>Being Flynn</td>
      <td>2012</td>
      <td>102.0</td>
      <td>Drama</td>
      <td>6.4</td>
      <td>15780</td>
      <td>Focus</td>
      <td>540000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Margaret</td>
      <td>Margaret</td>
      <td>2011</td>
      <td>150.0</td>
      <td>Drama</td>
      <td>6.5</td>
      <td>14708</td>
      <td>FoxS</td>
      <td>46500.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>She Smiles, She's Snared!</td>
      <td>Hasee Toh Phasee</td>
      <td>2014</td>
      <td>141.0</td>
      <td>Comedy,Romance</td>
      <td>6.8</td>
      <td>12406</td>
      <td>Relbig.</td>
      <td>646000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Gangnam Blues</td>
      <td>Gangnam 1970</td>
      <td>2015</td>
      <td>135.0</td>
      <td>Action,Drama</td>
      <td>6.3</td>
      <td>1291</td>
      <td>CJ</td>
      <td>18000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Luis and the Aliens</td>
      <td>Luis &amp; the Aliens</td>
      <td>2018</td>
      <td>86.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.0</td>
      <td>1355</td>
      <td>VPD</td>
      <td>170000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>576 rows × 10 columns</p>
</div>



Looks like we accomplished our task!

#### Missing Data Confirmation

Great. Let's do one last check to see if we have any other missing data.


```python
df_combined.isna().sum()
```




    primary_title      0
    original_title     0
    start_year         0
    runtime_minutes    0
    genres             0
    averagerating      0
    numvotes           0
    studio             0
    domestic_gross     0
    foreign_gross      0
    dtype: int64



All right! All missing data is fixed up and we can move onto data formatting!

### Data Formatting
Let's take a look at the datatypes we have and see if we have to make any adjustments.


```python
df_combined.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1839 entries, 0 to 17
    Data columns (total 10 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   primary_title    1839 non-null   object 
     1   original_title   1839 non-null   object 
     2   start_year       1839 non-null   int64  
     3   runtime_minutes  1839 non-null   float64
     4   genres           1839 non-null   object 
     5   averagerating    1839 non-null   float64
     6   numvotes         1839 non-null   int64  
     7   studio           1839 non-null   object 
     8   domestic_gross   1839 non-null   float64
     9   foreign_gross    1839 non-null   object 
    dtypes: float64(3), int64(2), object(5)
    memory usage: 158.0+ KB
    


```python
df_compare=df_combined.copy()
```

We see that foreign_gross is an object. We should change that to a float in order to use it in conjunction with domestic_gross.

After further investigation, some foreign_gross objects contain commas. We will have to remove the commas in order to convert foreign_gross to a float.


```python
df_combined['foreign_gross'] = df_combined['foreign_gross'].str.replace(',','')
df_combined['foreign_gross'] = df_combined['foreign_gross'].astype(float)
df_combined.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1839 entries, 0 to 17
    Data columns (total 10 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   primary_title    1839 non-null   object 
     1   original_title   1839 non-null   object 
     2   start_year       1839 non-null   int64  
     3   runtime_minutes  1839 non-null   float64
     4   genres           1839 non-null   object 
     5   averagerating    1839 non-null   float64
     6   numvotes         1839 non-null   int64  
     7   studio           1839 non-null   object 
     8   domestic_gross   1839 non-null   float64
     9   foreign_gross    1263 non-null   float64
    dtypes: float64(4), int64(2), object(4)
    memory usage: 158.0+ KB
    


```python
null_rows = df_combined[df_combined['foreign_gross'].isna()].index
```


```python
df_compare.loc[null_rows]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Jeepers Creepers III</td>
      <td>Jeepers Creepers 3</td>
      <td>2017</td>
      <td>100.0</td>
      <td>Action,Horror,Mystery</td>
      <td>4.1</td>
      <td>14586</td>
      <td>Scre.</td>
      <td>2300000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Crime,Drama</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>She Smiles, She's Snared!</td>
      <td>Hasee Toh Phasee</td>
      <td>2014</td>
      <td>141.0</td>
      <td>Comedy,Romance</td>
      <td>6.8</td>
      <td>12406</td>
      <td>Relbig.</td>
      <td>646000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>The Stanford Prison Experiment</td>
      <td>The Stanford Prison Experiment</td>
      <td>2015</td>
      <td>122.0</td>
      <td>Biography,Drama,History</td>
      <td>6.9</td>
      <td>32591</td>
      <td>IFC</td>
      <td>661000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Luis and the Aliens</td>
      <td>Luis &amp; the Aliens</td>
      <td>2018</td>
      <td>86.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>6.0</td>
      <td>1355</td>
      <td>VPD</td>
      <td>170000.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>The A-Team</td>
      <td>The A-Team</td>
      <td>2010</td>
      <td>117.0</td>
      <td>Action,Adventure,Thriller</td>
      <td>6.8</td>
      <td>235256</td>
      <td>Fox</td>
      <td>77200000.0</td>
      <td>100000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Toy Story 3</td>
      <td>Toy Story 3</td>
      <td>2010</td>
      <td>103.0</td>
      <td>Adventure,Animation,Comedy</td>
      <td>8.3</td>
      <td>682218</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>586 rows × 10 columns</p>
</div>



Unfortunately the conversion made some of the foreign_gross values null. As we did before with null values, we will change them to 0.


```python
df_combined['foreign_gross'].fillna(0,inplace=True)
df_combined.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1839 entries, 0 to 17
    Data columns (total 10 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   primary_title    1839 non-null   object 
     1   original_title   1839 non-null   object 
     2   start_year       1839 non-null   int64  
     3   runtime_minutes  1839 non-null   float64
     4   genres           1839 non-null   object 
     5   averagerating    1839 non-null   float64
     6   numvotes         1839 non-null   int64  
     7   studio           1839 non-null   object 
     8   domestic_gross   1839 non-null   float64
     9   foreign_gross    1839 non-null   float64
    dtypes: float64(4), int64(2), object(4)
    memory usage: 238.0+ KB
    

Now there are no null values in the foreign_gross column and it is also formatted to float.

## Data Analysis
Now it's time to identify what trends make for successful movies.

### Total Gross Revenue
Microsoft may care about domestic gross revenue and foreign gross revenue separately, but the financial success of a movie depends on both domestic and foreign revenue. Let's combine the columns to make total gross revenue. We can also divide total gross revenue by 1 million. It will make the data easier to work with.


```python
df_combined['total_gross_MM'] = (df_combined['domestic_gross'] + df_combined['foreign_gross']) / 1000000
df_combined
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>total_gross_MM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.100000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the Road</td>
      <td>On the Road</td>
      <td>2012</td>
      <td>124.0</td>
      <td>Adventure,Drama,Romance</td>
      <td>6.1</td>
      <td>37886</td>
      <td>IFC</td>
      <td>744000.0</td>
      <td>8000000.0</td>
      <td>8.744000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Secret Life of Walter Mitty</td>
      <td>The Secret Life of Walter Mitty</td>
      <td>2013</td>
      <td>114.0</td>
      <td>Adventure,Comedy,Drama</td>
      <td>7.3</td>
      <td>275300</td>
      <td>Fox</td>
      <td>58200000.0</td>
      <td>129900000.0</td>
      <td>188.100000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A Walk Among the Tombstones</td>
      <td>A Walk Among the Tombstones</td>
      <td>2014</td>
      <td>114.0</td>
      <td>Action,Crime,Drama</td>
      <td>6.5</td>
      <td>105116</td>
      <td>Uni.</td>
      <td>26300000.0</td>
      <td>26900000.0</td>
      <td>53.200000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jurassic World</td>
      <td>Jurassic World</td>
      <td>2015</td>
      <td>124.0</td>
      <td>Action,Adventure,Sci-Fi</td>
      <td>7.0</td>
      <td>539338</td>
      <td>Uni.</td>
      <td>652300000.0</td>
      <td>1019.4</td>
      <td>652.301019</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>0.0</td>
      <td>0.005500</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000.0</td>
      <td>48.300000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000.0</td>
      <td>4.230000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.736000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0.0</td>
      <td>0.037700</td>
    </tr>
  </tbody>
</table>
<p>1839 rows × 11 columns</p>
</div>



Now that we have the total gross revenue, we can start to make some comparisons.

### Identifying Trends

Let's look at some general statistics.


```python
df_combined.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>total_gross_MM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1839.000000</td>
      <td>1839.000000</td>
      <td>1839.000000</td>
      <td>1.839000e+03</td>
      <td>1.839000e+03</td>
      <td>1.839000e+03</td>
      <td>1839.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2013.990212</td>
      <td>111.031539</td>
      <td>6.410386</td>
      <td>9.214169e+04</td>
      <td>4.256335e+07</td>
      <td>6.547470e+07</td>
      <td>108.038044</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.517601</td>
      <td>19.738803</td>
      <td>1.007737</td>
      <td>1.503749e+05</td>
      <td>7.715444e+07</td>
      <td>1.337304e+08</td>
      <td>200.893627</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2010.000000</td>
      <td>40.000000</td>
      <td>1.600000</td>
      <td>1.200000e+01</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000300</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2012.000000</td>
      <td>97.000000</td>
      <td>5.800000</td>
      <td>7.976500e+03</td>
      <td>5.465000e+05</td>
      <td>0.000000e+00</td>
      <td>1.300000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2014.000000</td>
      <td>107.000000</td>
      <td>6.500000</td>
      <td>3.605300e+04</td>
      <td>1.020000e+07</td>
      <td>8.600000e+06</td>
      <td>25.200000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2016.000000</td>
      <td>123.000000</td>
      <td>7.100000</td>
      <td>1.056950e+05</td>
      <td>5.195000e+07</td>
      <td>5.945000e+07</td>
      <td>113.700000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2018.000000</td>
      <td>189.000000</td>
      <td>8.800000</td>
      <td>1.841066e+06</td>
      <td>7.001000e+08</td>
      <td>9.464000e+08</td>
      <td>1405.400000</td>
    </tr>
  </tbody>
</table>
</div>



This gives a good overview of the data we are working with.

Now let's only use data for movies that came out after 2015. Microsoft will want recommendations based on recent data. We can use 3 years worth of data from 2016 to 2018.


```python
df_combined = df_combined[df_combined['start_year'] > 2015]
df_combined.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 604 entries, 0 to 17
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   primary_title    604 non-null    object 
     1   original_title   604 non-null    object 
     2   start_year       604 non-null    int64  
     3   runtime_minutes  604 non-null    float64
     4   genres           604 non-null    object 
     5   averagerating    604 non-null    float64
     6   numvotes         604 non-null    int64  
     7   studio           604 non-null    object 
     8   domestic_gross   604 non-null    float64
     9   foreign_gross    604 non-null    float64
     10  total_gross_MM   604 non-null    float64
    dtypes: float64(5), int64(2), object(4)
    memory usage: 56.6+ KB
    

By removing movies before 2016, our overall sample size has reduced to 604 movies.

## Business Recommendations

### Effect of Release Location on Total Revenue

Let's determine if release location is a good indicator on whether a movie will have a large revenue.

First let's make a column in the dataframe to help separate whether a movie is a was released only domestically or was released worldwide as well.


```python
release_location = []
for value in df_combined['foreign_gross']:
    if value == 0:
        release_location.append('Domestic')
    else:
        release_location.append('Worldwide')
df_combined['release_location'] = release_location
df_combined
```

    <ipython-input-41-4da9719db1fc>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_combined['release_location'] = release_location
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>total_gross_MM</th>
      <th>release_location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action,Crime,Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.1000</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Crime,Drama</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0.0</td>
      <td>0.5440</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Wonder Woman</td>
      <td>Wonder Woman</td>
      <td>2017</td>
      <td>141.0</td>
      <td>Action,Adventure,Fantasy</td>
      <td>7.5</td>
      <td>487527</td>
      <td>WB</td>
      <td>412600000.0</td>
      <td>409300000.0</td>
      <td>821.9000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>28</th>
      <td>The Only Living Boy in New York</td>
      <td>The Only Living Boy in New York</td>
      <td>2017</td>
      <td>89.0</td>
      <td>Drama</td>
      <td>6.3</td>
      <td>8727</td>
      <td>RAtt.</td>
      <td>624000.0</td>
      <td>1900000.0</td>
      <td>2.5240</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Hail, Caesar!</td>
      <td>Hail, Caesar!</td>
      <td>2016</td>
      <td>106.0</td>
      <td>Comedy,Drama,Music</td>
      <td>6.3</td>
      <td>111422</td>
      <td>Uni.</td>
      <td>30500000.0</td>
      <td>33100000.0</td>
      <td>63.6000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>Adventure,Drama,History</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>0.0</td>
      <td>0.0055</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>Comedy,Drama,Horror</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000.0</td>
      <td>48.3000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Comedy,Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000.0</td>
      <td>4.2300</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action,Crime,Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.7360</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0.0</td>
      <td>0.0377</td>
      <td>Domestic</td>
    </tr>
  </tbody>
</table>
<p>604 rows × 12 columns</p>
</div>



We have created our new column. Now let's see how many movies were released domestically vs. worldwide release.


```python
df_combined['release_location'].value_counts()
```




    Worldwide    393
    Domestic     211
    Name: release_location, dtype: int64



Luckily we have large enough sample size to make a determination. Let's create a visualization and see if we can draw conclusions.


```python
ax = sns.catplot(data=df_combined, x='release_location',y='total_gross_MM',
                height = 8,palette='dark')
ax.set(xlabel = 'Release Location', ylabel = 'Total Gross Revenue (MM USD)',
      title = 'Total Revenue vs. Release Location');
```


    
![png](output_107_0.png)
    


The graph clearly shows that movies released worldwide have a larger total gross revenue than movies only released domestically.

Microsoft should consider releasing their movies worldwide in order to make the most revenue.

### Effect of Genre on Total Revenue

Let's determine which movie genres produce the most revenue.

We must make some changes to the dataframe in order to do this comparison because the genre column has multiple genres for single movies. Let's make a row for each movie for each genre. That will make it easier to compare.


```python
df_combined['genres'] = df_combined['genres'].str.split(pat=',')
df_combined
```

    <ipython-input-44-f2d1653295b6>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_combined['genres'] = df_combined['genres'].str.split(pat=',')
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>total_gross_MM</th>
      <th>release_location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>[Action, Crime, Drama]</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.1000</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>[Crime, Drama]</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0.0</td>
      <td>0.5440</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Wonder Woman</td>
      <td>Wonder Woman</td>
      <td>2017</td>
      <td>141.0</td>
      <td>[Action, Adventure, Fantasy]</td>
      <td>7.5</td>
      <td>487527</td>
      <td>WB</td>
      <td>412600000.0</td>
      <td>409300000.0</td>
      <td>821.9000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>28</th>
      <td>The Only Living Boy in New York</td>
      <td>The Only Living Boy in New York</td>
      <td>2017</td>
      <td>89.0</td>
      <td>[Drama]</td>
      <td>6.3</td>
      <td>8727</td>
      <td>RAtt.</td>
      <td>624000.0</td>
      <td>1900000.0</td>
      <td>2.5240</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Hail, Caesar!</td>
      <td>Hail, Caesar!</td>
      <td>2016</td>
      <td>106.0</td>
      <td>[Comedy, Drama, Music]</td>
      <td>6.3</td>
      <td>111422</td>
      <td>Uni.</td>
      <td>30500000.0</td>
      <td>33100000.0</td>
      <td>63.6000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Gold</td>
      <td>Oro</td>
      <td>2017</td>
      <td>103.0</td>
      <td>[Adventure, Drama, History]</td>
      <td>5.6</td>
      <td>1103</td>
      <td>Sony</td>
      <td>5500.0</td>
      <td>0.0</td>
      <td>0.0055</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Boo 2! A Madea Halloween</td>
      <td>Tyler Perry's Boo 2! A Madea Halloween</td>
      <td>2017</td>
      <td>101.0</td>
      <td>[Comedy, Drama, Horror]</td>
      <td>3.8</td>
      <td>3383</td>
      <td>LGF</td>
      <td>47300000.0</td>
      <td>1000000.0</td>
      <td>48.3000</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>[Comedy, Drama]</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000.0</td>
      <td>4.2300</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>[Action, Crime, Thriller]</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.7360</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>[Comedy]</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0.0</td>
      <td>0.0377</td>
      <td>Domestic</td>
    </tr>
  </tbody>
</table>
<p>604 rows × 12 columns</p>
</div>



The step above made the genre column into a list with multiple genres instead of a string. Now we will explode the dataframe so each movie has a row for each genre.


```python
df_genres = None
df_genres = df_combined.explode('genres')
df_genres
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>averagerating</th>
      <th>numvotes</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>total_gross_MM</th>
      <th>release_location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Action</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.1000</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Crime</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.1000</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Wazir</td>
      <td>Wazir</td>
      <td>2016</td>
      <td>103.0</td>
      <td>Drama</td>
      <td>7.1</td>
      <td>15378</td>
      <td>Relbig.</td>
      <td>1100000.0</td>
      <td>0.0</td>
      <td>1.1000</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Crime</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0.0</td>
      <td>0.5440</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>6</th>
      <td>American Pastoral</td>
      <td>American Pastoral</td>
      <td>2016</td>
      <td>108.0</td>
      <td>Drama</td>
      <td>6.1</td>
      <td>12898</td>
      <td>LGF</td>
      <td>544000.0</td>
      <td>0.0</td>
      <td>0.5440</td>
      <td>Domestic</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hiccup</td>
      <td>Hichki</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Drama</td>
      <td>7.5</td>
      <td>7418</td>
      <td>Yash</td>
      <td>330000.0</td>
      <td>3900000.0</td>
      <td>4.2300</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Action</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.7360</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Crime</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.7360</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>16</th>
      <td>SuperFly</td>
      <td>Superfly</td>
      <td>2018</td>
      <td>116.0</td>
      <td>Thriller</td>
      <td>5.0</td>
      <td>4753</td>
      <td>Sony</td>
      <td>20500000.0</td>
      <td>236000.0</td>
      <td>20.7360</td>
      <td>Worldwide</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Oolong Courtyard: KungFu School</td>
      <td>Oolong Courtyard</td>
      <td>2018</td>
      <td>103.0</td>
      <td>Comedy</td>
      <td>4.6</td>
      <td>61</td>
      <td>CL</td>
      <td>37700.0</td>
      <td>0.0</td>
      <td>0.0377</td>
      <td>Domestic</td>
    </tr>
  </tbody>
</table>
<p>1514 rows × 12 columns</p>
</div>



Now let's order the genres by median total gross revenue. Because the data is quite skewed we will use median to better reflect the distribution of the data.


```python
unique_genres=df_genres['genres'].unique()
unique_genres
```




    array(['Action', 'Crime', 'Drama', 'Adventure', 'Fantasy', 'Comedy',
           'Music', 'History', 'Romance', 'Horror', 'Mystery', 'Biography',
           'Thriller', 'Sci-Fi', 'Family', 'Musical', 'Animation', 'Sport',
           'War', 'Documentary', 'Western'], dtype=object)



Here is an array of all the genres.


```python
genre_dict = {}
for genre in unique_genres:
    genre_dict[genre] = df_genres.loc[df_genres['genres'] == genre].median()['total_gross_MM']
genre_dict
```




    {'Action': 75.65,
     'Crime': 20.736,
     'Drama': 12.9,
     'Adventure': 189.35000000000002,
     'Fantasy': 123.89349999999999,
     'Comedy': 47.3,
     'Music': 12.95,
     'History': 14.45,
     'Romance': 8.4,
     'Horror': 43.0,
     'Mystery': 28.15,
     'Biography': 14.7,
     'Thriller': 29.25,
     'Sci-Fi': 314.54999999999995,
     'Family': 93.3,
     'Musical': 211.7,
     'Animation': 190.3,
     'Sport': 13.95,
     'War': 0.954,
     'Documentary': 0.6575,
     'Western': 29.8}



This is a dictionary with genres and the corresponding median total gross revenue.


```python
df_genre_order = pd.DataFrame.from_dict(genre_dict,orient='index')
df_genre_order
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Action</th>
      <td>75.6500</td>
    </tr>
    <tr>
      <th>Crime</th>
      <td>20.7360</td>
    </tr>
    <tr>
      <th>Drama</th>
      <td>12.9000</td>
    </tr>
    <tr>
      <th>Adventure</th>
      <td>189.3500</td>
    </tr>
    <tr>
      <th>Fantasy</th>
      <td>123.8935</td>
    </tr>
    <tr>
      <th>Comedy</th>
      <td>47.3000</td>
    </tr>
    <tr>
      <th>Music</th>
      <td>12.9500</td>
    </tr>
    <tr>
      <th>History</th>
      <td>14.4500</td>
    </tr>
    <tr>
      <th>Romance</th>
      <td>8.4000</td>
    </tr>
    <tr>
      <th>Horror</th>
      <td>43.0000</td>
    </tr>
    <tr>
      <th>Mystery</th>
      <td>28.1500</td>
    </tr>
    <tr>
      <th>Biography</th>
      <td>14.7000</td>
    </tr>
    <tr>
      <th>Thriller</th>
      <td>29.2500</td>
    </tr>
    <tr>
      <th>Sci-Fi</th>
      <td>314.5500</td>
    </tr>
    <tr>
      <th>Family</th>
      <td>93.3000</td>
    </tr>
    <tr>
      <th>Musical</th>
      <td>211.7000</td>
    </tr>
    <tr>
      <th>Animation</th>
      <td>190.3000</td>
    </tr>
    <tr>
      <th>Sport</th>
      <td>13.9500</td>
    </tr>
    <tr>
      <th>War</th>
      <td>0.9540</td>
    </tr>
    <tr>
      <th>Documentary</th>
      <td>0.6575</td>
    </tr>
    <tr>
      <th>Western</th>
      <td>29.8000</td>
    </tr>
  </tbody>
</table>
</div>



We made a dataframe with the genre dictionary and now we will order the genres by median total gross revenue.


```python
df_genre_order = df_genre_order[0].sort_values(ascending = False)
df_genre_order
```




    Sci-Fi         314.5500
    Musical        211.7000
    Animation      190.3000
    Adventure      189.3500
    Fantasy        123.8935
    Family          93.3000
    Action          75.6500
    Comedy          47.3000
    Horror          43.0000
    Western         29.8000
    Thriller        29.2500
    Mystery         28.1500
    Crime           20.7360
    Biography       14.7000
    History         14.4500
    Sport           13.9500
    Music           12.9500
    Drama           12.9000
    Romance          8.4000
    War              0.9540
    Documentary      0.6575
    Name: 0, dtype: float64



Let's create an index to sort the data.


```python
df_genre_order.index
```




    Index(['Sci-Fi', 'Musical', 'Animation', 'Adventure', 'Fantasy', 'Family',
           'Action', 'Comedy', 'Horror', 'Western', 'Thriller', 'Mystery', 'Crime',
           'Biography', 'History', 'Sport', 'Music', 'Drama', 'Romance', 'War',
           'Documentary'],
          dtype='object')



Finally we will plot the median gross revenue vs. genre.


```python
plt.figure(figsize=(15,20))
ax = sns.barplot(y=df_genres['genres'], x=df_genres['total_gross_MM'],
            estimator=np.median, order=df_genre_order.index ,
                 palette='dark')
ax.set(xlabel = 'Median Gross Revenue (MM USD)', ylabel = 'Genre',
      title = 'Median Gross Revenue vs. Genre');
```


    
![png](output_125_0.png)
    


The graph shows that movies in the Sci-Fi category have the greatest median gross revenue. This is followed by the Musical Genre, Animation, and Adventure genres.

Microsoft should consider making movies that fall under the following genres, in order: Sci-Fi, Musical, Animation, and Adventure.

### Effect of Rating on Total Revenue

Finally, let's compare IMDb rating with total gross revenue to see if creating highly rated movies will result in high revenue streams.


```python
ax = sns.lmplot(data=df_combined, x='averagerating',y='total_gross_MM',
                height = 10, palette='dark')
ax.set(ylim=(0,1400))
ax.set(xlabel = 'Average Rating', ylabel = 'Total Gross Revenue (MM USD)',
      title = 'Average Rating vs. Total Gross Revenue (MM USD)');
```


    
![png](output_129_0.png)
    


The graph shows there is a positive correlation between Average Rating and Total Gross Revenue.


Microsoft should consider making audience favorite movies in order to make more revenue.

## Conclusion

After analyzing data from IMDb and Box Office Mojo, we have the following recommendations for Microsoft in order to create the most Total Gross Revenue for the movies that they create:

* Release films worldwide, not just domestically.
* Make Sci-Fi, Musical, Animation, or Adventure movies.
* Make movies which audiences enjoy and rate highly.

## Future Work

This project is just a preliminary analysis and more work could be done to provide additional recommendations for Microsoft. The following is a list of additional work that could be done:

* This analysis based success on total gross revenue. Microsoft may consider other metrics of success such as profit.


* A comparison between existing movie studios could be completed to determine which studios are successful. Microsoft could consider emulating or partnering with the most successful studios.


* Success of movies could be compared with actors and directors to determine if Microsoft should hire specific people.


* An analysis of movie runtime could be performed to determine if it affects movie success.
