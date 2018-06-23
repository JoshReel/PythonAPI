

```python
from citipy import citipy
from random import uniform
import pandas as pd
import numpy as np
import csv
import random
import matplotlib.pyplot as plt
import requests as req
import json
import seaborn as sns
from datetime import datetime as dt
```


```python
def newpoint():
    return uniform(-90,90), uniform(-180,180)

points = []
points = (newpoint() for x in range(1500))
columns = ("Lat", "Lng")
City_df = pd.DataFrame([x for x in points], columns=columns)
City_df.head()
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
      <th>Lat</th>
      <th>Lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>77.456829</td>
      <td>-68.188523</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-19.665146</td>
      <td>-107.725133</td>
    </tr>
    <tr>
      <th>2</th>
      <td>89.213659</td>
      <td>-171.563856</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-43.236750</td>
      <td>25.891401</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-29.904506</td>
      <td>20.977323</td>
    </tr>
  </tbody>
</table>
</div>




```python
City_df["City"] = ""
City_df["Country"] = ""
for index, row in City_df.iterrows():
    city = citipy.nearest_city(row['Lat'], row['Lng']).city_name
    country = citipy.nearest_city(row['Lat'], row['Lng']).country_code
    City_df.set_value(index, "City", city)
    City_df.set_value(index, "Country", country)
City_Sample = City_df.drop_duplicates(subset='City').sample(n=500).reset_index()
    
City_Sample["Temp"] = ""
City_Sample["Humidity"] = ""
City_Sample["Date"] = ""
City_Sample["Wind Speed"] = ""
City_Sample["Cloudiness"] = ""
del City_Sample['index']
City_Sample.head()
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:6: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      
    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:7: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      import sys





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
      <th>Lat</th>
      <th>Lng</th>
      <th>City</th>
      <th>Country</th>
      <th>Temp</th>
      <th>Humidity</th>
      <th>Date</th>
      <th>Wind Speed</th>
      <th>Cloudiness</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>59.607673</td>
      <td>166.879813</td>
      <td>tilichiki</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>-51.860881</td>
      <td>24.354770</td>
      <td>plettenberg bay</td>
      <td>za</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.702120</td>
      <td>164.685830</td>
      <td>bairiki</td>
      <td>ki</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>18.387177</td>
      <td>-158.146512</td>
      <td>lahaina</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>26.669271</td>
      <td>-172.322868</td>
      <td>kapaa</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>16.162721</td>
      <td>102.492384</td>
      <td>kaeng khlo</td>
      <td>th</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>21.006766</td>
      <td>12.039408</td>
      <td>bilma</td>
      <td>ne</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>-17.521659</td>
      <td>144.403012</td>
      <td>atherton</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>8</th>
      <td>-24.562795</td>
      <td>-95.751486</td>
      <td>pisco</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>12.045573</td>
      <td>-62.864491</td>
      <td>gouyave</td>
      <td>gd</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>10</th>
      <td>19.986954</td>
      <td>-113.524921</td>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>11</th>
      <td>15.071757</td>
      <td>55.438684</td>
      <td>salalah</td>
      <td>om</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>12</th>
      <td>76.150489</td>
      <td>26.161215</td>
      <td>honningsvag</td>
      <td>no</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>13</th>
      <td>-34.421028</td>
      <td>2.236768</td>
      <td>luderitz</td>
      <td>na</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>14</th>
      <td>-15.525823</td>
      <td>30.076941</td>
      <td>luangwa</td>
      <td>zm</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>15</th>
      <td>-19.509456</td>
      <td>-54.783823</td>
      <td>rio verde de mato grosso</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>16</th>
      <td>-18.618330</td>
      <td>137.378143</td>
      <td>mount isa</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>17</th>
      <td>56.859289</td>
      <td>49.505957</td>
      <td>mari-turek</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>18</th>
      <td>58.213499</td>
      <td>86.075946</td>
      <td>belyy yar</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>19</th>
      <td>44.147174</td>
      <td>5.205956</td>
      <td>carpentras</td>
      <td>fr</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>20</th>
      <td>-16.527514</td>
      <td>-49.173350</td>
      <td>neropolis</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>21</th>
      <td>61.312183</td>
      <td>-124.093709</td>
      <td>fort nelson</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>22</th>
      <td>67.910520</td>
      <td>53.320441</td>
      <td>iskateley</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>23</th>
      <td>64.243111</td>
      <td>155.788851</td>
      <td>omsukchan</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>24</th>
      <td>22.158504</td>
      <td>69.980673</td>
      <td>lalpur</td>
      <td>in</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>25</th>
      <td>-17.037948</td>
      <td>13.204072</td>
      <td>opuwo</td>
      <td>na</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>26</th>
      <td>54.362516</td>
      <td>-43.917004</td>
      <td>nanortalik</td>
      <td>gl</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>27</th>
      <td>-0.200787</td>
      <td>117.942263</td>
      <td>bontang</td>
      <td>id</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>28</th>
      <td>-13.355073</td>
      <td>169.376884</td>
      <td>sola</td>
      <td>vu</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>29</th>
      <td>-19.665146</td>
      <td>-107.725133</td>
      <td>puerto ayora</td>
      <td>ec</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
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
    </tr>
    <tr>
      <th>470</th>
      <td>61.836136</td>
      <td>3.128045</td>
      <td>floro</td>
      <td>no</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>471</th>
      <td>80.245520</td>
      <td>-97.219766</td>
      <td>thompson</td>
      <td>ca</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>472</th>
      <td>-13.635032</td>
      <td>36.385616</td>
      <td>cuamba</td>
      <td>mz</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>473</th>
      <td>0.184566</td>
      <td>100.348410</td>
      <td>payakumbuh</td>
      <td>id</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>474</th>
      <td>77.124431</td>
      <td>119.691665</td>
      <td>saskylakh</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>475</th>
      <td>-12.938573</td>
      <td>-74.787637</td>
      <td>lircay</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>476</th>
      <td>27.079757</td>
      <td>-99.366683</td>
      <td>guerrero</td>
      <td>mx</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>477</th>
      <td>-3.399010</td>
      <td>12.950096</td>
      <td>sibiti</td>
      <td>cg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>478</th>
      <td>16.296753</td>
      <td>-71.604064</td>
      <td>pedernales</td>
      <td>do</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>479</th>
      <td>22.739629</td>
      <td>25.944669</td>
      <td>tahta</td>
      <td>eg</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>480</th>
      <td>-8.079941</td>
      <td>-84.180743</td>
      <td>sechura</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>481</th>
      <td>3.016253</td>
      <td>-52.318994</td>
      <td>camopi</td>
      <td>gf</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>482</th>
      <td>-41.032352</td>
      <td>-81.863357</td>
      <td>ancud</td>
      <td>cl</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>483</th>
      <td>-32.303561</td>
      <td>-60.523137</td>
      <td>victoria</td>
      <td>ar</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>484</th>
      <td>-3.361551</td>
      <td>31.931978</td>
      <td>ushirombo</td>
      <td>tz</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>485</th>
      <td>-11.154479</td>
      <td>-55.945266</td>
      <td>alta floresta</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>486</th>
      <td>80.113716</td>
      <td>32.053255</td>
      <td>berlevag</td>
      <td>no</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>487</th>
      <td>-47.643233</td>
      <td>-5.140155</td>
      <td>cape town</td>
      <td>za</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>488</th>
      <td>55.072354</td>
      <td>99.366309</td>
      <td>atagay</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>489</th>
      <td>-11.659484</td>
      <td>-72.954450</td>
      <td>pangoa</td>
      <td>pe</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>490</th>
      <td>39.453366</td>
      <td>84.868785</td>
      <td>korla</td>
      <td>cn</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>491</th>
      <td>60.165968</td>
      <td>67.605348</td>
      <td>kondinskoye</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>492</th>
      <td>10.828025</td>
      <td>-38.932580</td>
      <td>acarau</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>493</th>
      <td>-84.244914</td>
      <td>10.511296</td>
      <td>hermanus</td>
      <td>za</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>494</th>
      <td>56.264443</td>
      <td>75.690592</td>
      <td>muromtsevo</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>495</th>
      <td>65.843236</td>
      <td>117.801754</td>
      <td>nyurba</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>496</th>
      <td>-16.790279</td>
      <td>-33.337429</td>
      <td>belmonte</td>
      <td>br</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>497</th>
      <td>64.740604</td>
      <td>143.478033</td>
      <td>ust-nera</td>
      <td>ru</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>498</th>
      <td>-40.918892</td>
      <td>168.848989</td>
      <td>hokitika</td>
      <td>nz</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>499</th>
      <td>33.978197</td>
      <td>96.545258</td>
      <td>xining</td>
      <td>cn</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>500 rows × 9 columns</p>
</div>




```python
api_key = "c5714c323dda46f5e44b1bfdb0fe341c"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "Imperial"
```


```python
counter = 0
for index, row in City_Sample.iterrows():
    try:
        target_url = "http://api.openweathermap.org/data/2.5/weather?units=%s&APPID=%s&q=%s" % (units,api_key, row['City'])
        cities_data = req.get(target_url).json()
        City_Sample.set_value(index, "Temp", cities_data["main"]["temp_max"])
        City_Sample.set_value(index, "Humidity", cities_data["main"]["humidity"])
        City_Sample.set_value(index, "Date", cities_data["dt"])
        City_Sample.set_value(index, "Wind Speed", cities_data["wind"]["speed"])
        City_Sample.set_value(index, "Cloudiness", cities_data["clouds"]["all"])
        counter = counter + 1
    except (AttributeError, KeyError) as e:
        pass

    print("------------------------")
    print("Proceesing Record : " , counter)
    print(target_url)                  
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:6: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      
    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:7: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      import sys
    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:8: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      
    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:9: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      if __name__ == '__main__':
    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:10: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      # Remove the CWD from sys.path while we load stuff.


    ------------------------
    Proceesing Record :  1
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tilichiki
    ------------------------
    Proceesing Record :  2
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=plettenberg bay
    ------------------------
    Proceesing Record :  2
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bairiki
    ------------------------
    Proceesing Record :  3
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lahaina
    ------------------------
    Proceesing Record :  4
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kapaa
    ------------------------
    Proceesing Record :  4
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kaeng khlo
    ------------------------
    Proceesing Record :  5
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bilma
    ------------------------
    Proceesing Record :  6
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=atherton
    ------------------------
    Proceesing Record :  7
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pisco
    ------------------------
    Proceesing Record :  8
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gouyave
    ------------------------
    Proceesing Record :  9
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cabo san lucas
    ------------------------
    Proceesing Record :  10
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=salalah
    ------------------------
    Proceesing Record :  11
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=honningsvag
    ------------------------
    Proceesing Record :  12
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=luderitz
    ------------------------
    Proceesing Record :  13
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=luangwa
    ------------------------
    Proceesing Record :  14
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=rio verde de mato grosso
    ------------------------
    Proceesing Record :  15
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mount isa
    ------------------------
    Proceesing Record :  16
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mari-turek
    ------------------------
    Proceesing Record :  17
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=belyy yar
    ------------------------
    Proceesing Record :  18
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=carpentras
    ------------------------
    Proceesing Record :  19
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=neropolis
    ------------------------
    Proceesing Record :  20
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=fort nelson
    ------------------------
    Proceesing Record :  21
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=iskateley
    ------------------------
    Proceesing Record :  22
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=omsukchan
    ------------------------
    Proceesing Record :  23
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lalpur
    ------------------------
    Proceesing Record :  24
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=opuwo
    ------------------------
    Proceesing Record :  25
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nanortalik
    ------------------------
    Proceesing Record :  26
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bontang
    ------------------------
    Proceesing Record :  27
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sola
    ------------------------
    Proceesing Record :  28
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=puerto ayora
    ------------------------
    Proceesing Record :  29
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nelson bay
    ------------------------
    Proceesing Record :  30
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ugoofaaru
    ------------------------
    Proceesing Record :  31
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=thinadhoo
    ------------------------
    Proceesing Record :  32
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=qostanay
    ------------------------
    Proceesing Record :  32
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tabialan
    ------------------------
    Proceesing Record :  33
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cukai
    ------------------------
    Proceesing Record :  34
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=windsor
    ------------------------
    Proceesing Record :  35
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=las vegas
    ------------------------
    Proceesing Record :  36
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bonthe
    ------------------------
    Proceesing Record :  37
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dunedin
    ------------------------
    Proceesing Record :  38
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kenai
    ------------------------
    Proceesing Record :  39
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mnogovershinnyy
    ------------------------
    Proceesing Record :  40
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chokurdakh
    ------------------------
    Proceesing Record :  41
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tiksi
    ------------------------
    Proceesing Record :  41
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dolbeau
    ------------------------
    Proceesing Record :  42
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dayong
    ------------------------
    Proceesing Record :  43
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=maxixe
    ------------------------
    Proceesing Record :  44
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=valparaiso
    ------------------------
    Proceesing Record :  45
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=abu samrah
    ------------------------
    Proceesing Record :  46
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=santa elena
    ------------------------
    Proceesing Record :  47
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ormara
    ------------------------
    Proceesing Record :  48
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sambava
    ------------------------
    Proceesing Record :  49
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=norfolk
    ------------------------
    Proceesing Record :  50
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kosjeric
    ------------------------
    Proceesing Record :  51
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kedrovyy
    ------------------------
    Proceesing Record :  52
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=san quintin
    ------------------------
    Proceesing Record :  53
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hofn
    ------------------------
    Proceesing Record :  53
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tsihombe
    ------------------------
    Proceesing Record :  54
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ahipara
    ------------------------
    Proceesing Record :  55
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=rocha
    ------------------------
    Proceesing Record :  56
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=manaus
    ------------------------
    Proceesing Record :  57
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mar del plata
    ------------------------
    Proceesing Record :  58
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=junction city
    ------------------------
    Proceesing Record :  59
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ishigaki
    ------------------------
    Proceesing Record :  60
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=upernavik
    ------------------------
    Proceesing Record :  61
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port elizabeth
    ------------------------
    Proceesing Record :  62
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mugla
    ------------------------
    Proceesing Record :  63
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=namatanai
    ------------------------
    Proceesing Record :  64
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=djambala
    ------------------------
    Proceesing Record :  65
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=okha
    ------------------------
    Proceesing Record :  66
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kavaratti
    ------------------------
    Proceesing Record :  67
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=santa fe
    ------------------------
    Proceesing Record :  68
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=buala
    ------------------------
    Proceesing Record :  69
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hamilton
    ------------------------
    Proceesing Record :  70
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yagodnoye
    ------------------------
    Proceesing Record :  71
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=fairbanks
    ------------------------
    Proceesing Record :  72
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tuktoyaktuk
    ------------------------
    Proceesing Record :  73
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pevek
    ------------------------
    Proceesing Record :  74
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=shimoda
    ------------------------
    Proceesing Record :  75
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vredendal
    ------------------------
    Proceesing Record :  76
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=morgan city
    ------------------------
    Proceesing Record :  77
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yurga
    ------------------------
    Proceesing Record :  78
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=basco
    ------------------------
    Proceesing Record :  79
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=acapulco
    ------------------------
    Proceesing Record :  80
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kahului
    ------------------------
    Proceesing Record :  81
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=podporozhye
    ------------------------
    Proceesing Record :  81
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sentyabrskiy
    ------------------------
    Proceesing Record :  82
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vila velha
    ------------------------
    Proceesing Record :  83
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ixtapa
    ------------------------
    Proceesing Record :  84
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mahebourg
    ------------------------
    Proceesing Record :  85
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=georgetown
    ------------------------
    Proceesing Record :  86
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=zhigansk
    ------------------------
    Proceesing Record :  87
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kailua
    ------------------------
    Proceesing Record :  88
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dwarka
    ------------------------
    Proceesing Record :  89
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nha trang
    ------------------------
    Proceesing Record :  90
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=butembo
    ------------------------
    Proceesing Record :  91
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=puerto madero
    ------------------------
    Proceesing Record :  92
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ruteng
    ------------------------
    Proceesing Record :  93
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kupang
    ------------------------
    Proceesing Record :  94
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lovington
    ------------------------
    Proceesing Record :  95
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=beringovskiy
    ------------------------
    Proceesing Record :  96
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kuybyshevo
    ------------------------
    Proceesing Record :  97
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=folldal
    ------------------------
    Proceesing Record :  98
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=comodoro rivadavia
    ------------------------
    Proceesing Record :  99
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=potosi
    ------------------------
    Proceesing Record :  100
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=waipawa
    ------------------------
    Proceesing Record :  101
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=inyonga
    ------------------------
    Proceesing Record :  102
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kirkwood
    ------------------------
    Proceesing Record :  103
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lagoa
    ------------------------
    Proceesing Record :  104
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=koygorodok
    ------------------------
    Proceesing Record :  105
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=temiscaming
    ------------------------
    Proceesing Record :  106
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=narsaq
    ------------------------
    Proceesing Record :  107
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mocuba
    ------------------------
    Proceesing Record :  107
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=wulanhaote
    ------------------------
    Proceesing Record :  108
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=milkovo
    ------------------------
    Proceesing Record :  108
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=eldikan
    ------------------------
    Proceesing Record :  109
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=necochea
    ------------------------
    Proceesing Record :  110
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=coracora
    ------------------------
    Proceesing Record :  111
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port macquarie
    ------------------------
    Proceesing Record :  112
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=byron bay
    ------------------------
    Proceesing Record :  113
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nioro
    ------------------------
    Proceesing Record :  114
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sistranda
    ------------------------
    Proceesing Record :  115
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=new norfolk
    ------------------------
    Proceesing Record :  116
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nikolskoye
    ------------------------
    Proceesing Record :  117
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=calama
    ------------------------
    Proceesing Record :  118
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=roald
    ------------------------
    Proceesing Record :  119
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vardo
    ------------------------
    Proceesing Record :  120
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=srednekolymsk
    ------------------------
    Proceesing Record :  121
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=coihaique
    ------------------------
    Proceesing Record :  121
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=shchelyayur
    ------------------------
    Proceesing Record :  122
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bowen
    ------------------------
    Proceesing Record :  123
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=galdar
    ------------------------
    Proceesing Record :  124
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chengde
    ------------------------
    Proceesing Record :  125
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yellowknife
    ------------------------
    Proceesing Record :  126
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mackay
    ------------------------
    Proceesing Record :  127
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=santa luzia
    ------------------------
    Proceesing Record :  128
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=shenzhen
    ------------------------
    Proceesing Record :  129
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alice springs
    ------------------------
    Proceesing Record :  130
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=samfya
    ------------------------
    Proceesing Record :  131
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kindu
    ------------------------
    Proceesing Record :  132
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port alfred
    ------------------------
    Proceesing Record :  133
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=carnarvon
    ------------------------
    Proceesing Record :  134
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=igarka
    ------------------------
    Proceesing Record :  135
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=payson
    ------------------------
    Proceesing Record :  136
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=shirokiy
    ------------------------
    Proceesing Record :  137
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=rauma
    ------------------------
    Proceesing Record :  138
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lahan
    ------------------------
    Proceesing Record :  139
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=flin flon
    ------------------------
    Proceesing Record :  140
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=abeche
    ------------------------
    Proceesing Record :  141
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=san cristobal
    ------------------------
    Proceesing Record :  142
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=turayf
    ------------------------
    Proceesing Record :  143
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=wuchang
    ------------------------
    Proceesing Record :  144
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kandrian
    ------------------------
    Proceesing Record :  145
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=porto novo
    ------------------------
    Proceesing Record :  146
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=torbay
    ------------------------
    Proceesing Record :  147
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=garissa
    ------------------------
    Proceesing Record :  148
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=deputatskiy
    ------------------------
    Proceesing Record :  149
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=perugia
    ------------------------
    Proceesing Record :  150
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gazojak
    ------------------------
    Proceesing Record :  151
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ossora
    ------------------------
    Proceesing Record :  152
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=avarua
    ------------------------
    Proceesing Record :  153
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=santa maria do para
    ------------------------
    Proceesing Record :  154
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ambilobe
    ------------------------
    Proceesing Record :  155
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=zwedru
    ------------------------
    Proceesing Record :  156
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=magadan
    ------------------------
    Proceesing Record :  157
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=neiafu
    ------------------------
    Proceesing Record :  157
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bur gabo
    ------------------------
    Proceesing Record :  158
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alyangula
    ------------------------
    Proceesing Record :  159
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dicabisagan
    ------------------------
    Proceesing Record :  159
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kismayo
    ------------------------
    Proceesing Record :  159
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kota bahru
    ------------------------
    Proceesing Record :  160
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=iranshahr
    ------------------------
    Proceesing Record :  161
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=flinders
    ------------------------
    Proceesing Record :  162
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kouango
    ------------------------
    Proceesing Record :  163
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gillette
    ------------------------
    Proceesing Record :  164
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sao gabriel da cachoeira
    ------------------------
    Proceesing Record :  165
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hilo
    ------------------------
    Proceesing Record :  166
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=uribia
    ------------------------
    Proceesing Record :  167
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=paramirim
    ------------------------
    Proceesing Record :  168
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=batagay-alyta
    ------------------------
    Proceesing Record :  169
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=constitucion
    ------------------------
    Proceesing Record :  170
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chiang mai
    ------------------------
    Proceesing Record :  171
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kaitangata
    ------------------------
    Proceesing Record :  172
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cordoba
    ------------------------
    Proceesing Record :  173
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hirara
    ------------------------
    Proceesing Record :  174
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=oga
    ------------------------
    Proceesing Record :  175
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bredasdorp
    ------------------------
    Proceesing Record :  176
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sitka
    ------------------------
    Proceesing Record :  177
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=atambua
    ------------------------
    Proceesing Record :  178
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=santiago del estero
    ------------------------
    Proceesing Record :  179
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dengzhou
    ------------------------
    Proceesing Record :  180
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cayenne
    ------------------------
    Proceesing Record :  181
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=marsh harbour
    ------------------------
    Proceesing Record :  182
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=beisfjord
    ------------------------
    Proceesing Record :  183
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=babanka
    ------------------------
    Proceesing Record :  184
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tautira
    ------------------------
    Proceesing Record :  185
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=portland
    ------------------------
    Proceesing Record :  186
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bluff
    ------------------------
    Proceesing Record :  187
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=leningradskiy
    ------------------------
    Proceesing Record :  188
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=taltal
    ------------------------
    Proceesing Record :  189
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=marienburg
    ------------------------
    Proceesing Record :  190
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nouadhibou
    ------------------------
    Proceesing Record :  191
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lai
    ------------------------
    Proceesing Record :  192
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=katsuura
    ------------------------
    Proceesing Record :  193
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mount gambier
    ------------------------
    Proceesing Record :  194
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gurlan
    ------------------------
    Proceesing Record :  195
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yenagoa
    ------------------------
    Proceesing Record :  196
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=salima
    ------------------------
    Proceesing Record :  197
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=knysna
    ------------------------
    Proceesing Record :  198
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=meulaboh
    ------------------------
    Proceesing Record :  199
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=govindgarh
    ------------------------
    Proceesing Record :  200
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cap-aux-meules
    ------------------------
    Proceesing Record :  201
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lorengau
    ------------------------
    Proceesing Record :  202
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=san carlos de bariloche
    ------------------------
    Proceesing Record :  203
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=upington
    ------------------------
    Proceesing Record :  204
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kologriv
    ------------------------
    Proceesing Record :  204
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=varzea alegre
    ------------------------
    Proceesing Record :  204
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=louisbourg
    ------------------------
    Proceesing Record :  205
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=eyl
    ------------------------
    Proceesing Record :  206
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kruisfontein
    ------------------------
    Proceesing Record :  207
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=astoria
    ------------------------
    Proceesing Record :  208
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=san luis
    ------------------------
    Proceesing Record :  208
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cam pha
    ------------------------
    Proceesing Record :  209
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=havre-saint-pierre
    ------------------------
    Proceesing Record :  210
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cotusca
    ------------------------
    Proceesing Record :  211
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ussel
    ------------------------
    Proceesing Record :  211
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nizhneyansk
    ------------------------
    Proceesing Record :  211
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=milingimbi
    ------------------------
    Proceesing Record :  212
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hervey bay
    ------------------------
    Proceesing Record :  213
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tasiilaq
    ------------------------
    Proceesing Record :  214
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=juneau
    ------------------------
    Proceesing Record :  215
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=burkburnett
    ------------------------
    Proceesing Record :  216
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=camacha
    ------------------------
    Proceesing Record :  217
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=anage
    ------------------------
    Proceesing Record :  218
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=betulia
    ------------------------
    Proceesing Record :  219
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=komsomolskiy
    ------------------------
    Proceesing Record :  220
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=snyder
    ------------------------
    Proceesing Record :  221
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nyrob
    ------------------------
    Proceesing Record :  221
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ruatoria
    ------------------------
    Proceesing Record :  222
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port lincoln
    ------------------------
    Proceesing Record :  223
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sorsk
    ------------------------
    Proceesing Record :  224
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=shaki
    ------------------------
    Proceesing Record :  225
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kendari
    ------------------------
    Proceesing Record :  226
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vila franca do campo
    ------------------------
    Proceesing Record :  227
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=faanui
    ------------------------
    Proceesing Record :  228
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=huangmei
    ------------------------
    Proceesing Record :  229
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vaini
    ------------------------
    Proceesing Record :  230
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=calvinia
    ------------------------
    Proceesing Record :  231
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=rawson
    ------------------------
    Proceesing Record :  232
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tuatapere
    ------------------------
    Proceesing Record :  232
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=barentsburg
    ------------------------
    Proceesing Record :  233
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lasa
    ------------------------
    Proceesing Record :  234
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=haines junction
    ------------------------
    Proceesing Record :  235
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hay river
    ------------------------
    Proceesing Record :  236
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hobart
    ------------------------
    Proceesing Record :  237
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tigil
    ------------------------
    Proceesing Record :  238
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bloomsburg
    ------------------------
    Proceesing Record :  239
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mawlaik
    ------------------------
    Proceesing Record :  240
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bundaberg
    ------------------------
    Proceesing Record :  241
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bandarbeyla
    ------------------------
    Proceesing Record :  241
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chagda
    ------------------------
    Proceesing Record :  242
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=rio formoso
    ------------------------
    Proceesing Record :  243
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sao filipe
    ------------------------
    Proceesing Record :  244
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tupi paulista
    ------------------------
    Proceesing Record :  245
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=malindi
    ------------------------
    Proceesing Record :  246
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=geraldton
    ------------------------
    Proceesing Record :  247
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cherskiy
    ------------------------
    Proceesing Record :  248
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ambo
    ------------------------
    Proceesing Record :  249
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bud
    ------------------------
    Proceesing Record :  249
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=grand river south east
    ------------------------
    Proceesing Record :  250
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tongzi
    ------------------------
    Proceesing Record :  251
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=turukhansk
    ------------------------
    Proceesing Record :  252
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=san patricio
    ------------------------
    Proceesing Record :  253
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saint george
    ------------------------
    Proceesing Record :  254
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ambon
    ------------------------
    Proceesing Record :  255
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=naze
    ------------------------
    Proceesing Record :  256
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mmabatho
    ------------------------
    Proceesing Record :  257
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pereleshino
    ------------------------
    Proceesing Record :  258
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=qaanaaq
    ------------------------
    Proceesing Record :  259
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=the valley
    ------------------------
    Proceesing Record :  260
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=richards bay
    ------------------------
    Proceesing Record :  261
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mazagao
    ------------------------
    Proceesing Record :  262
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=clyde river
    ------------------------
    Proceesing Record :  263
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=verkhoyansk
    ------------------------
    Proceesing Record :  264
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=maraba
    ------------------------
    Proceesing Record :  265
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=caravelas
    ------------------------
    Proceesing Record :  266
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=soria
    ------------------------
    Proceesing Record :  266
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=stoyba
    ------------------------
    Proceesing Record :  267
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=capreol
    ------------------------
    Proceesing Record :  268
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ulaanbaatar
    ------------------------
    Proceesing Record :  269
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=istok
    ------------------------
    Proceesing Record :  270
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tezu
    ------------------------
    Proceesing Record :  270
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=parras
    ------------------------
    Proceesing Record :  271
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port augusta
    ------------------------
    Proceesing Record :  272
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=martapura
    ------------------------
    Proceesing Record :  273
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=omachi
    ------------------------
    Proceesing Record :  274
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=maldonado
    ------------------------
    Proceesing Record :  275
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=esperance
    ------------------------
    Proceesing Record :  276
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mataura
    ------------------------
    Proceesing Record :  277
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bathsheba
    ------------------------
    Proceesing Record :  278
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kalmunai
    ------------------------
    Proceesing Record :  279
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=punta arenas
    ------------------------
    Proceesing Record :  280
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=krasnogorsk
    ------------------------
    Proceesing Record :  281
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=te anau
    ------------------------
    Proceesing Record :  282
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=milove
    ------------------------
    Proceesing Record :  283
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bambous virieux
    ------------------------
    Proceesing Record :  284
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=winnemucca
    ------------------------
    Proceesing Record :  285
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=coahuayana
    ------------------------
    Proceesing Record :  286
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kloulklubed
    ------------------------
    Proceesing Record :  287
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sultanpur
    ------------------------
    Proceesing Record :  288
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mosquera
    ------------------------
    Proceesing Record :  289
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=arraial do cabo
    ------------------------
    Proceesing Record :  290
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=igrim
    ------------------------
    Proceesing Record :  291
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=krasnovishersk
    ------------------------
    Proceesing Record :  292
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bozhou
    ------------------------
    Proceesing Record :  293
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=maumere
    ------------------------
    Proceesing Record :  294
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dharchula
    ------------------------
    Proceesing Record :  295
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hammerfest
    ------------------------
    Proceesing Record :  296
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=egvekinot
    ------------------------
    Proceesing Record :  297
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saint-philippe
    ------------------------
    Proceesing Record :  298
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hollola
    ------------------------
    Proceesing Record :  299
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=leuwiliang
    ------------------------
    Proceesing Record :  300
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dubbo
    ------------------------
    Proceesing Record :  301
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alofi
    ------------------------
    Proceesing Record :  302
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=jiehu
    ------------------------
    Proceesing Record :  303
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=burnie
    ------------------------
    Proceesing Record :  304
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gat
    ------------------------
    Proceesing Record :  305
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sur
    ------------------------
    Proceesing Record :  306
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=khatanga
    ------------------------
    Proceesing Record :  307
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=touros
    ------------------------
    Proceesing Record :  308
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hasaki
    ------------------------
    Proceesing Record :  309
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=muravlenko
    ------------------------
    Proceesing Record :  310
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=spresiano
    ------------------------
    Proceesing Record :  311
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mokhsogollokh
    ------------------------
    Proceesing Record :  312
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sawtell
    ------------------------
    Proceesing Record :  313
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saldanha
    ------------------------
    Proceesing Record :  314
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sharan
    ------------------------
    Proceesing Record :  315
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=laguna
    ------------------------
    Proceesing Record :  316
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=letterkenny
    ------------------------
    Proceesing Record :  317
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alexandria
    ------------------------
    Proceesing Record :  318
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=auki
    ------------------------
    Proceesing Record :  319
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sarankhola
    ------------------------
    Proceesing Record :  320
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tokur
    ------------------------
    Proceesing Record :  321
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=arkhangelsk
    ------------------------
    Proceesing Record :  322
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=awbari
    ------------------------
    Proceesing Record :  323
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=avera
    ------------------------
    Proceesing Record :  324
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nicolas bravo
    ------------------------
    Proceesing Record :  325
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=weligama
    ------------------------
    Proceesing Record :  326
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chicama
    ------------------------
    Proceesing Record :  327
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ribeira grande
    ------------------------
    Proceesing Record :  327
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=matsanga
    ------------------------
    Proceesing Record :  328
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=emerald
    ------------------------
    Proceesing Record :  328
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tabiauea
    ------------------------
    Proceesing Record :  329
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=banff
    ------------------------
    Proceesing Record :  330
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=albany
    ------------------------
    Proceesing Record :  331
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gizo
    ------------------------
    Proceesing Record :  332
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pandrup
    ------------------------
    Proceesing Record :  333
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mhlambanyatsi
    ------------------------
    Proceesing Record :  333
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ha
    ------------------------
    Proceesing Record :  334
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bafq
    ------------------------
    Proceesing Record :  335
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=vao
    ------------------------
    Proceesing Record :  336
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=puerto asis
    ------------------------
    Proceesing Record :  337
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=beira
    ------------------------
    Proceesing Record :  338
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cache creek
    ------------------------
    Proceesing Record :  339
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=broome
    ------------------------
    Proceesing Record :  339
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ust-kamchatsk
    ------------------------
    Proceesing Record :  340
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saint-ambroise
    ------------------------
    Proceesing Record :  340
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pemangkat
    ------------------------
    Proceesing Record :  341
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=luanda
    ------------------------
    Proceesing Record :  342
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hambantota
    ------------------------
    Proceesing Record :  343
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lebu
    ------------------------
    Proceesing Record :  344
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sorland
    ------------------------
    Proceesing Record :  345
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=severo-kurilsk
    ------------------------
    Proceesing Record :  346
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mecca
    ------------------------
    Proceesing Record :  347
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lokosovo
    ------------------------
    Proceesing Record :  347
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=palabuhanratu
    ------------------------
    Proceesing Record :  348
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=whitianga
    ------------------------
    Proceesing Record :  349
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=barcelos
    ------------------------
    Proceesing Record :  350
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=koumra
    ------------------------
    Proceesing Record :  350
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lolua
    ------------------------
    Proceesing Record :  350
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=toliary
    ------------------------
    Proceesing Record :  351
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=wagar
    ------------------------
    Proceesing Record :  352
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=atuona
    ------------------------
    Proceesing Record :  353
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=peachland
    ------------------------
    Proceesing Record :  353
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=chokwe
    ------------------------
    Proceesing Record :  354
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=altamont
    ------------------------
    Proceesing Record :  355
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=boromlya
    ------------------------
    Proceesing Record :  356
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=berdigestyakh
    ------------------------
    Proceesing Record :  357
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hermiston
    ------------------------
    Proceesing Record :  358
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=butaritari
    ------------------------
    Proceesing Record :  358
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bengkulu
    ------------------------
    Proceesing Record :  359
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pajapan
    ------------------------
    Proceesing Record :  360
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alugan
    ------------------------
    Proceesing Record :  361
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=iqaluit
    ------------------------
    Proceesing Record :  362
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=aklavik
    ------------------------
    Proceesing Record :  363
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=barrow
    ------------------------
    Proceesing Record :  364
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=poum
    ------------------------
    Proceesing Record :  365
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bulawayo
    ------------------------
    Proceesing Record :  366
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mubi
    ------------------------
    Proceesing Record :  367
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yar-sale
    ------------------------
    Proceesing Record :  367
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=asilah
    ------------------------
    Proceesing Record :  367
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=jahrom
    ------------------------
    Proceesing Record :  368
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=savannah bight
    ------------------------
    Proceesing Record :  369
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=castro
    ------------------------
    Proceesing Record :  370
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=anadyr
    ------------------------
    Proceesing Record :  370
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=amancio
    ------------------------
    Proceesing Record :  370
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=samusu
    ------------------------
    Proceesing Record :  371
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=puerto escondido
    ------------------------
    Proceesing Record :  372
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pauini
    ------------------------
    Proceesing Record :  372
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=amderma
    ------------------------
    Proceesing Record :  373
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=seabra
    ------------------------
    Proceesing Record :  374
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port hardy
    ------------------------
    Proceesing Record :  375
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=belvedere marittimo
    ------------------------
    Proceesing Record :  376
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=keetmanshoop
    ------------------------
    Proceesing Record :  377
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=eureka
    ------------------------
    Proceesing Record :  377
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=asau
    ------------------------
    Proceesing Record :  378
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=albanel
    ------------------------
    Proceesing Record :  379
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mehamn
    ------------------------
    Proceesing Record :  380
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=half moon bay
    ------------------------
    Proceesing Record :  381
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nicoya
    ------------------------
    Proceesing Record :  382
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=grafton
    ------------------------
    Proceesing Record :  383
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=oswego
    ------------------------
    Proceesing Record :  383
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=umzimvubu
    ------------------------
    Proceesing Record :  384
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mayo
    ------------------------
    Proceesing Record :  385
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ilulissat
    ------------------------
    Proceesing Record :  386
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=maceio
    ------------------------
    Proceesing Record :  387
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=port-gentil
    ------------------------
    Proceesing Record :  388
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=emmett
    ------------------------
    Proceesing Record :  389
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=darhan
    ------------------------
    Proceesing Record :  390
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=jerusalem
    ------------------------
    Proceesing Record :  391
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=aksarka
    ------------------------
    Proceesing Record :  392
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mujiayingzi
    ------------------------
    Proceesing Record :  393
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pacific grove
    ------------------------
    Proceesing Record :  394
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sunrise manor
    ------------------------
    Proceesing Record :  395
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=oussouye
    ------------------------
    Proceesing Record :  395
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=asfi
    ------------------------
    Proceesing Record :  395
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=linchuan
    ------------------------
    Proceesing Record :  396
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sept-iles
    ------------------------
    Proceesing Record :  397
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=bethel
    ------------------------
    Proceesing Record :  398
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=provideniya
    ------------------------
    Proceesing Record :  399
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kodiak
    ------------------------
    Proceesing Record :  400
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=krasnyy chikoy
    ------------------------
    Proceesing Record :  401
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sao felix do xingu
    ------------------------
    Proceesing Record :  402
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=codrington
    ------------------------
    Proceesing Record :  403
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lavrentiya
    ------------------------
    Proceesing Record :  404
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=gidole
    ------------------------
    Proceesing Record :  405
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=warrnambool
    ------------------------
    Proceesing Record :  406
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=menongue
    ------------------------
    Proceesing Record :  407
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pangnirtung
    ------------------------
    Proceesing Record :  408
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cidreira
    ------------------------
    Proceesing Record :  409
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=mandan
    ------------------------
    Proceesing Record :  410
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=namibe
    ------------------------
    Proceesing Record :  410
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saleaula
    ------------------------
    Proceesing Record :  411
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=udachnyy
    ------------------------
    Proceesing Record :  412
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=atbasar
    ------------------------
    Proceesing Record :  413
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nemuro
    ------------------------
    Proceesing Record :  414
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=busselton
    ------------------------
    Proceesing Record :  415
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saint-augustin
    ------------------------
    Proceesing Record :  416
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=dhangadhi
    ------------------------
    Proceesing Record :  417
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=amboasary
    ------------------------
    Proceesing Record :  418
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=basoko
    ------------------------
    Proceesing Record :  419
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=road town
    ------------------------
    Proceesing Record :  420
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=yulara
    ------------------------
    Proceesing Record :  421
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=svetlogorsk
    ------------------------
    Proceesing Record :  422
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=fortuna
    ------------------------
    Proceesing Record :  423
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=praia da vitoria
    ------------------------
    Proceesing Record :  423
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kazalinsk
    ------------------------
    Proceesing Record :  424
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=grindavik
    ------------------------
    Proceesing Record :  425
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sainte-suzanne
    ------------------------
    Proceesing Record :  426
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tomatlan
    ------------------------
    Proceesing Record :  427
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=floro
    ------------------------
    Proceesing Record :  428
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=thompson
    ------------------------
    Proceesing Record :  429
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cuamba
    ------------------------
    Proceesing Record :  430
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=payakumbuh
    ------------------------
    Proceesing Record :  431
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=saskylakh
    ------------------------
    Proceesing Record :  432
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=lircay
    ------------------------
    Proceesing Record :  433
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=guerrero
    ------------------------
    Proceesing Record :  434
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sibiti
    ------------------------
    Proceesing Record :  435
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pedernales
    ------------------------
    Proceesing Record :  435
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=tahta
    ------------------------
    Proceesing Record :  436
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=sechura
    ------------------------
    Proceesing Record :  437
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=camopi
    ------------------------
    Proceesing Record :  438
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ancud
    ------------------------
    Proceesing Record :  439
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=victoria
    ------------------------
    Proceesing Record :  440
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ushirombo
    ------------------------
    Proceesing Record :  441
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=alta floresta
    ------------------------
    Proceesing Record :  442
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=berlevag
    ------------------------
    Proceesing Record :  443
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=cape town
    ------------------------
    Proceesing Record :  444
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=atagay
    ------------------------
    Proceesing Record :  445
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=pangoa
    ------------------------
    Proceesing Record :  445
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=korla
    ------------------------
    Proceesing Record :  446
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=kondinskoye
    ------------------------
    Proceesing Record :  446
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=acarau
    ------------------------
    Proceesing Record :  447
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hermanus
    ------------------------
    Proceesing Record :  448
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=muromtsevo
    ------------------------
    Proceesing Record :  449
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=nyurba
    ------------------------
    Proceesing Record :  450
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=belmonte
    ------------------------
    Proceesing Record :  451
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=ust-nera
    ------------------------
    Proceesing Record :  452
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=hokitika
    ------------------------
    Proceesing Record :  453
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=c5714c323dda46f5e44b1bfdb0fe341c&q=xining



```python
City_Sample = City_Sample.replace('', np.NaN)
City_Sample.head()
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
      <th>Lat</th>
      <th>Lng</th>
      <th>City</th>
      <th>Country</th>
      <th>Temp</th>
      <th>Humidity</th>
      <th>Date</th>
      <th>Wind Speed</th>
      <th>Cloudiness</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>59.607673</td>
      <td>166.879813</td>
      <td>tilichiki</td>
      <td>ru</td>
      <td>50.70</td>
      <td>86.0</td>
      <td>1.529790e+09</td>
      <td>3.87</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-51.860881</td>
      <td>24.354770</td>
      <td>plettenberg bay</td>
      <td>za</td>
      <td>61.59</td>
      <td>96.0</td>
      <td>1.529790e+09</td>
      <td>27.13</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.702120</td>
      <td>164.685830</td>
      <td>bairiki</td>
      <td>ki</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18.387177</td>
      <td>-158.146512</td>
      <td>lahaina</td>
      <td>us</td>
      <td>84.20</td>
      <td>51.0</td>
      <td>1.529787e+09</td>
      <td>13.87</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>26.669271</td>
      <td>-172.322868</td>
      <td>kapaa</td>
      <td>us</td>
      <td>84.20</td>
      <td>66.0</td>
      <td>1.529787e+09</td>
      <td>18.34</td>
      <td>40.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Build a scatter plot for lat / temp
plt.scatter(City_Sample["Lat"], City_Sample["Temp"], marker="o", color = 'red')

plt.title("Altitude vs Temperature")
plt.ylabel("Temperature (F)")
plt.xlabel("Latitude")
plt.yticks(np.arange(-40, 120, 20))
plt.grid(True)
sns.set_style('darkgrid')

plt.savefig("TempvsLat.png")
plt.show()
```


![png](output_6_0.png)



```python
# Build a scatter plot for lat / humidity
plt.scatter(City_Sample["Lat"], City_Sample["Humidity"], marker="o", color = 'green')

plt.title("Altitude vs Humidity")
plt.ylabel("Humidity(%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.yticks(np.arange(-20, 120, 20))
sns.set_style('darkgrid')

plt.savefig("HumidvsLat.png")
plt.show()

```


![png](output_7_0.png)



```python
# Build a scatter plot for lat / cloudiness
plt.scatter(City_Sample["Lat"], City_Sample["Cloudiness"], marker="o", color = 'pink')

plt.title("Altitude vs Cloudiness")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.yticks(np.arange(-20, 120, 20))
sns.set_style('darkgrid')

plt.savefig("CloudvsLat.png")
plt.show()
```


![png](output_8_0.png)



```python
# Build a scatter plot for lat / wind speed
plt.scatter(City_Sample["Lat"], City_Sample["Wind Speed"], marker="o", color = 'blue')

plt.title("Altitude vs Wind Speed")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)

sns.set_style('darkgrid')

plt.savefig("WindvsLat.png")
plt.show()
```


![png](output_9_0.png)


3 Observable Trends:
1. Cities closest to the equator tend to be warmer.
2. Humidity typically ranges between 65 to 100.
3. Wind speed typically ranges between 2 to 7 mph.

