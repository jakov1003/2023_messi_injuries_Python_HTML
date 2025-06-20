Importing the necessary libraries


```python
import pandas as pd
pd.set_option('display.max_rows', 10)
```


```python
import requests as r
from bs4 import BeautifulSoup as soup
```

Getting Messi's injury data from Transfermarket using a scraper that I had built


```python
injury_data = []
headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36'}
for page_number in range (1,4):
    url = f"https://www.transfermarkt.com/lionel-messi/verletzungen/spieler/28003/ajax/yw1/sort/saison_id.desc/page/{page_number}"
    print(url)
    pageTree = r.get(url, headers=headers)
    pageSoup = soup(pageTree.content, 'html.parser')
    #injury_data = pageSoup.find_all("tr", {'class': 'odd'})
    header_1 = pageSoup.find_all("th", {'id': 'yw1_c0'})
    header_2 = pageSoup.find_all("th", {"id": "yw1_c1"})
    header_3 = pageSoup.find_all("th", {'id': 'yw1_c2'})
    header_4 = pageSoup.find_all("th", {'id': 'yw1_c3'})
    header_5 = pageSoup.find_all("th", {'id': 'yw1_c4'})
    header_6 = pageSoup.find_all("th", {'id': 'yw1_c5'})

    full_injury_data = header_1 + header_2 + header_3 + header_4 + header_5 + header_6 + injury_data
    
    
    injury_data_odd_row = pageSoup.find_all("tr", {'class': 'odd'})
    injury_data_even_row = pageSoup.find_all("tr", {'class': 'even'})

    for row in injury_data_odd_row + injury_data_even_row:
        season = row.find("td", {'class': 'zentriert'}).text.strip()
        injury = row.find("td", {'class': 'hauptlink'}).text.strip()
        start_date = row.find_all("td")[2].text.strip()
        end_date = row.find_all("td")[3].text.strip()
        duration = row.find_all("td")[4].text.strip()
        games_missed = row.find_all("td")[5].text.strip()

        print("season:", season)
        print("injury:", injury)
        print("start_date:", start_date)
        print("end_date:", end_date)
        print("duration:", duration)
        print("games_missed:", games_missed)
        print()
        
        injury_data.append({
            'season': season,
            'injury': injury,
            'start_date': start_date,
            'end_date': end_date,
            'duration_in_days': duration,
            'games_missed': games_missed,
        })

df = pd.DataFrame(injury_data)
df.to_csv('injury_data.csv', index=False)
display(df)
```

    https://www.transfermarkt.com/lionel-messi/verletzungen/spieler/28003/ajax/yw1/sort/saison_id.desc/page/1
    season: 22/23
    injury: Calf Problems
    start_date: Oct 5, 2022
    end_date: Oct 13, 2022
    duration: 8 days
    games_missed: 3
    
    season: 22/23
    injury: Rest
    start_date: Dec 26, 2022
    end_date: Jan 3, 2023
    duration: 8 days
    games_missed: 2
    
    season: 21/22
    injury: Fitness
    start_date: Aug 1, 2021
    end_date: Aug 26, 2021
    duration: 25 days
    games_missed: 4
    
    season: 21/22
    injury: Knee Problems
    start_date: Oct 29, 2021
    end_date: Nov 11, 2021
    duration: 13 days
    games_missed: 3
    
    season: 21/22
    injury: Quarantine
    start_date: Jan 6, 2022
    end_date: Jan 20, 2022
    duration: 14 days
    games_missed: 2
    
    season: 21/22
    injury: Flu
    start_date: Mar 17, 2022
    end_date: Mar 24, 2022
    duration: 7 days
    games_missed: 1
    
    season: 20/21
    injury: Hamstring Injury
    start_date: Jan 12, 2021
    end_date: Jan 15, 2021
    duration: 3 days
    games_missed: 1
    
    season: 19/20
    injury: Adductor problems
    start_date: Sep 25, 2019
    end_date: Oct 1, 2019
    duration: 6 days
    games_missed: 1
    
    season: 22/23
    injury: Achilles tendon problems
    start_date: Nov 5, 2022
    end_date: Nov 7, 2022
    duration: 2 days
    games_missed: 1
    
    season: 22/23
    injury: Knee Problems
    start_date: Feb 8, 2023
    end_date: Feb 13, 2023
    duration: 5 days
    games_missed: 2
    
    season: 21/22
    injury: Bone Bruise
    start_date: Sep 20, 2021
    end_date: Sep 27, 2021
    duration: 7 days
    games_missed: 2
    
    season: 21/22
    injury: Corona virus
    start_date: Jan 1, 2022
    end_date: Jan 5, 2022
    duration: 4 days
    games_missed: 1
    
    season: 21/22
    injury: Quarantine
    start_date: Jan 6, 2022
    end_date: Jan 20, 2022
    duration: 14 days
    games_missed: 2
    
    season: 20/21
    injury: Ankle Injury
    start_date: Dec 23, 2020
    end_date: Jan 1, 2021
    duration: 9 days
    games_missed: 1
    
    season: 19/20
    injury: Foot Injury
    start_date: Aug 5, 2019
    end_date: Sep 16, 2019
    duration: 42 days
    games_missed: 6
    
    https://www.transfermarkt.com/lionel-messi/verletzungen/spieler/28003/ajax/yw1/sort/saison_id.desc/page/2
    season: 18/19
    injury: Forearm Fracture
    start_date: Oct 21, 2018
    end_date: Nov 9, 2018
    duration: 19 days
    games_missed: 5
    
    season: 18/19
    injury: Forearm Fracture
    start_date: Oct 21, 2018
    end_date: Nov 9, 2018
    duration: 19 days
    games_missed: 5
    
    season: 17/18
    injury: Adductor problems
    start_date: Mar 22, 2018
    end_date: Mar 30, 2018
    duration: 8 days
    games_missed: 2
    
    season: 16/17
    injury: Torn muscle bundle
    start_date: Sep 22, 2016
    end_date: Oct 15, 2016
    duration: 23 days
    games_missed: 6
    
    season: 15/16
    injury: Medial Collateral Ligament Knee Injury
    start_date: Sep 28, 2015
    end_date: Nov 18, 2015
    duration: 51 days
    games_missed: 13
    
    season: 15/16
    injury: Thigh Problems
    start_date: Jan 18, 2016
    end_date: Jan 21, 2016
    duration: 3 days
    games_missed: 1
    
    season: 15/16
    injury: Back bruise
    start_date: May 28, 2016
    end_date: Jun 10, 2016
    duration: 13 days
    games_missed: 1
    
    season: 13/14
    injury: Thigh Problems
    start_date: Aug 11, 2013
    end_date: Aug 15, 2013
    duration: 4 days
    games_missed: 1
    
    season: 18/19
    injury: Forearm Fracture
    start_date: Oct 21, 2018
    end_date: Nov 9, 2018
    duration: 19 days
    games_missed: 5
    
    season: 18/19
    injury: Pubitis
    start_date: Mar 23, 2019
    end_date: Mar 29, 2019
    duration: 6 days
    games_missed: 1
    
    season: 16/17
    injury: Adductor problems
    start_date: Sep 5, 2016
    end_date: Sep 8, 2016
    duration: 3 days
    games_missed: 1
    
    season: 16/17
    injury: Influenza
    start_date: Nov 17, 2016
    end_date: Nov 21, 2016
    duration: 4 days
    games_missed: 1
    
    season: 15/16
    injury: Kidney problems
    start_date: Dec 14, 2015
    end_date: Dec 17, 2015
    duration: 3 days
    games_missed: 1
    
    season: 15/16
    injury: Kidney stone surgery
    start_date: Feb 9, 2016
    end_date: Feb 14, 2016
    duration: 5 days
    games_missed: 2
    
    season: 14/15
    injury: Ankle problems
    start_date: Mar 29, 2015
    end_date: Mar 31, 2015
    duration: 2 days
    games_missed: -
    
    https://www.transfermarkt.com/lionel-messi/verletzungen/spieler/28003/ajax/yw1/sort/saison_id.desc/page/3
    season: 13/14
    injury: Thigh Problems
    start_date: Aug 22, 2013
    end_date: Aug 26, 2013
    duration: 4 days
    games_missed: 1
    
    season: 13/14
    injury: Torn muscle bundle
    start_date: Nov 11, 2013
    end_date: Jan 6, 2014
    duration: 56 days
    games_missed: 11
    
    season: 12/13
    injury: Hamstring Injury
    start_date: May 13, 2013
    end_date: Jun 6, 2013
    duration: 24 days
    games_missed: 3
    
    season: 09/10
    injury: Strain
    start_date: Aug 10, 2009
    end_date: Aug 18, 2009
    duration: 8 days
    games_missed: 2
    
    season: 07/08
    injury: Torn muscle bundle
    start_date: Mar 6, 2008
    end_date: Apr 10, 2008
    duration: 35 days
    games_missed: 9
    
    season: 05/06
    injury: Thigh Muscle Strain
    start_date: Feb 6, 2006
    end_date: Feb 16, 2006
    duration: 10 days
    games_missed: 1
    
    season: 13/14
    injury: Hamstring Injury
    start_date: Sep 29, 2013
    end_date: Oct 18, 2013
    duration: 19 days
    games_missed: 4
    
    season: 12/13
    injury: Hamstring Injury
    start_date: Apr 3, 2013
    end_date: Apr 22, 2013
    duration: 19 days
    games_missed: 4
    
    season: 10/11
    injury: Stretched Ligament
    start_date: Sep 20, 2010
    end_date: Sep 27, 2010
    duration: 7 days
    games_missed: 2
    
    season: 07/08
    injury: Hamstring Injury
    start_date: Dec 17, 2007
    end_date: Jan 14, 2008
    duration: 28 days
    games_missed: 6
    
    season: 06/07
    injury: Metatarsal Fracture
    start_date: Nov 13, 2006
    end_date: Feb 8, 2007
    duration: 87 days
    games_missed: 19
    
    season: 05/06
    injury: Torn muscle bundle
    start_date: Mar 8, 2006
    end_date: May 21, 2006
    duration: 74 days
    games_missed: 17
    
    


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
      <th>season</th>
      <th>injury</th>
      <th>start_date</th>
      <th>end_date</th>
      <th>duration_in_days</th>
      <th>games_missed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22/23</td>
      <td>Calf Problems</td>
      <td>Oct 5, 2022</td>
      <td>Oct 13, 2022</td>
      <td>8 days</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22/23</td>
      <td>Rest</td>
      <td>Dec 26, 2022</td>
      <td>Jan 3, 2023</td>
      <td>8 days</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21/22</td>
      <td>Fitness</td>
      <td>Aug 1, 2021</td>
      <td>Aug 26, 2021</td>
      <td>25 days</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21/22</td>
      <td>Knee Problems</td>
      <td>Oct 29, 2021</td>
      <td>Nov 11, 2021</td>
      <td>13 days</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21/22</td>
      <td>Quarantine</td>
      <td>Jan 6, 2022</td>
      <td>Jan 20, 2022</td>
      <td>14 days</td>
      <td>2</td>
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
      <th>37</th>
      <td>12/13</td>
      <td>Hamstring Injury</td>
      <td>Apr 3, 2013</td>
      <td>Apr 22, 2013</td>
      <td>19 days</td>
      <td>4</td>
    </tr>
    <tr>
      <th>38</th>
      <td>10/11</td>
      <td>Stretched Ligament</td>
      <td>Sep 20, 2010</td>
      <td>Sep 27, 2010</td>
      <td>7 days</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>07/08</td>
      <td>Hamstring Injury</td>
      <td>Dec 17, 2007</td>
      <td>Jan 14, 2008</td>
      <td>28 days</td>
      <td>6</td>
    </tr>
    <tr>
      <th>40</th>
      <td>06/07</td>
      <td>Metatarsal Fracture</td>
      <td>Nov 13, 2006</td>
      <td>Feb 8, 2007</td>
      <td>87 days</td>
      <td>19</td>
    </tr>
    <tr>
      <th>41</th>
      <td>05/06</td>
      <td>Torn muscle bundle</td>
      <td>Mar 8, 2006</td>
      <td>May 21, 2006</td>
      <td>74 days</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>42 rows × 6 columns</p>
</div>


Checking the data types of columns with the intention of converting two columns into the integer type


```python
df.dtypes
```




    season              object
    injury              object
    start_date          object
    end_date            object
    duration_in_days    object
    games_missed        object
    dtype: object



Attempting to convert two columns to the integer type for the purposes of visualization. An error occurred due to the scraper writing the '-' character instead of 0 in the 'games_missed' column. This relates to Messi's injuries which did not cause him to miss a single match.   


```python
df['games_missed'] = df['games_missed'].astype(int)
df['duration_in_days'] = df['duration_in_days'].astype(int)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    Cell In[5], line 1
    ----> 1 df['games_missed'] = df['games_missed'].astype(int)
          2 df['duration_in_days'] = df['duration_in_days'].astype(int)
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\generic.py:6240, in NDFrame.astype(self, dtype, copy, errors)
       6233     results = [
       6234         self.iloc[:, i].astype(dtype, copy=copy)
       6235         for i in range(len(self.columns))
       6236     ]
       6238 else:
       6239     # else, only a single dtype is given
    -> 6240     new_data = self._mgr.astype(dtype=dtype, copy=copy, errors=errors)
       6241     return self._constructor(new_data).__finalize__(self, method="astype")
       6243 # GH 33113: handle empty frame or series
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\internals\managers.py:448, in BaseBlockManager.astype(self, dtype, copy, errors)
        447 def astype(self: T, dtype, copy: bool = False, errors: str = "raise") -> T:
    --> 448     return self.apply("astype", dtype=dtype, copy=copy, errors=errors)
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\internals\managers.py:352, in BaseBlockManager.apply(self, f, align_keys, ignore_failures, **kwargs)
        350         applied = b.apply(f, **kwargs)
        351     else:
    --> 352         applied = getattr(b, f)(**kwargs)
        353 except (TypeError, NotImplementedError):
        354     if not ignore_failures:
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\internals\blocks.py:526, in Block.astype(self, dtype, copy, errors)
        508 """
        509 Coerce to the new dtype.
        510 
       (...)
        522 Block
        523 """
        524 values = self.values
    --> 526 new_values = astype_array_safe(values, dtype, copy=copy, errors=errors)
        528 new_values = maybe_coerce_values(new_values)
        529 newb = self.make_block(new_values)
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\dtypes\astype.py:299, in astype_array_safe(values, dtype, copy, errors)
        296     return values.copy()
        298 try:
    --> 299     new_values = astype_array(values, dtype, copy=copy)
        300 except (ValueError, TypeError):
        301     # e.g. astype_nansafe can fail on object-dtype of strings
        302     #  trying to convert to float
        303     if errors == "ignore":
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\dtypes\astype.py:230, in astype_array(values, dtype, copy)
        227     values = values.astype(dtype, copy=copy)
        229 else:
    --> 230     values = astype_nansafe(values, dtype, copy=copy)
        232 # in pandas we don't store numpy str dtypes, so convert to object
        233 if isinstance(dtype, np.dtype) and issubclass(values.dtype.type, str):
    

    File C:\ProgramData\anaconda3\Lib\site-packages\pandas\core\dtypes\astype.py:170, in astype_nansafe(arr, dtype, copy, skipna)
        166     raise ValueError(msg)
        168 if copy or is_object_dtype(arr.dtype) or is_object_dtype(dtype):
        169     # Explicit copy, or required since NumPy can't view from / to object.
    --> 170     return arr.astype(dtype, copy=True)
        172 return arr.astype(dtype, copy=copy)
    

    ValueError: invalid literal for int() with base 10: '-'


Replacing the '-' character with 0 in the 'games_missed' column, and the string 'days' with an empty string in the 'duration_in_days' column allowed to convert the data in both of those columns to the integer type.


```python
df = df.replace('-', 0)
```


```python
df['games_missed'] = df['games_missed'].astype(int)
display(df)
df.dtypes
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
      <th>season</th>
      <th>injury</th>
      <th>start_date</th>
      <th>end_date</th>
      <th>duration_in_days</th>
      <th>games_missed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22/23</td>
      <td>Calf Problems</td>
      <td>Oct 5, 2022</td>
      <td>Oct 13, 2022</td>
      <td>8 days</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22/23</td>
      <td>Rest</td>
      <td>Dec 26, 2022</td>
      <td>Jan 3, 2023</td>
      <td>8 days</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21/22</td>
      <td>Fitness</td>
      <td>Aug 1, 2021</td>
      <td>Aug 26, 2021</td>
      <td>25 days</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21/22</td>
      <td>Knee Problems</td>
      <td>Oct 29, 2021</td>
      <td>Nov 11, 2021</td>
      <td>13 days</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21/22</td>
      <td>Quarantine</td>
      <td>Jan 6, 2022</td>
      <td>Jan 20, 2022</td>
      <td>14 days</td>
      <td>2</td>
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
      <th>37</th>
      <td>12/13</td>
      <td>Hamstring Injury</td>
      <td>Apr 3, 2013</td>
      <td>Apr 22, 2013</td>
      <td>19 days</td>
      <td>4</td>
    </tr>
    <tr>
      <th>38</th>
      <td>10/11</td>
      <td>Stretched Ligament</td>
      <td>Sep 20, 2010</td>
      <td>Sep 27, 2010</td>
      <td>7 days</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>07/08</td>
      <td>Hamstring Injury</td>
      <td>Dec 17, 2007</td>
      <td>Jan 14, 2008</td>
      <td>28 days</td>
      <td>6</td>
    </tr>
    <tr>
      <th>40</th>
      <td>06/07</td>
      <td>Metatarsal Fracture</td>
      <td>Nov 13, 2006</td>
      <td>Feb 8, 2007</td>
      <td>87 days</td>
      <td>19</td>
    </tr>
    <tr>
      <th>41</th>
      <td>05/06</td>
      <td>Torn muscle bundle</td>
      <td>Mar 8, 2006</td>
      <td>May 21, 2006</td>
      <td>74 days</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>42 rows × 6 columns</p>
</div>





    season              object
    injury              object
    start_date          object
    end_date            object
    duration_in_days    object
    games_missed         int32
    dtype: object




```python
df['duration_in_days'] = df['duration_in_days'].str.replace('days', '')
df['duration_in_days'] = df['duration_in_days'].astype(int)
display(df)
df.dtypes
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
      <th>season</th>
      <th>injury</th>
      <th>start_date</th>
      <th>end_date</th>
      <th>duration_in_days</th>
      <th>games_missed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22/23</td>
      <td>Calf Problems</td>
      <td>Oct 5, 2022</td>
      <td>Oct 13, 2022</td>
      <td>8</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22/23</td>
      <td>Rest</td>
      <td>Dec 26, 2022</td>
      <td>Jan 3, 2023</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21/22</td>
      <td>Fitness</td>
      <td>Aug 1, 2021</td>
      <td>Aug 26, 2021</td>
      <td>25</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21/22</td>
      <td>Knee Problems</td>
      <td>Oct 29, 2021</td>
      <td>Nov 11, 2021</td>
      <td>13</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21/22</td>
      <td>Quarantine</td>
      <td>Jan 6, 2022</td>
      <td>Jan 20, 2022</td>
      <td>14</td>
      <td>2</td>
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
      <th>37</th>
      <td>12/13</td>
      <td>Hamstring Injury</td>
      <td>Apr 3, 2013</td>
      <td>Apr 22, 2013</td>
      <td>19</td>
      <td>4</td>
    </tr>
    <tr>
      <th>38</th>
      <td>10/11</td>
      <td>Stretched Ligament</td>
      <td>Sep 20, 2010</td>
      <td>Sep 27, 2010</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>07/08</td>
      <td>Hamstring Injury</td>
      <td>Dec 17, 2007</td>
      <td>Jan 14, 2008</td>
      <td>28</td>
      <td>6</td>
    </tr>
    <tr>
      <th>40</th>
      <td>06/07</td>
      <td>Metatarsal Fracture</td>
      <td>Nov 13, 2006</td>
      <td>Feb 8, 2007</td>
      <td>87</td>
      <td>19</td>
    </tr>
    <tr>
      <th>41</th>
      <td>05/06</td>
      <td>Torn muscle bundle</td>
      <td>Mar 8, 2006</td>
      <td>May 21, 2006</td>
      <td>74</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>42 rows × 6 columns</p>
</div>





    season              object
    injury              object
    start_date          object
    end_date            object
    duration_in_days     int32
    games_missed         int32
    dtype: object



Saving the CSV file and then uploading the data into my local PostgreSQL database. 

I created the connection to my local database with the engine object and then built the table with the .to_sql function.

The number 42 as the output refers to the the number of rows in the CSV file, meaning the upload was successful.


```python
df.to_csv("messi_injuries")
```


```python
from sqlalchemy import create_engine
import pandas as pd
df4 = pd.read_csv("messi_injuries", index_col=0)
host = 'localhost'
database = 'postgres'
user = 'postgres'
password = '1001'
port = 5432

engine = create_engine(f'postgresql://{user}:{password}@{host}:{port}/{database}')
df4.to_sql('messi_injuries', engine,if_exists='replace')
```




    42



I decided not to treat a COVID-related quarantine as an injury even though I had not excluded that row from the upload to the database. 

This was a mistake, as Messi himself has spoken multiple times during 2022 about physical issues stemming from that bout of COVID. 

While working on this project in 2023, I wrongly concluded that "Quarantine" meant isolation instead of the person in question being ill.


```python
import pandas as pd
df2 = pd.read_csv("messi_injuries", index_col = 0)
df2 = df2[df2.injury != 'Quarantine']
display(df2)
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
      <th>season</th>
      <th>injury</th>
      <th>start_date</th>
      <th>end_date</th>
      <th>duration_in_days</th>
      <th>games_missed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22/23</td>
      <td>Calf Problems</td>
      <td>Oct 5, 2022</td>
      <td>Oct 13, 2022</td>
      <td>8</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22/23</td>
      <td>Rest</td>
      <td>Dec 26, 2022</td>
      <td>Jan 3, 2023</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21/22</td>
      <td>Fitness</td>
      <td>Aug 1, 2021</td>
      <td>Aug 26, 2021</td>
      <td>25</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21/22</td>
      <td>Knee Problems</td>
      <td>Oct 29, 2021</td>
      <td>Nov 11, 2021</td>
      <td>13</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21/22</td>
      <td>Flu</td>
      <td>Mar 17, 2022</td>
      <td>Mar 24, 2022</td>
      <td>7</td>
      <td>1</td>
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
      <th>37</th>
      <td>12/13</td>
      <td>Hamstring Injury</td>
      <td>Apr 3, 2013</td>
      <td>Apr 22, 2013</td>
      <td>19</td>
      <td>4</td>
    </tr>
    <tr>
      <th>38</th>
      <td>10/11</td>
      <td>Stretched Ligament</td>
      <td>Sep 20, 2010</td>
      <td>Sep 27, 2010</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>07/08</td>
      <td>Hamstring Injury</td>
      <td>Dec 17, 2007</td>
      <td>Jan 14, 2008</td>
      <td>28</td>
      <td>6</td>
    </tr>
    <tr>
      <th>40</th>
      <td>06/07</td>
      <td>Metatarsal Fracture</td>
      <td>Nov 13, 2006</td>
      <td>Feb 8, 2007</td>
      <td>87</td>
      <td>19</td>
    </tr>
    <tr>
      <th>41</th>
      <td>05/06</td>
      <td>Torn muscle bundle</td>
      <td>Mar 8, 2006</td>
      <td>May 21, 2006</td>
      <td>74</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>40 rows × 6 columns</p>
</div>


Dropping the unnecessary columns


```python
df2 = df2.drop(['injury', 'start_date', 'end_date'], axis=1)
```


```python
display(df2)
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
      <th>season</th>
      <th>duration_in_days</th>
      <th>games_missed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22/23</td>
      <td>8</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22/23</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21/22</td>
      <td>25</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21/22</td>
      <td>13</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21/22</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>37</th>
      <td>12/13</td>
      <td>19</td>
      <td>4</td>
    </tr>
    <tr>
      <th>38</th>
      <td>10/11</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>07/08</td>
      <td>28</td>
      <td>6</td>
    </tr>
    <tr>
      <th>40</th>
      <td>06/07</td>
      <td>87</td>
      <td>19</td>
    </tr>
    <tr>
      <th>41</th>
      <td>05/06</td>
      <td>74</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>40 rows × 3 columns</p>
</div>


In the cell below I grouped the games missed and the duration in days data by season.


```python
df3 = df2.groupby('season')[['games_missed', 'duration_in_days']].sum()
df3 = df3.reset_index()
display(df3)
df3.dtypes
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
      <th>season</th>
      <th>games_missed</th>
      <th>duration_in_days</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>05/06</td>
      <td>18</td>
      <td>84</td>
    </tr>
    <tr>
      <th>1</th>
      <td>06/07</td>
      <td>19</td>
      <td>87</td>
    </tr>
    <tr>
      <th>2</th>
      <td>07/08</td>
      <td>15</td>
      <td>63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>09/10</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10/11</td>
      <td>2</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>18/19</td>
      <td>16</td>
      <td>63</td>
    </tr>
    <tr>
      <th>12</th>
      <td>19/20</td>
      <td>7</td>
      <td>48</td>
    </tr>
    <tr>
      <th>13</th>
      <td>20/21</td>
      <td>2</td>
      <td>12</td>
    </tr>
    <tr>
      <th>14</th>
      <td>21/22</td>
      <td>11</td>
      <td>56</td>
    </tr>
    <tr>
      <th>15</th>
      <td>22/23</td>
      <td>8</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
<p>16 rows × 3 columns</p>
</div>





    season              object
    games_missed         int64
    duration_in_days     int64
    dtype: object



However, I was intersted in how Messi's age correlated to the duration of his injuries in days and the number of matches he missed because of injuries.

In the cell below I replaced the season data with age data.

Since I had already grouped the duration and the games missed data by season, there was no need to group (by age) again.


```python
import pandas as pd


updated_values = [
    [18, 18, 84],
    [19, 18, 87],
    [20, 14, 63],
    [21, 0, 0],
    [22, 1, 8],
    [23, 2, 7],
    [24, 0, 0],
    [25, 7, 43],
    [26, 12, 83],
    [27, 0, 2],
    [28, 13, 75],
    [29, 5, 30],
    [30, 0, 8],
    [31, 15, 63],
    [32, 5, 48],
    [33, 2, 12],
    [34, 11, 56],
    [35, 8, 23]
]

updated_df3 = pd.DataFrame(updated_values, columns=['age', 'games_missed', 'duration_in_days'])


df3[['season', 'games_missed', 'duration_in_days']] = updated_df3[['age', 'games_missed', 'duration_in_days']]
display(updated_df3)
updated_df3.to_csv("messi_injuries_by_age")
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
      <th>age</th>
      <th>games_missed</th>
      <th>duration_in_days</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18</td>
      <td>18</td>
      <td>84</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19</td>
      <td>18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>14</td>
      <td>63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>1</td>
      <td>8</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>31</td>
      <td>15</td>
      <td>63</td>
    </tr>
    <tr>
      <th>14</th>
      <td>32</td>
      <td>5</td>
      <td>48</td>
    </tr>
    <tr>
      <th>15</th>
      <td>33</td>
      <td>2</td>
      <td>12</td>
    </tr>
    <tr>
      <th>16</th>
      <td>34</td>
      <td>11</td>
      <td>56</td>
    </tr>
    <tr>
      <th>17</th>
      <td>35</td>
      <td>8</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
<p>18 rows × 3 columns</p>
</div>



```python
updated_df3.dtypes
```




    age                 int64
    games_missed        int64
    duration_in_days    int64
    dtype: object



In cell below this markdown you can see a graph showing the relationship between Messi's age and the number of matches he missed due to injury.

I also made another graph further below showcasing the relationship between the duration of Messi's injuries in days and, again,
his age. I wrote the insights below that second graph.

The graphs look very similar, but they are not the same. However, the same conclusion can be derived from both graphs. 

If I were doing this project now (2025), I would have only made the duration in days graph due to that factor being more relevant in this case. 

Why I do I think the duration in days is more important than the number of matches missed? 

Well, my intention was to assess how injuries affected Messi over the course of his career up to 2023 rather than 
whether they were detrimental to his record-breaking contribution to Barcelona and the spell at PSG (he had not transferred to Inter Miami yet).


```python
import matplotlib.pyplot as plt
updated_df3.plot(x="age", y=["games_missed"],
        kind="line", figsize=(7, 7))
x_ticks = range(17, 36, 1)
plt.xticks(x_ticks)
y_ticks = range(0, 19, 2)
plt.yticks(y_ticks)

plt.gca().get_legend().remove()
plt.title("Messi's injuries: Games missed vs. Age")
plt.xlabel("Age")
plt.ylabel("Games Missed")
plt.show()
```


    
![png](C:\Users\Jakov\Downloads\messi_1.png)
    


## Second graph and insights


```python
import matplotlib.pyplot as plt
updated_df3.plot(x="age", y=["duration_in_days"],
        kind="line", figsize=(7, 7))
x_ticks = range(17, 36, 1)
plt.xticks(x_ticks)
y_ticks = range(0, 90, 10)
plt.yticks(y_ticks)
plt.gca().get_legend().remove()
plt.title("Messi's injuries: Duration vs. Age")
plt.xlabel("Age")
plt.ylabel("Duration in Days")
plt.show()
```


    
![png](messi_injuries_files/messi_injuries_29_0.png)
    


The insight text below this sentence was written by me at the time of making of this project in 2023. I haven't edited it. 

We can notice a pattern in Messi injury record. Look at the lowpoints of his injuries in three periods: before the age of 21, between the ages of 21 and 29 and after and including the age of 30. The extent varies, but before the age of 21 and after the age of 29 the lengths of his shortest injuries in those two periods of his career are bigger than the lengths of his shortest injuries between the ages of 21 and 29.


```python

```
