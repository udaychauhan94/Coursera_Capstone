
# **Delhi Population Control**
##### ***Data based suggestions, to Indian Government by Uday Pratap Singh, to control Delhi population growth.***


___

#### **A) Why it is important to Depopulate Delhi, or at least control the population growth?**

Let's start by importing libraries.


```python
import numpy as np # library to handle data in a vectorized manner

import pandas as pd # library for data analsysis
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

import json # library to handle JSON files

#!conda install -c conda-forge geopy --yes # uncomment this line if you haven't completed the Foursquare API lab
from geopy.geocoders import Nominatim # convert an address into latitude and longitude values

import requests # library to handle requests
from pandas.io.json import json_normalize # tranform JSON file into a pandas dataframe

# Matplotlib and associated plotting modules
import matplotlib.cm as cm
import matplotlib.colors as colors

# import k-means from clustering stage
from sklearn.cluster import KMeans

#!conda install -c conda-forge folium=0.5.0 --yes # uncomment this line if you haven't completed the Foursquare API lab
import folium # map rendering library


import xml

!conda install -c conda-forge folium=0.5.0 --yes # uncomment this line if you haven't completed the Foursquare API lab
import folium # map rendering library

# for webscraping import Beautiful Soup 
from bs4 import BeautifulSoup

#for plot
import matplotlib as mpl
import matplotlib.pyplot as plt

print('Libraries imported.')
```

    WARNING conda.base.context:use_only_tar_bz2(632): Conda is constrained to only using the old .tar.bz2 file format because you have conda-build installed, and it is <3.18.3.  Update or remove conda-build to get smaller downloads and faster extractions.
    Collecting package metadata (repodata.json): done
    Solving environment: | 
    The environment is inconsistent, please check the package plan carefully
    The following packages are causing the inconsistency:
    
      - defaults/linux-64::anaconda==5.3.1=py37_0
      - defaults/linux-64::astropy==3.0.4=py37h14c3975_0
      - defaults/linux-64::bkcharts==0.2=py37_0
      - defaults/linux-64::blaze==0.11.3=py37_0
      - defaults/linux-64::bokeh==0.13.0=py37_0
      - defaults/linux-64::bottleneck==1.2.1=py37h035aef0_1
      - defaults/linux-64::dask==0.19.1=py37_0
      - defaults/linux-64::datashape==0.5.4=py37_1
      - defaults/linux-64::mkl-service==1.1.2=py37h90e4bf4_5
      - defaults/linux-64::numba==0.39.0=py37h04863e7_0
      - defaults/linux-64::numexpr==2.6.8=py37hd89afb7_0
      - defaults/linux-64::odo==0.5.1=py37_0
      - defaults/linux-64::pytables==3.4.4=py37ha205bf6_0
      - defaults/linux-64::pytest-arraydiff==0.2=py37h39e3cac_0
      - defaults/linux-64::pytest-astropy==0.4.0=py37_0
      - defaults/linux-64::pytest-doctestplus==0.1.3=py37_0
      - defaults/linux-64::pywavelets==1.0.0=py37hdd07704_0
      - defaults/linux-64::scikit-image==0.14.0=py37hf484d3e_1
    failed
    
    UnsatisfiableError: The following specifications were found to be incompatible with each other:
    
      - anaconda/linux-64::anaconda-navigator==1.9.7=py36_0
      - anaconda/linux-64::graphviz==2.40.1=h21bd128_2 -> pango[version='>=1.42.1,<2.0a0']
      - anaconda/linux-64::importlib_metadata==0.8=py36_0
      - anaconda/linux-64::lxml==4.3.0=py36hefd8a0e_0
      - anaconda/linux-64::mkl_fft==1.0.6=py36h7dd41cf_0 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::mkl_random==1.0.1=py36h4414c95_1 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::navigator-updater==0.2.1=py36_0
      - anaconda/linux-64::numpy-base==1.15.4=py36h81de0dd_0 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::numpy==1.15.4=py36h1d66e8a_0 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::pytorch==0.4.1=py36ha74772b_0 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::scikit-learn==0.20.1=py36h4989274_0 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::scipy==1.1.0=py36hfa4b5c9_1 -> mkl[version='>=2018.0.3']
      - anaconda/linux-64::spyder==3.3.4=py36_0
      - anaconda/linux-64::sympy==1.4=py36_0
      - anaconda/linux-64::torchvision==0.2.1=py36_0 -> pytorch[version='>=0.4'] -> mkl[version='>=2019.1,<2020.0a0']
      - anaconda/noarch::openpyxl==2.6.2=py_0
      - anaconda/noarch::path.py==12.0.1=py_0 -> importlib_metadata[version='>=0.5']
      - anaconda/noarch::xlsxwriter==1.1.6=py_0
      - mkl-service -> mkl[version='>=2019.4,<2020.0a0']
      - pkgs/main/linux-64::mkl==2019.0=118
      - pkgs/main/linux-64::pango==1.42.4=h049681c_0
    
    
    
    Libraries imported.


***Now, import data***


```python
trend = pd.read_csv("Delhi Population Trend.csv")
tr = pd.read_csv("Delhi Population Trend_CSV.csv")
trend.head()
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
      <th>Census</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1901</td>
      <td>405819</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1911</td>
      <td>413851</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1921</td>
      <td>488452</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1931</td>
      <td>636246</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1941</td>
      <td>9,17,939</td>
    </tr>
  </tbody>
</table>
</div>



*Plot the population trend of Delhi with repsect to time.*


```python
import seaborn as sns
ax=sns.regplot(x="Census", y="Population", data=trend, color="red", marker="4")

#please ignore the error and see the plot below error notification
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-4-338b8767b01a> in <module>
          1 import seaborn as sns
    ----> 2 ax=sns.regplot(x="Census", y="Population", data=trend, color="red", marker="4")
          3 
          4 #please ignore the error and see the plot below error notification


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in regplot(x, y, data, x_estimator, x_bins, x_ci, scatter, fit_reg, ci, n_boot, units, order, logistic, lowess, robust, logx, x_partial, y_partial, truncate, dropna, x_jitter, y_jitter, label, color, marker, scatter_kws, line_kws, ax)
        787     scatter_kws["marker"] = marker
        788     line_kws = {} if line_kws is None else copy.copy(line_kws)
    --> 789     plotter.plot(ax, scatter_kws, line_kws)
        790     return ax
        791 


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in plot(self, ax, scatter_kws, line_kws)
        342             self.scatterplot(ax, scatter_kws)
        343         if self.fit_reg:
    --> 344             self.lineplot(ax, line_kws)
        345 
        346         # Label the axes


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in lineplot(self, ax, kws)
        387 
        388         # Fit the regression model
    --> 389         grid, yhat, err_bands = self.fit_regression(ax)
        390 
        391         # Get set default aesthetics


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in fit_regression(self, ax, x_range, grid)
        206             yhat, yhat_boots = self.fit_logx(grid)
        207         else:
    --> 208             yhat, yhat_boots = self.fit_fast(grid)
        209 
        210         # Compute the confidence interval at each grid point


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in fit_fast(self, grid)
        223         X, y = np.c_[np.ones(len(self.x)), self.x], self.y
        224         grid = np.c_[np.ones(len(grid)), grid]
    --> 225         yhat = grid.dot(reg_func(X, y))
        226         if self.ci is None:
        227             return yhat, None


    ~/conda/lib/python3.6/site-packages/seaborn/regression.py in reg_func(_x, _y)
        219         """Low-level regression and prediction using linear algebra."""
        220         def reg_func(_x, _y):
    --> 221             return np.linalg.pinv(_x).dot(_y)
        222 
        223         X, y = np.c_[np.ones(len(self.x)), self.x], self.y


    TypeError: can't multiply sequence by non-int of type 'float'



![png](output_6_1.png)


 *In the above plot, we can clearly see how population has increased over time and slope slope of the plot increased tremendously for past few years*
 
 #### **B) Now, let's explore how we can derive a strategy to get control over the situation, using available data.**
 
 
 There are mainly 2 kind of population in Delhi, that significantly contributes to pollution:
 - Who are resident of Delhi. These are further of 2 kind:
         - Who are by birth, resident of Delhi.
         - who have moved to Delhi for Job or Business.
 - Who regularly visit Delhi. 
 
 *One thing common in all of above is that they have some incentive to live in or visit Delhi: whether it is career, or best quality health care service of AIIMS, or anything else. * **Problem of Delhi can be solved if we can provide these incentive, at much more convenient location outside Delhi, to above mentioned population**
 
Now, let's explore, what are these incentives are, and what can be done. 

If someone has to pass through Delhi for a for something, which is also available in Delhi, he won't go to another city for same. So let's start with ***connectivity of Delhi versus Neighboring cities*** with the other parts of country.


*Load the Foursquare Credentials.*


```python
CLIENT_ID = 'Not Sharing This With You' # your Foursquare ID
CLIENT_SECRET = 'Not Sharing This With You' # your Foursquare Secret
VERSION = '20180604'
LIMIT = 500
print('Your credentails:')
print('CLIENT_ID: ' + CLIENT_ID)
print('CLIENT_SECRET:' + CLIENT_SECRET)
```

    Your credentails:
    CLIENT_ID: D0URIR3FR3PHYXW2ROONC2WPWKSAGICT23KM35N5FE2EWQKI
    CLIENT_SECRET:LM32RNYNH1Q5JQF4FZP5OQCFZAGGMIXCGMHB5012ARWU5MKB


*Get the Delhi location, and define parameters.*


```python
address = 'Delhi, India'

geolocator = Nominatim(user_agent="foursquare_agent")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude
print(latitude, longitude)
```

    28.6517178 77.2219388


*Search for railway stations within 80 Km radius of Delhi.


```python
search_query = 'Railway station'
radius = 80000
print(search_query + ' .... OK!')
```

    Railway station .... OK!


*Create URL*


```python
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)
url
```




    'https://api.foursquare.com/v2/venues/search?client_id=D0URIR3FR3PHYXW2ROONC2WPWKSAGICT23KM35N5FE2EWQKI&client_secret=LM32RNYNH1Q5JQF4FZP5OQCFZAGGMIXCGMHB5012ARWU5MKB&ll=28.6517178,77.2219388&v=20180604&query=Railway station&radius=80000&limit=500'



*Print result as json.*


```python
results = requests.get(url).json()
results
```




    {'meta': {'code': 200, 'requestId': '5d17d654d9a6e600387cd2a7'},
     'response': {'venues': [{'id': '4c16685f7fd00f47bf2efab6',
        'name': 'New Delhi Railway Station (NDLS)',
        'location': {'address': 'Paharganj-Ajmeri Gate',
         'lat': 28.642028217894634,
         'lng': 77.21962470476957,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.642028217894634,
           'lng': 77.21962470476957}],
         'distance': 1102,
         'postalCode': '110001',
         'cc': 'IN',
         'neighborhood': 'Central Delhi',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Paharganj-Ajmeri Gate',
          'New Delhi 110001',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4c47e2fe1ddec928e5e29e32',
        'name': 'Old Delhi Railway Station (DLI)',
        'location': {'address': 'Shyama Prasad Mukherji Marg',
         'lat': 28.660579163305297,
         'lng': 77.22856521606445,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.660579163305297,
           'lng': 77.22856521606445}],
         'distance': 1179,
         'postalCode': '110001',
         'cc': 'IN',
         'city': 'Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Shyama Prasad Mukherji Marg',
          'Delhi 110001',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '518ddbdf498eab246c509547',
        'name': 'bloomrooms @ New Delhi Railway Station',
        'location': {'address': '8591, Arakashan Road',
         'crossStreet': 'opp. new delhi railway station',
         'lat': 28.64553655967833,
         'lng': 77.21770059555993,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64553655967833,
           'lng': 77.21770059555993}],
         'distance': 803,
         'postalCode': '110055',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['8591, Arakashan Road (opp. new delhi railway station)',
          'New Delhi 110055',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d1fa931735',
          'name': 'Hotel',
          'pluralName': 'Hotels',
          'shortName': 'Hotel',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/hotel_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4cca25af7965b60c3a04848a',
        'name': 'Platform No. 1, Delhi Railway Station',
        'location': {'address': 'Bhavbhuti Marg, Kamla Market, Ajmeri Gate',
         'lat': 28.642763809765903,
         'lng': 77.21941137331639,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.642763809765903,
           'lng': 77.21941137331639}],
         'distance': 1026,
         'cc': 'IN',
         'neighborhood': 'Central Delhi',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Bhavbhuti Marg, Kamla Market, Ajmeri Gate',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '527a7b8b498e0016b22a09c6',
        'name': 'Sadar Bazaar Railway Station',
        'location': {'lat': 28.654232219141104,
         'lng': 77.21801348137043,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.654232219141104,
           'lng': 77.21801348137043}],
         'distance': 474,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '532998b1498e50e6ea270522',
        'name': 'Platform Number 5 , New Delhi Railway Station',
        'location': {'address': 'New Delhi Railway Station',
         'lat': 28.64121982795762,
         'lng': 77.22001206879214,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64121982795762,
           'lng': 77.22001206879214}],
         'distance': 1183,
         'postalCode': '110001',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi Railway Station',
          'New Delhi 110001',
          'Delhi',
          'India']},
        'categories': [],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '54f84ede498eae1662c04df0',
        'name': 'The Prime Balaji Deluxe @ New Delhi Railway Station',
        'location': {'address': '8574 Arakashan Road Pahar Ganj',
         'lat': 28.645246634338402,
         'lng': 77.21743343727594,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.645246634338402,
           'lng': 77.21743343727594}],
         'distance': 844,
         'postalCode': '110055',
         'cc': 'IN',
         'neighborhood': 'Central Delhi',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['8574 Arakashan Road Pahar Ganj',
          'New Delhi 110055',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d1fa931735',
          'name': 'Hotel',
          'pluralName': 'Hotels',
          'shortName': 'Hotel',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/hotel_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4e79fb4fd22d391717d89069',
        'name': 'Delhi Subzi Mandi Railway Station (SZM)',
        'location': {'lat': 28.668121127109586,
         'lng': 77.20027173048291,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.668121127109586,
           'lng': 77.20027173048291}],
         'distance': 2795,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4b99ae37f964a520a28b35e3',
        'name': 'Sarai Rohilla | सराय रोहिल्ला Railway Station',
        'location': {'address': 'New Rohtak Road, Sarai Rohilla, NH 10',
         'lat': 28.663130407563944,
         'lng': 77.1870231628418,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.663130407563944,
           'lng': 77.1870231628418}],
         'distance': 3639,
         'postalCode': '110035',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Rohtak Road, Sarai Rohilla, NH 10',
          'New Delhi 110035',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4cc1598b56bda0932250eb35',
        'name': 'Hazrat Nizamuddin Railway Station (NZM)',
        'location': {'address': 'Nizamuddin',
         'lat': 28.588511067944086,
         'lng': 77.2540277761072,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.588511067944086,
           'lng': 77.2540277761072}],
         'distance': 7703,
         'postalCode': '110013',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Nizamuddin', 'New Delhi 110013', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d5e56099f67f04df20f5ffb',
        'name': 'Railway Station',
        'location': {'address': 'Patna rajdhani',
         'lat': 28.642526135540994,
         'lng': 77.22013557589467,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.642526135540994,
           'lng': 77.22013557589467}],
         'distance': 1038,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Patna rajdhani', 'New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d12a951735',
          'name': 'Train',
          'pluralName': 'Trains',
          'shortName': 'Train',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '58d4ab6c44689a1e9b7e242f',
        'name': "Optimum Palm D'or @ New Delhi Railway station",
        'location': {'address': '8595/1',
         'crossStreet': 'D.B Gupta Road',
         'lat': 28.64487493481358,
         'lng': 77.21744075417519,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64487493481358,
           'lng': 77.21744075417519}],
         'distance': 879,
         'postalCode': '110055',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['8595/1 (D.B Gupta Road)',
          'New Delhi 110055',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d1f8931735',
          'name': 'Bed & Breakfast',
          'pluralName': 'Bed & Breakfasts',
          'shortName': 'B & B',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/bedandbreakfast_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4da8814f43a12d0c26535135',
        'name': 'Platform No. 4 - Delhi Railway Station',
        'location': {'address': 'New Delhi Railway Station',
         'lat': 28.643799672960405,
         'lng': 77.21970330047104,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.643799672960405,
           'lng': 77.21970330047104}],
         'distance': 908,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['New Delhi Railway Station', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '51e7e3d3498eaefba836de20',
        'name': 'Chawndni Chowk Railway Station',
        'location': {'lat': 28.657038,
         'lng': 77.230049,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.657038,
           'lng': 77.230049}],
         'distance': 989,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d1fd931735',
          'name': 'Metro Station',
          'pluralName': 'Metro Stations',
          'shortName': 'Metro',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/subway_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '528eb22c11d2d652f1bf03fb',
        'name': 'New Delhi Railway Station , Platform No 2',
        'location': {'lat': 28.642647519082534,
         'lng': 77.21950681111937,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.642647519082534,
           'lng': 77.21950681111937}],
         'distance': 1037,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '50c51f85e4b08c3b5a794923',
        'name': 'Platfrom No. 3, New Delhi Railway Station',
        'location': {'lat': 28.64277132489119,
         'lng': 77.21975382625152,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64277132489119,
           'lng': 77.21975382625152}],
         'distance': 1018,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4ccd383197d0224b982065b8',
        'name': 'Platform No.14, Delhi Railway Station',
        'location': {'lat': 28.641560044611264,
         'lng': 77.22137625371724,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.641560044611264,
           'lng': 77.22137625371724}],
         'distance': 1132,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4e5dc34414954da39fe72e16',
        'name': 'Upper Class Waiting Hall New Delhi Railway Station',
        'location': {'address': 'New Delhi Railway Station',
         'lat': 28.641487107533884,
         'lng': 77.22094398283251,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.641487107533884,
           'lng': 77.22094398283251}],
         'distance': 1143,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi Railway Station',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4d954b16a243a5684b65b473',
          'name': 'Rest Area',
          'pluralName': 'Rest Areas',
          'shortName': 'Rest Areas',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/restarea_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '5092a460e4b07db78b993b1a',
        'name': 'Platform No 8 New Delhi Railway station',
        'location': {'lat': 28.641377793179192,
         'lng': 77.22083731834495,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.641377793179192,
           'lng': 77.22083731834495}],
         'distance': 1156,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4cf528b61d18a143c41f6aec',
        'name': 'Alighting area at new delhi railway station',
        'location': {'address': 'New delhi railway station',
         'lat': 28.641397417010225,
         'lng': 77.22203307787835,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.641397417010225,
           'lng': 77.22203307787835}],
         'distance': 1148,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New delhi railway station',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4b9cd730f964a520317e36e3',
        'name': 'Delhi Cantt Railway Station',
        'location': {'address': 'Delhi Cantonment',
         'crossStreet': 'Jail Road',
         'lat': 28.611056656881633,
         'lng': 77.11492270980183,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.611056656881633,
           'lng': 77.11492270980183}],
         'distance': 11393,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Delhi Cantonment (Jail Road)',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '51846d49498e908674a39b74',
        'name': 'Kalka Shatabdi, New delhi railway station',
        'location': {'lat': 28.640846252441406,
         'lng': 77.22261047363281,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.640846252441406,
           'lng': 77.22261047363281}],
         'distance': 1211,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d12a951735',
          'name': 'Train',
          'pluralName': 'Trains',
          'shortName': 'Train',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '564f0b2a498ea3de98ac892f',
        'name': 'New Delhi Railway Station',
        'location': {'lat': 28.662429385847595,
         'lng': 77.21608222995867,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.662429385847595,
           'lng': 77.21608222995867}],
         'distance': 1322,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d12a951735',
          'name': 'Train',
          'pluralName': 'Trains',
          'shortName': 'Train',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4ea2f280c2eefedd04052b0b',
        'name': 'Tilak Bridge Railway Station',
        'location': {'address': 'Deen Dayal Upaddhya Marg',
         'lat': 28.627775,
         'lng': 77.237828,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.627775,
           'lng': 77.237828}],
         'distance': 3084,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Deen Dayal Upaddhya Marg',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4c746e813adda1433ac504af',
        'name': 'Gurgaon Railway Station (GGN)',
        'location': {'address': 'Kheri, Ashok Vihar',
         'crossStreet': 'Railway Rd.',
         'lat': 28.489326102486906,
         'lng': 77.01124747794933,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.489326102486906,
           'lng': 77.01124747794933}],
         'distance': 27405,
         'cc': 'IN',
         'city': 'Gurgaon',
         'state': 'Haryāna',
         'country': 'India',
         'formattedAddress': ['Kheri, Ashok Vihar (Railway Rd.)',
          'Gurgaon',
          'Haryāna',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '532d6f47498ec51f061b8d95',
        'name': 'New Delhi Railway Station',
        'location': {'lat': 28.631552078404606,
         'lng': 77.22760190177725,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.631552078404606,
           'lng': 77.22760190177725}],
         'distance': 2312,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '50c51c27e4b0e2b40d30c29f',
        'name': 'Delhi Kishan Ganj Railway Station',
        'location': {'lat': 28.663164067441667,
         'lng': 77.19933965359665,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.663164067441667,
           'lng': 77.19933965359665}],
         'distance': 2548,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4ea2f21bc2eefedd04051a1f',
        'name': 'Shivaji Bridge Railway Station',
        'location': {'address': 'Shivaji Bridge',
         'lat': 28.633556640776597,
         'lng': 77.22608685493469,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.633556640776597,
           'lng': 77.22608685493469}],
         'distance': 2061,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'NCR',
         'country': 'India',
         'formattedAddress': ['Shivaji Bridge', 'New Delhi', 'NCR', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4e03021062e189f23d67a52f',
        'name': 'Delhi Safdarjang Railway Station',
        'location': {'address': 'Safdarjang',
         'lat': 28.5823925241335,
         'lng': 77.18620777130127,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.5823925241335,
           'lng': 77.18620777130127}],
         'distance': 8470,
         'postalCode': '110001',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Safdarjang', 'New Delhi 110001', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '515588a8e4b06f694a93728a',
        'name': 'Nangloi Railway Station Metro Station',
        'location': {'lat': 28.682173342499112,
         'lng': 77.05621719360352,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.682173342499112,
           'lng': 77.05621719360352}],
         'distance': 16537,
         'postalCode': '110041',
         'cc': 'IN',
         'city': 'Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Delhi 110041', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d1fd931735',
          'name': 'Metro Station',
          'pluralName': 'Metro Stations',
          'shortName': 'Metro',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/subway_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d305288a241f04d7032b327',
        'name': 'Ballabgarh Railway Station',
        'location': {'address': 'MDR 133, Sector 25, Ballabgarh',
         'lat': 28.34285702104608,
         'lng': 77.31239846502024,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.34285702104608,
           'lng': 77.31239846502024}],
         'distance': 35502,
         'cc': 'IN',
         'city': 'Farīdābād',
         'state': 'Haryāna',
         'country': 'India',
         'formattedAddress': ['MDR 133, Sector 25, Ballabgarh',
          'Farīdābād',
          'Haryāna',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4def0067887754a6af891f6e',
        'name': 'Ghaziabad Railway Station',
        'location': {'lat': 28.653157248859525,
         'lng': 77.42796505293477,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.653157248859525,
           'lng': 77.42796505293477}],
         'distance': 20126,
         'postalCode': '201009',
         'cc': 'IN',
         'city': 'Ghāziābād',
         'state': 'Uttar Pradesh',
         'country': 'India',
         'formattedAddress': ['Ghāziābād 201009', 'Uttar Pradesh', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4cb25a691168a093308e3a23',
        'name': 'Delhi Shahdara Junction railway station',
        'location': {'address': 'Shahdara',
         'lat': 28.672882931205567,
         'lng': 77.29251980781555,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.672882931205567,
           'lng': 77.29251980781555}],
         'distance': 7285,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Shahdara', 'New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '523c430111d2b0c267cb3d7e',
        'name': 'New delhi Railway station',
        'location': {'lat': 28.627971961651564,
         'lng': 77.22179729526631,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.627971961651564,
           'lng': 77.22179729526631}],
         'distance': 2643,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d12a951735',
          'name': 'Train',
          'pluralName': 'Trains',
          'shortName': 'Train',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4cee9b4a5de16ea8c396bc96',
        'name': 'Shivaji Bridge Railway station',
        'location': {'crossStreet': 'Connaught Place',
         'lat': 28.62654,
         'lng': 77.2283087,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.62654,
           'lng': 77.2283087}],
         'distance': 2871,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi', 'Delhi', 'India']},
        'categories': [],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d3ebb8d457cb60c620603a5',
        'name': 'Dayabasti Railway Station',
        'location': {'address': 'Daya basti',
         'lat': 28.66690538200464,
         'lng': 77.17161655426025,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.66690538200464,
           'lng': 77.17161655426025}],
         'distance': 5198,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Daya basti', 'New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4ec8963d77c8be492405be5e',
        'name': 'Tughlakabad Railway Station',
        'location': {'lat': 28.504788835873445,
         'lng': 77.29624271392822,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.504788835873445,
           'lng': 77.29624271392822}],
         'distance': 17896,
         'postalCode': '110044',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi 110044', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4c9f6a7b0e9bb1f71a19f05f',
        'name': 'Anand Vihar Railway Station',
        'location': {'address': 'Anand Vihar',
         'lat': 28.64873641274196,
         'lng': 77.3156800054386,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64873641274196,
           'lng': 77.3156800054386}],
         'distance': 9163,
         'postalCode': '110092',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Anand Vihar',
          'New Delhi 110092',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d1b6241c5f5b1f75f73db36',
        'name': 'Narela Railway Station',
        'location': {'lat': 28.858209495803468,
         'lng': 77.12418233924228,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.858209495803468,
           'lng': 77.12418233924228}],
         'distance': 24887,
         'postalCode': '110040',
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi 110040', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '52818f6911d2ac719a76b51f',
        'name': 'Mangolpuri Railway Station',
        'location': {'lat': 28.683602224933377,
         'lng': 77.09146702339936,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.683602224933377,
           'lng': 77.09146702339936}],
         'distance': 13228,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '50bf70b9e4b089ddbdf3901e',
        'name': 'Purani Dilli Railway Station',
        'location': {'address': 'Near red Fort',
         'crossStreet': 'Opposite Chandni Chowk',
         'lat': 28.602825164794922,
         'lng': 77.24996948242188,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.602825164794922,
           'lng': 77.24996948242188}],
         'distance': 6092,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['Near red Fort (Opposite Chandni Chowk)',
          'New Delhi',
          'Delhi',
          'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4ea2f353c2eefedd04054d56',
        'name': 'Pragati Maidan Railway Station',
        'location': {'address': 'Mahatma Gandhi Marg',
         'lat': 28.615456,
         'lng': 77.247226,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.615456,
           'lng': 77.247226}],
         'distance': 4732,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'NCR',
         'country': 'India',
         'formattedAddress': ['Mahatma Gandhi Marg', 'New Delhi', 'NCR', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d5f425a196ba0936df60b56',
        'name': 'Sahibabaad Railway Station',
        'location': {'lat': 28.673920397788407,
         'lng': 77.36498734824607,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.673920397788407,
           'lng': 77.36498734824607}],
         'distance': 14189,
         'postalCode': '201005',
         'cc': 'IN',
         'city': 'Ghāziābād',
         'state': 'Uttar Pradesh',
         'country': 'India',
         'formattedAddress': ['Ghāziābād 201005', 'Uttar Pradesh', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '50a09574e4b007e3c2f870da',
        'name': 'Ganaur Railway Station',
        'location': {'lat': 29.13228085068735,
         'lng': 77.01159125077561,
         'labeledLatLngs': [{'label': 'display',
           'lat': 29.13228085068735,
           'lng': 77.01159125077561}],
         'distance': 57289,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '5661d163498eb2bfac97305b',
        'name': 'Hazrat Nizamuddin Railway Station Platform No.4',
        'location': {'lat': 28.589252376151666,
         'lng': 77.25413441057447,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.589252376151666,
           'lng': 77.25413441057447}],
         'distance': 7632,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4f4531504b9074f6e4fb0102',
          'name': 'Platform',
          'pluralName': 'Platforms',
          'shortName': 'Platform',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4e7a103145dd082cbc3128fa',
        'name': 'Sonipat Railway Station',
        'location': {'lat': 28.989455670526425,
         'lng': 77.017602885143,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.989455670526425,
           'lng': 77.017602885143}],
         'distance': 42551,
         'cc': 'IN',
         'neighborhood': 'Sukhdev dhaba',
         'city': 'Sonīpat',
         'state': 'Haryāna',
         'country': 'India',
         'formattedAddress': ['Sonīpat', 'Haryāna', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4f4cec33e4b0cb12b890112c',
        'name': 'Gtb Nagar Railway Station',
        'location': {'address': 'GTB Nagar, Guru Teg Bahadur Nagar',
         'lat': 28.707183121238955,
         'lng': 77.20675676115992,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.707183121238955,
           'lng': 77.20675676115992}],
         'distance': 6349,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['GTB Nagar, Guru Teg Bahadur Nagar', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d1fee71756e8cfa9cbb6754',
        'name': 'Kirti nagar Railway Station',
        'location': {'lat': 28.64688046188621,
         'lng': 77.1455454826355,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.64688046188621,
           'lng': 77.1455454826355}],
         'distance': 7482,
         'cc': 'IN',
         'country': 'India',
         'formattedAddress': ['India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '4d3be3e444f5a093080fc8bb',
        'name': 'Rohtak Railway Station',
        'location': {'address': 'Rohtak',
         'lat': 28.890237227355627,
         'lng': 76.58123327506917,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.890237227355627,
           'lng': 76.58123327506917}],
         'distance': 67922,
         'postalCode': '124001',
         'cc': 'IN',
         'city': 'Rohtak',
         'state': 'Haryāna',
         'country': 'India',
         'formattedAddress': ['Rohtak', 'Rohtak 124001', 'Haryāna', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False},
       {'id': '5aedd85c603d2a002d9536fd',
        'name': 'Naraina Vihar Railway Station',
        'location': {'lat': 28.636896,
         'lng': 77.141307,
         'labeledLatLngs': [{'label': 'display',
           'lat': 28.636896,
           'lng': 77.141307}],
         'distance': 8048,
         'cc': 'IN',
         'city': 'New Delhi',
         'state': 'Delhi',
         'country': 'India',
         'formattedAddress': ['New Delhi', 'Delhi', 'India']},
        'categories': [{'id': '4bf58dd8d48988d129951735',
          'name': 'Train Station',
          'pluralName': 'Train Stations',
          'shortName': 'Train Station',
          'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/travel/trainstation_',
           'suffix': '.png'},
          'primary': True}],
        'referralId': 'v-1561843284',
        'hasPerk': False}]}}




```python
# assign relevant part of JSON to venues
venues = results['response']['venues']

# tranform venues into a dataframe
dataframe = json_normalize(venues)
dataframe.head()
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
      <th>categories</th>
      <th>hasPerk</th>
      <th>id</th>
      <th>location.address</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.crossStreet</th>
      <th>location.distance</th>
      <th>location.formattedAddress</th>
      <th>location.labeledLatLngs</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.neighborhood</th>
      <th>location.postalCode</th>
      <th>location.state</th>
      <th>name</th>
      <th>referralId</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[{'id': '4bf58dd8d48988d129951735', 'name': 'T...</td>
      <td>False</td>
      <td>4c16685f7fd00f47bf2efab6</td>
      <td>Paharganj-Ajmeri Gate</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1102</td>
      <td>[Paharganj-Ajmeri Gate, New Delhi 110001, Delh...</td>
      <td>[{'label': 'display', 'lat': 28.64202821789463...</td>
      <td>28.642028</td>
      <td>77.219625</td>
      <td>Central Delhi</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>New Delhi Railway Station (NDLS)</td>
      <td>v-1561843284</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[{'id': '4bf58dd8d48988d129951735', 'name': 'T...</td>
      <td>False</td>
      <td>4c47e2fe1ddec928e5e29e32</td>
      <td>Shyama Prasad Mukherji Marg</td>
      <td>IN</td>
      <td>Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1179</td>
      <td>[Shyama Prasad Mukherji Marg, Delhi 110001, De...</td>
      <td>[{'label': 'display', 'lat': 28.66057916330529...</td>
      <td>28.660579</td>
      <td>77.228565</td>
      <td>NaN</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>Old Delhi Railway Station (DLI)</td>
      <td>v-1561843284</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>518ddbdf498eab246c509547</td>
      <td>8591, Arakashan Road</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>opp. new delhi railway station</td>
      <td>803</td>
      <td>[8591, Arakashan Road (opp. new delhi railway ...</td>
      <td>[{'label': 'display', 'lat': 28.64553655967833...</td>
      <td>28.645537</td>
      <td>77.217701</td>
      <td>NaN</td>
      <td>110055</td>
      <td>Delhi</td>
      <td>bloomrooms @ New Delhi Railway Station</td>
      <td>v-1561843284</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[{'id': '4bf58dd8d48988d129951735', 'name': 'T...</td>
      <td>False</td>
      <td>4cca25af7965b60c3a04848a</td>
      <td>Bhavbhuti Marg, Kamla Market, Ajmeri Gate</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1026</td>
      <td>[Bhavbhuti Marg, Kamla Market, Ajmeri Gate, Ne...</td>
      <td>[{'label': 'display', 'lat': 28.64276380976590...</td>
      <td>28.642764</td>
      <td>77.219411</td>
      <td>Central Delhi</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>Platform No. 1, Delhi Railway Station</td>
      <td>v-1561843284</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[]</td>
      <td>False</td>
      <td>527a7b8b498e0016b22a09c6</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>474</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.65423221914110...</td>
      <td>28.654232</td>
      <td>77.218013</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Sadar Bazaar Railway Station</td>
      <td>v-1561843284</td>
    </tr>
  </tbody>
</table>
</div>




```python
# keep only columns that include venue name, and anything that is associated with location
filtered_columns = ['name', 'categories'] + [col for col in dataframe.columns if col.startswith('location.')] + ['id']
dataframe_filtered = dataframe.loc[:, filtered_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']
        
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']

# filter the category for each row
dataframe_filtered['categories'] = dataframe_filtered.apply(get_category_type, axis=1)

# clean column names by keeping only last term
dataframe_filtered.columns = [column.split('.')[-1] for column in dataframe_filtered.columns]

dataframe_filtered
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>neighborhood</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>New Delhi Railway Station (NDLS)</td>
      <td>Train Station</td>
      <td>Paharganj-Ajmeri Gate</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1102</td>
      <td>[Paharganj-Ajmeri Gate, New Delhi 110001, Delh...</td>
      <td>[{'label': 'display', 'lat': 28.64202821789463...</td>
      <td>28.642028</td>
      <td>77.219625</td>
      <td>Central Delhi</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>4c16685f7fd00f47bf2efab6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Old Delhi Railway Station (DLI)</td>
      <td>Train Station</td>
      <td>Shyama Prasad Mukherji Marg</td>
      <td>IN</td>
      <td>Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1179</td>
      <td>[Shyama Prasad Mukherji Marg, Delhi 110001, De...</td>
      <td>[{'label': 'display', 'lat': 28.66057916330529...</td>
      <td>28.660579</td>
      <td>77.228565</td>
      <td>NaN</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>4c47e2fe1ddec928e5e29e32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bloomrooms @ New Delhi Railway Station</td>
      <td>Hotel</td>
      <td>8591, Arakashan Road</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>opp. new delhi railway station</td>
      <td>803</td>
      <td>[8591, Arakashan Road (opp. new delhi railway ...</td>
      <td>[{'label': 'display', 'lat': 28.64553655967833...</td>
      <td>28.645537</td>
      <td>77.217701</td>
      <td>NaN</td>
      <td>110055</td>
      <td>Delhi</td>
      <td>518ddbdf498eab246c509547</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Platform No. 1, Delhi Railway Station</td>
      <td>Train Station</td>
      <td>Bhavbhuti Marg, Kamla Market, Ajmeri Gate</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1026</td>
      <td>[Bhavbhuti Marg, Kamla Market, Ajmeri Gate, Ne...</td>
      <td>[{'label': 'display', 'lat': 28.64276380976590...</td>
      <td>28.642764</td>
      <td>77.219411</td>
      <td>Central Delhi</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4cca25af7965b60c3a04848a</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sadar Bazaar Railway Station</td>
      <td>None</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>474</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.65423221914110...</td>
      <td>28.654232</td>
      <td>77.218013</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>527a7b8b498e0016b22a09c6</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Platform Number 5 , New Delhi Railway Station</td>
      <td>None</td>
      <td>New Delhi Railway Station</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1183</td>
      <td>[New Delhi Railway Station, New Delhi 110001, ...</td>
      <td>[{'label': 'display', 'lat': 28.64121982795762...</td>
      <td>28.641220</td>
      <td>77.220012</td>
      <td>NaN</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>532998b1498e50e6ea270522</td>
    </tr>
    <tr>
      <th>6</th>
      <td>The Prime Balaji Deluxe @ New Delhi Railway St...</td>
      <td>Hotel</td>
      <td>8574 Arakashan Road Pahar Ganj</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>844</td>
      <td>[8574 Arakashan Road Pahar Ganj, New Delhi 110...</td>
      <td>[{'label': 'display', 'lat': 28.64524663433840...</td>
      <td>28.645247</td>
      <td>77.217433</td>
      <td>Central Delhi</td>
      <td>110055</td>
      <td>Delhi</td>
      <td>54f84ede498eae1662c04df0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Delhi Subzi Mandi Railway Station (SZM)</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>2795</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.66812112710958...</td>
      <td>28.668121</td>
      <td>77.200272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4e79fb4fd22d391717d89069</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Sarai Rohilla | सराय रोहिल्ला Railway Station</td>
      <td>Train Station</td>
      <td>New Rohtak Road, Sarai Rohilla, NH 10</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>3639</td>
      <td>[New Rohtak Road, Sarai Rohilla, NH 10, New De...</td>
      <td>[{'label': 'display', 'lat': 28.66313040756394...</td>
      <td>28.663130</td>
      <td>77.187023</td>
      <td>NaN</td>
      <td>110035</td>
      <td>Delhi</td>
      <td>4b99ae37f964a520a28b35e3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Hazrat Nizamuddin Railway Station (NZM)</td>
      <td>Train Station</td>
      <td>Nizamuddin</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>7703</td>
      <td>[Nizamuddin, New Delhi 110013, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.58851106794408...</td>
      <td>28.588511</td>
      <td>77.254028</td>
      <td>NaN</td>
      <td>110013</td>
      <td>Delhi</td>
      <td>4cc1598b56bda0932250eb35</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Railway Station</td>
      <td>Train</td>
      <td>Patna rajdhani</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1038</td>
      <td>[Patna rajdhani, New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.64252613554099...</td>
      <td>28.642526</td>
      <td>77.220136</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4d5e56099f67f04df20f5ffb</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Optimum Palm D'or @ New Delhi Railway station</td>
      <td>Bed &amp; Breakfast</td>
      <td>8595/1</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>D.B Gupta Road</td>
      <td>879</td>
      <td>[8595/1 (D.B Gupta Road), New Delhi 110055, De...</td>
      <td>[{'label': 'display', 'lat': 28.64487493481358...</td>
      <td>28.644875</td>
      <td>77.217441</td>
      <td>NaN</td>
      <td>110055</td>
      <td>Delhi</td>
      <td>58d4ab6c44689a1e9b7e242f</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Platform No. 4 - Delhi Railway Station</td>
      <td>Train Station</td>
      <td>New Delhi Railway Station</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>908</td>
      <td>[New Delhi Railway Station, India]</td>
      <td>[{'label': 'display', 'lat': 28.64379967296040...</td>
      <td>28.643800</td>
      <td>77.219703</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4da8814f43a12d0c26535135</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Chawndni Chowk Railway Station</td>
      <td>Metro Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>989</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.657038, 'lng':...</td>
      <td>28.657038</td>
      <td>77.230049</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51e7e3d3498eaefba836de20</td>
    </tr>
    <tr>
      <th>14</th>
      <td>New Delhi Railway Station , Platform No 2</td>
      <td>Platform</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>1037</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.64264751908253...</td>
      <td>28.642648</td>
      <td>77.219507</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>528eb22c11d2d652f1bf03fb</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Platfrom No. 3, New Delhi Railway Station</td>
      <td>Platform</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>1018</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.64277132489119...</td>
      <td>28.642771</td>
      <td>77.219754</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>50c51f85e4b08c3b5a794923</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Platform No.14, Delhi Railway Station</td>
      <td>Platform</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1132</td>
      <td>[New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.64156004461126...</td>
      <td>28.641560</td>
      <td>77.221376</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4ccd383197d0224b982065b8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Upper Class Waiting Hall New Delhi Railway Sta...</td>
      <td>Rest Area</td>
      <td>New Delhi Railway Station</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1143</td>
      <td>[New Delhi Railway Station, New Delhi, Delhi, ...</td>
      <td>[{'label': 'display', 'lat': 28.64148710753388...</td>
      <td>28.641487</td>
      <td>77.220944</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4e5dc34414954da39fe72e16</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Platform No 8 New Delhi Railway station</td>
      <td>Platform</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>1156</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.64137779317919...</td>
      <td>28.641378</td>
      <td>77.220837</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5092a460e4b07db78b993b1a</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Alighting area at new delhi railway station</td>
      <td>Train Station</td>
      <td>New delhi railway station</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>1148</td>
      <td>[New delhi railway station, New Delhi, Delhi, ...</td>
      <td>[{'label': 'display', 'lat': 28.64139741701022...</td>
      <td>28.641397</td>
      <td>77.222033</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4cf528b61d18a143c41f6aec</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Delhi Cantt Railway Station</td>
      <td>Train Station</td>
      <td>Delhi Cantonment</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>Jail Road</td>
      <td>11393</td>
      <td>[Delhi Cantonment (Jail Road), New Delhi, Delh...</td>
      <td>[{'label': 'display', 'lat': 28.61105665688163...</td>
      <td>28.611057</td>
      <td>77.114923</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4b9cd730f964a520317e36e3</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Kalka Shatabdi, New delhi railway station</td>
      <td>Train</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>1211</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.64084625244140...</td>
      <td>28.640846</td>
      <td>77.222610</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51846d49498e908674a39b74</td>
    </tr>
    <tr>
      <th>22</th>
      <td>New Delhi Railway Station</td>
      <td>Train</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>1322</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.66242938584759...</td>
      <td>28.662429</td>
      <td>77.216082</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>564f0b2a498ea3de98ac892f</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Tilak Bridge Railway Station</td>
      <td>Train Station</td>
      <td>Deen Dayal Upaddhya Marg</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>3084</td>
      <td>[Deen Dayal Upaddhya Marg, New Delhi, Delhi, I...</td>
      <td>[{'label': 'display', 'lat': 28.627775, 'lng':...</td>
      <td>28.627775</td>
      <td>77.237828</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4ea2f280c2eefedd04052b0b</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Gurgaon Railway Station (GGN)</td>
      <td>Train Station</td>
      <td>Kheri, Ashok Vihar</td>
      <td>IN</td>
      <td>Gurgaon</td>
      <td>India</td>
      <td>Railway Rd.</td>
      <td>27405</td>
      <td>[Kheri, Ashok Vihar (Railway Rd.), Gurgaon, Ha...</td>
      <td>[{'label': 'display', 'lat': 28.48932610248690...</td>
      <td>28.489326</td>
      <td>77.011247</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Haryāna</td>
      <td>4c746e813adda1433ac504af</td>
    </tr>
    <tr>
      <th>25</th>
      <td>New Delhi Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>2312</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.63155207840460...</td>
      <td>28.631552</td>
      <td>77.227602</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>532d6f47498ec51f061b8d95</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Delhi Kishan Ganj Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>2548</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.66316406744166...</td>
      <td>28.663164</td>
      <td>77.199340</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>50c51c27e4b0e2b40d30c29f</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Shivaji Bridge Railway Station</td>
      <td>Train Station</td>
      <td>Shivaji Bridge</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>2061</td>
      <td>[Shivaji Bridge, New Delhi, NCR, India]</td>
      <td>[{'label': 'display', 'lat': 28.63355664077659...</td>
      <td>28.633557</td>
      <td>77.226087</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NCR</td>
      <td>4ea2f21bc2eefedd04051a1f</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Delhi Safdarjang Railway Station</td>
      <td>Train Station</td>
      <td>Safdarjang</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>8470</td>
      <td>[Safdarjang, New Delhi 110001, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.5823925241335,...</td>
      <td>28.582393</td>
      <td>77.186208</td>
      <td>NaN</td>
      <td>110001</td>
      <td>Delhi</td>
      <td>4e03021062e189f23d67a52f</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Nangloi Railway Station Metro Station</td>
      <td>Metro Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>16537</td>
      <td>[Delhi 110041, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.68217334249911...</td>
      <td>28.682173</td>
      <td>77.056217</td>
      <td>NaN</td>
      <td>110041</td>
      <td>Delhi</td>
      <td>515588a8e4b06f694a93728a</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Ballabgarh Railway Station</td>
      <td>Train Station</td>
      <td>MDR 133, Sector 25, Ballabgarh</td>
      <td>IN</td>
      <td>Farīdābād</td>
      <td>India</td>
      <td>NaN</td>
      <td>35502</td>
      <td>[MDR 133, Sector 25, Ballabgarh, Farīdābād, Ha...</td>
      <td>[{'label': 'display', 'lat': 28.34285702104608...</td>
      <td>28.342857</td>
      <td>77.312398</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Haryāna</td>
      <td>4d305288a241f04d7032b327</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Ghaziabad Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>Ghāziābād</td>
      <td>India</td>
      <td>NaN</td>
      <td>20126</td>
      <td>[Ghāziābād 201009, Uttar Pradesh, India]</td>
      <td>[{'label': 'display', 'lat': 28.65315724885952...</td>
      <td>28.653157</td>
      <td>77.427965</td>
      <td>NaN</td>
      <td>201009</td>
      <td>Uttar Pradesh</td>
      <td>4def0067887754a6af891f6e</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Delhi Shahdara Junction railway station</td>
      <td>Platform</td>
      <td>Shahdara</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>7285</td>
      <td>[Shahdara, New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.67288293120556...</td>
      <td>28.672883</td>
      <td>77.292520</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4cb25a691168a093308e3a23</td>
    </tr>
    <tr>
      <th>33</th>
      <td>New delhi Railway station</td>
      <td>Train</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>2643</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.62797196165156...</td>
      <td>28.627972</td>
      <td>77.221797</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>523c430111d2b0c267cb3d7e</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Shivaji Bridge Railway station</td>
      <td>None</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>Connaught Place</td>
      <td>2871</td>
      <td>[New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.62654, 'lng': ...</td>
      <td>28.626540</td>
      <td>77.228309</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4cee9b4a5de16ea8c396bc96</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Dayabasti Railway Station</td>
      <td>Train Station</td>
      <td>Daya basti</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>5198</td>
      <td>[Daya basti, New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.66690538200464...</td>
      <td>28.666905</td>
      <td>77.171617</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>4d3ebb8d457cb60c620603a5</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Tughlakabad Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>17896</td>
      <td>[New Delhi 110044, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.50478883587344...</td>
      <td>28.504789</td>
      <td>77.296243</td>
      <td>NaN</td>
      <td>110044</td>
      <td>Delhi</td>
      <td>4ec8963d77c8be492405be5e</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Anand Vihar Railway Station</td>
      <td>Train Station</td>
      <td>Anand Vihar</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>9163</td>
      <td>[Anand Vihar, New Delhi 110092, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.64873641274196...</td>
      <td>28.648736</td>
      <td>77.315680</td>
      <td>NaN</td>
      <td>110092</td>
      <td>Delhi</td>
      <td>4c9f6a7b0e9bb1f71a19f05f</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Narela Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>24887</td>
      <td>[New Delhi 110040, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.85820949580346...</td>
      <td>28.858209</td>
      <td>77.124182</td>
      <td>NaN</td>
      <td>110040</td>
      <td>Delhi</td>
      <td>4d1b6241c5f5b1f75f73db36</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Mangolpuri Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>13228</td>
      <td>[New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.68360222493337...</td>
      <td>28.683602</td>
      <td>77.091467</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>52818f6911d2ac719a76b51f</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Purani Dilli Railway Station</td>
      <td>Train Station</td>
      <td>Near red Fort</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>Opposite Chandni Chowk</td>
      <td>6092</td>
      <td>[Near red Fort (Opposite Chandni Chowk), New D...</td>
      <td>[{'label': 'display', 'lat': 28.60282516479492...</td>
      <td>28.602825</td>
      <td>77.249969</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>50bf70b9e4b089ddbdf3901e</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Pragati Maidan Railway Station</td>
      <td>Train Station</td>
      <td>Mahatma Gandhi Marg</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>4732</td>
      <td>[Mahatma Gandhi Marg, New Delhi, NCR, India]</td>
      <td>[{'label': 'display', 'lat': 28.615456, 'lng':...</td>
      <td>28.615456</td>
      <td>77.247226</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NCR</td>
      <td>4ea2f353c2eefedd04054d56</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Sahibabaad Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>Ghāziābād</td>
      <td>India</td>
      <td>NaN</td>
      <td>14189</td>
      <td>[Ghāziābād 201005, Uttar Pradesh, India]</td>
      <td>[{'label': 'display', 'lat': 28.67392039778840...</td>
      <td>28.673920</td>
      <td>77.364987</td>
      <td>NaN</td>
      <td>201005</td>
      <td>Uttar Pradesh</td>
      <td>4d5f425a196ba0936df60b56</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Ganaur Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>57289</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 29.13228085068735...</td>
      <td>29.132281</td>
      <td>77.011591</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>50a09574e4b007e3c2f870da</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Hazrat Nizamuddin Railway Station Platform No.4</td>
      <td>Platform</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>7632</td>
      <td>[New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.58925237615166...</td>
      <td>28.589252</td>
      <td>77.254134</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>5661d163498eb2bfac97305b</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Sonipat Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>Sonīpat</td>
      <td>India</td>
      <td>NaN</td>
      <td>42551</td>
      <td>[Sonīpat, Haryāna, India]</td>
      <td>[{'label': 'display', 'lat': 28.98945567052642...</td>
      <td>28.989456</td>
      <td>77.017603</td>
      <td>Sukhdev dhaba</td>
      <td>NaN</td>
      <td>Haryāna</td>
      <td>4e7a103145dd082cbc3128fa</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Gtb Nagar Railway Station</td>
      <td>Train Station</td>
      <td>GTB Nagar, Guru Teg Bahadur Nagar</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>6349</td>
      <td>[GTB Nagar, Guru Teg Bahadur Nagar, India]</td>
      <td>[{'label': 'display', 'lat': 28.70718312123895...</td>
      <td>28.707183</td>
      <td>77.206757</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4f4cec33e4b0cb12b890112c</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Kirti nagar Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>NaN</td>
      <td>India</td>
      <td>NaN</td>
      <td>7482</td>
      <td>[India]</td>
      <td>[{'label': 'display', 'lat': 28.64688046188621...</td>
      <td>28.646880</td>
      <td>77.145545</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4d1fee71756e8cfa9cbb6754</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Rohtak Railway Station</td>
      <td>Train Station</td>
      <td>Rohtak</td>
      <td>IN</td>
      <td>Rohtak</td>
      <td>India</td>
      <td>NaN</td>
      <td>67922</td>
      <td>[Rohtak, Rohtak 124001, Haryāna, India]</td>
      <td>[{'label': 'display', 'lat': 28.89023722735562...</td>
      <td>28.890237</td>
      <td>76.581233</td>
      <td>NaN</td>
      <td>124001</td>
      <td>Haryāna</td>
      <td>4d3be3e444f5a093080fc8bb</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Naraina Vihar Railway Station</td>
      <td>Train Station</td>
      <td>NaN</td>
      <td>IN</td>
      <td>New Delhi</td>
      <td>India</td>
      <td>NaN</td>
      <td>8048</td>
      <td>[New Delhi, Delhi, India]</td>
      <td>[{'label': 'display', 'lat': 28.636896, 'lng':...</td>
      <td>28.636896</td>
      <td>77.141307</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Delhi</td>
      <td>5aedd85c603d2a002d9536fd</td>
    </tr>
  </tbody>
</table>
</div>




```python
dataframe_filtered.name
```




    0                      New Delhi Railway Station (NDLS)
    1                       Old Delhi Railway Station (DLI)
    2                bloomrooms @ New Delhi Railway Station
    3                 Platform No. 1, Delhi Railway Station
    4                          Sadar Bazaar Railway Station
    5         Platform Number 5 , New Delhi Railway Station
    6     The Prime Balaji Deluxe @ New Delhi Railway St...
    7               Delhi Subzi Mandi Railway Station (SZM)
    8         Sarai Rohilla | सराय रोहिल्ला Railway Station
    9               Hazrat Nizamuddin Railway Station (NZM)
    10                                      Railway Station
    11        Optimum Palm D'or @ New Delhi Railway station
    12               Platform No. 4 - Delhi Railway Station
    13                       Chawndni Chowk Railway Station
    14            New Delhi Railway Station , Platform No 2
    15            Platfrom No. 3, New Delhi Railway Station
    16                Platform No.14, Delhi Railway Station
    17    Upper Class Waiting Hall New Delhi Railway Sta...
    18              Platform No 8 New Delhi Railway station
    19          Alighting area at new delhi railway station
    20                          Delhi Cantt Railway Station
    21            Kalka Shatabdi, New delhi railway station
    22                            New Delhi Railway Station
    23                         Tilak Bridge Railway Station
    24                        Gurgaon Railway Station (GGN)
    25                            New Delhi Railway Station
    26                    Delhi Kishan Ganj Railway Station
    27                       Shivaji Bridge Railway Station
    28                     Delhi Safdarjang Railway Station
    29                Nangloi Railway Station Metro Station
    30                           Ballabgarh Railway Station
    31                            Ghaziabad Railway Station
    32              Delhi Shahdara Junction railway station
    33                            New delhi Railway station
    34                       Shivaji Bridge Railway station
    35                            Dayabasti Railway Station
    36                          Tughlakabad Railway Station
    37                          Anand Vihar Railway Station
    38                               Narela Railway Station
    39                           Mangolpuri Railway Station
    40                         Purani Dilli Railway Station
    41                       Pragati Maidan Railway Station
    42                           Sahibabaad Railway Station
    43                               Ganaur Railway Station
    44      Hazrat Nizamuddin Railway Station Platform No.4
    45                              Sonipat Railway Station
    46                            Gtb Nagar Railway Station
    47                          Kirti nagar Railway Station
    48                               Rohtak Railway Station
    49                        Naraina Vihar Railway Station
    Name: name, dtype: object



***Now we have all the required data related to nearby railway stations, let's see stations' location on map.***


```python
venues_map = folium.Map(location=[latitude, longitude], zoom_start=10) # generate map centred around the Conrad Hotel

# add a red circle marker to represent the Delhi
folium.features.CircleMarker(
    [latitude, longitude],
    radius=10,
    color='red',
    popup='Delhi',
    fill = True,
    fill_color = 'red',
    fill_opacity = 0.6
).add_to(venues_map)

# add the Railway Stations as blue circle markers
for lat, lng, label in zip(dataframe_filtered.lat, dataframe_filtered.lng, dataframe_filtered.categories):
    folium.features.CircleMarker(
        [lat, lng],
        radius=3,
        color='blue',
        label=label,
        fill = True,
        fill_color='blue',
        fill_opacity=0.6
    ).add_to(venues_map)

# display map
venues_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3IiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNycsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbMjguNjUxNzE3OCw3Ny4yMjE5Mzg4XSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHpvb206IDEwLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbWF4Qm91bmRzOiBib3VuZHMsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBsYXllcnM6IFtdLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3CiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0pOwogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl85MjdlMmU2N2ZiZDQ0MDlmYWFhNDg3Nzk4NGVmYTkzZSA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgJ2h0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nJywKICAgICAgICAgICAgICAgIHsKICAiYXR0cmlidXRpb24iOiBudWxsLAogICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAibWF4Wm9vbSI6IDE4LAogICJtaW5ab29tIjogMSwKICAibm9XcmFwIjogZmFsc2UsCiAgInN1YmRvbWFpbnMiOiAiYWJjIgp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWNmNjdjMDUwNjExNDljODg2ZTA5NDdmNWMyMmZhNjAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NTE3MTc4LDc3LjIyMTkzODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kNjVhNmYwOWM3NTk0OTU2OGU0ZjIyMTdiNTdhZTJlMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yZjE0NjZlMTkxNDI0Y2M1YmI3YTYyNDQ1M2I1MDc1ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfMmYxNDY2ZTE5MTQyNGNjNWJiN2E2MjQ0NTNiNTA3NWUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlbGhpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kNjVhNmYwOWM3NTk0OTU2OGU0ZjIyMTdiNTdhZTJlMy5zZXRDb250ZW50KGh0bWxfMmYxNDY2ZTE5MTQyNGNjNWJiN2E2MjQ0NTNiNTA3NWUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMWNmNjdjMDUwNjExNDljODg2ZTA5NDdmNWMyMmZhNjAuYmluZFBvcHVwKHBvcHVwX2Q2NWE2ZjA5Yzc1OTQ5NTY4ZTRmMjIxN2I1N2FlMmUzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZlYjdlZjBmNmQzZTQ4MDU4ODNiMmJjMGY0NDFjMmExID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQyMDI4MjE3ODk0NjM0LDc3LjIxOTYyNDcwNDc2OTU3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGM4ZTlkN2JmZDJhNGU2M2EyN2NjYTUyODJiNzBhNmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NjA1NzkxNjMzMDUyOTcsNzcuMjI4NTY1MjE2MDY0NDVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lM2FjMzFjMjM2ZjI0NGZjYWIxMWMwNTQxMzIxNDA2ZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0NTUzNjU1OTY3ODMzLDc3LjIxNzcwMDU5NTU1OTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmNjYjRlYTg2NWQ2NGExNTg1NjdhOWU1M2VhNGRhNGUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NDI3NjM4MDk3NjU5MDMsNzcuMjE5NDExMzczMzE2MzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xZTcyMjhkZjVjNTE0Mzc1ODJhNWQ2MjEzZGFkNjY2YyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY1NDIzMjIxOTE0MTEwNCw3Ny4yMTgwMTM0ODEzNzA0M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg1NjkxMmZjNWM3ODRiODc5NGRiM2U0NjE0NjkzNjIwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQxMjE5ODI3OTU3NjIsNzcuMjIwMDEyMDY4NzkyMTRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yZWNhZjkyMGFhY2U0N2JiOTAyNmEzNzRkYWY4MzhlMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0NTI0NjYzNDMzODQwMiw3Ny4yMTc0MzM0MzcyNzU5NF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ3MmFlZjI5MGU2OTQ5N2I5YzNlMzZmMjA5MWY1MDM2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjY4MTIxMTI3MTA5NTg2LDc3LjIwMDI3MTczMDQ4MjkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTU0NWRjZmQ5ZDUwNGQwZWFhYzNhODBjYjBmYzBmODYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NjMxMzA0MDc1NjM5NDQsNzcuMTg3MDIzMTYyODQxOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JiNDgxNzgxNGE4ODQ1NmI5ZTViMzMwZTA2ZWMyMjBmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNTg4NTExMDY3OTQ0MDg2LDc3LjI1NDAyNzc3NjEwNzJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kZTc0NzI0OTM2ZGU0ZWViOTZlZmM4ZWVhZGRiZjdlYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0MjUyNjEzNTU0MDk5NCw3Ny4yMjAxMzU1NzU4OTQ2N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZlZDYzNDUxNjQ1OTQzODFhNWQ1NGJmMzViYjM1ZjJhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQ0ODc0OTM0ODEzNTgsNzcuMjE3NDQwNzU0MTc1MTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85ZjYwMjFjNjMyMTk0MDliYTcyYjRlMGYzNWI2MDg1MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0Mzc5OTY3Mjk2MDQwNSw3Ny4yMTk3MDMzMDA0NzEwNF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFiZGNhNzdmYWJhZTQxYWM4NDgyNzlhY2Q0NmRlMTk1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjU3MDM4LDc3LjIzMDA0OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2EzNDIyNGYzZjU5NzRiZWY4ODQ2MDc0ODZhN2JlZDM4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQyNjQ3NTE5MDgyNTM0LDc3LjIxOTUwNjgxMTExOTM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODJhY2Y1M2QxODM1NDY5OGIwMmIxZjFmNTZlNmI4ZGIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NDI3NzEzMjQ4OTExOSw3Ny4yMTk3NTM4MjYyNTE1Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzA1NDkxNWM3NmY5OTQ0ZjA4ZDA2ZDE1ZTc5ODg2ZmVlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQxNTYwMDQ0NjExMjY0LDc3LjIyMTM3NjI1MzcxNzI0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWVjNDczZGFiNzVhNDA0ZGE1NTE0YjllMTExOTM0YTIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NDE0ODcxMDc1MzM4ODQsNzcuMjIwOTQzOTgyODMyNTFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84YTAzZGJjYTg5NTg0YWJlYmYxODU3ZGFiY2NhNWVlZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0MTM3Nzc5MzE3OTE5Miw3Ny4yMjA4MzczMTgzNDQ5NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzgxYzM2ZDcxOWE0NzQxYzVhMjliY2RhZGU5MmU0Zjc0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjQxMzk3NDE3MDEwMjI1LDc3LjIyMjAzMzA3Nzg3ODM1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzU3MGU3MGRhMDYyNGFmYWE0NTBhNzg5ZjQ0OGJmMzQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42MTEwNTY2NTY4ODE2MzMsNzcuMTE0OTIyNzA5ODAxODNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mY2I5NjlkNTBkNjk0OTllYjJmODdhYWM0MjcyZTM4OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0MDg0NjI1MjQ0MTQwNiw3Ny4yMjI2MTA0NzM2MzI4MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzlkOGY3MzJhOGM3ZTQ2MjM4MDJiYmQ3NTE3N2VmNzJiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjYyNDI5Mzg1ODQ3NTk1LDc3LjIxNjA4MjIyOTk1ODY3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDliNjRiY2Q2Yjc3NGQwYmJjOWU4Y2YwMTdhZjE2NTEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42Mjc3NzUsNzcuMjM3ODI4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDBmOTVjZDRlYWY3NDkyZjgxZDNlNmQ0OWU5YWZkZGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC40ODkzMjYxMDI0ODY5MDYsNzcuMDExMjQ3NDc3OTQ5MzNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xZDJkZjdmZWI0OTI0NjRkYWJkMzhiZGI0MDI5YTNhYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjYzMTU1MjA3ODQwNDYwNiw3Ny4yMjc2MDE5MDE3NzcyNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q4MTM1Nzk4Zjk0YTRiM2Q4ZWU2M2FkODk0ZGUzZGZiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjYzMTY0MDY3NDQxNjY3LDc3LjE5OTMzOTY1MzU5NjY1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGU4ZTQ5OTliOWQ1NDdiY2FhMzE1N2Q1MzJhZWJhZmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42MzM1NTY2NDA3NzY1OTcsNzcuMjI2MDg2ODU0OTM0NjldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wYmNmMWE0ZWYyNzc0NzVkOGE4YjgxNTJkYzE2NWUwMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjU4MjM5MjUyNDEzMzUsNzcuMTg2MjA3NzcxMzAxMjddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80YzgzZDIzZDZiOTM0MzJkOTVhY2JhN2E1M2FmNWZiZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY4MjE3MzM0MjQ5OTExMiw3Ny4wNTYyMTcxOTM2MDM1Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZmMzliYjhlM2ZiNzRlZWViNDEzN2ZkNWE0NmFjYzhjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguMzQyODU3MDIxMDQ2MDgsNzcuMzEyMzk4NDY1MDIwMjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mZjQ5NWZlNWIxMmE0NTY2OWVlYWU3YmUyNWMwNzk4ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY1MzE1NzI0ODg1OTUyNSw3Ny40Mjc5NjUwNTI5MzQ3N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JjMGZlZGRjZjA3MzRhMzdiNmMxMmI1ODlkZjM5OWQ0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjcyODgyOTMxMjA1NTY3LDc3LjI5MjUxOTgwNzgxNTU1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZWY1YzFmNjI2MmY5NGYyZmI1MGMwYTg5OWVlMWRhNDkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42Mjc5NzE5NjE2NTE1NjQsNzcuMjIxNzk3Mjk1MjY2MzFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMWFkNDQxZjkyNTA0MGJmODBlN2FkNTczOGQzYmU5YiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjYyNjU0LDc3LjIyODMwODddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85OWNiNDg4MDAwZDg0ZDliOTQ2ZmNiZWIwZmI4N2EzNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY2NjkwNTM4MjAwNDY0LDc3LjE3MTYxNjU1NDI2MDI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmIzNDZkMTRjZmM5NGY3NDkyMWU1NTAxY2E5Nzg4ZjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC41MDQ3ODg4MzU4NzM0NDUsNzcuMjk2MjQyNzEzOTI4MjJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xNzZiYzQxMWYwMzE0YzRiYjkyN2Y5MGVhZjZmMmM5NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY0ODczNjQxMjc0MTk2LDc3LjMxNTY4MDAwNTQzODZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lNjA1OTM3YmE0ZjU0ZDk0YWQzODllYmFlNjYxMjUxZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4Ljg1ODIwOTQ5NTgwMzQ2OCw3Ny4xMjQxODIzMzkyNDIyOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2MxMDk1YjA5NWI5MzRkOWJiMzI2OWY0ZmY1NGFlNTE1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjgzNjAyMjI0OTMzMzc3LDc3LjA5MTQ2NzAyMzM5OTM2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZGRjMmZjNjdjYWU4NGU0ZTlhMTRjMTc2M2EwMTU2ZTAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42MDI4MjUxNjQ3OTQ5MjIsNzcuMjQ5OTY5NDgyNDIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lYjVmNDgyYzUyYTE0MjZiYTAxYjk4ZTRjYzgzN2U3MCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjYxNTQ1Niw3Ny4yNDcyMjZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84OTgwYjcwZDNjYmE0OTc0YjJlY2Y4OWVmYjRhZTdmMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjY3MzkyMDM5Nzc4ODQwNyw3Ny4zNjQ5ODczNDgyNDYwN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZlZTUzYzg1ODllOTQwMDZhZjU4MTUyNGI5NzgwOTM3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjkuMTMyMjgwODUwNjg3MzUsNzcuMDExNTkxMjUwNzc1NjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wZGQ0YzY1NjczMDI0ZGQxYWI0OGJhM2IwYjEyZjQ3MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjU4OTI1MjM3NjE1MTY2Niw3Ny4yNTQxMzQ0MTA1NzQ0N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk1ODBkZGNkYzA2ZDQyN2RiOTc3NjdjNWY4NGI4NjI1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguOTg5NDU1NjcwNTI2NDI1LDc3LjAxNzYwMjg4NTE0M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF83NjY5YjU0NTg0ODE0NTNkOGNjOWU2NTljZjQ4YzkzNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzkyZDkyZGM1ZTc1OTQzMGI5NWZhZWQwMGQ1ZDEyNzI0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNzA3MTgzMTIxMjM4OTU1LDc3LjIwNjc1Njc2MTE1OTkyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjAwOTFjZThhMTc5NGU4YmIwY2QyNmVjMmI2MGM4YmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NDY4ODA0NjE4ODYyMSw3Ny4xNDU1NDU0ODI2MzU1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzc2NjliNTQ1ODQ4MTQ1M2Q4Y2M5ZTY1OWNmNDhjOTM3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWQ4YzY1YWFlZGU0NDFkYWIzOGJjMTZkYTEzNjQ5OTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC44OTAyMzcyMjczNTU2MjcsNzYuNTgxMjMzMjc1MDY5MTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xN2M5Mzc2MzAzYWY0YTgzYWJhYWZlMmI2N2ExMzJlNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjYzNjg5Niw3Ny4xNDEzMDddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNzY2OWI1NDU4NDgxNDUzZDhjYzllNjU5Y2Y0OGM5MzcpOwogICAgICAgICAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



In above plot, we can clearly see that there's no railway station in Noida, while there are many railway stations in Delhi. Thus, if someone usually travels a lot will prefer to live in Delhi over Noida. I someone come here from another city to buy something or some services, or for medical purposes, etc, by train, he/she will have to pass through Delhi, even if he want to go to Noida. If he has to go through Delhi and service or product he is looking for is available in Delhi, then why will he go to Noida instead of Delhi?

**SOLUTION:** There must be a railway station in Noida, so that people do not need to enter Delhi, if Noida, with a railway station, is more convenient to them.

**Education** is another important factor for which people move across the states, and spend much time in another state- sometimes settle. Let's explore developed Delhi is in education sector as compare to surrounding. Let's see how many institions of national importance are there in Delhi and its surrounding.


```python
address = 'Delhi, India'

geolocator = Nominatim(user_agent="foursquare_agent")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude
print(latitude, longitude)
```

    28.6517178 77.2219388



```python

search_query = 'Institute of National Importance'
radius = 50000

url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

results = requests.get(url).json()


# assign relevant part of JSON to venues
venues = results['response']['venues']

# tranform venues into a dataframe
dataframe = json_normalize(venues)


# keep only columns that include venue name, and anything that is associated with location
filtered_columns = ['name', 'categories'] + [col for col in dataframe.columns if col.startswith('location.')] + ['id']
dataframe_filtered = dataframe.loc[:, filtered_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']
        
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']

# filter the category for each row
dataframe_filtered['categories'] = dataframe_filtered.apply(get_category_type, axis=1)

# clean column names by keeping only last term
dataframe_filtered.columns = [column.split('.')[-1] for column in dataframe_filtered.columns]


dataframe_filtered.name
```




    0              Morarji Desai National Institute Of Yoga
    1     The National Institute Of Entrepreneurship & S...
    2     NISTADS (National Institute Of Science Technol...
    3     National Institute Of Criminology And Forensic...
    4              National institute of fashion technology
    5                National Institute of Business Studies
    6              National Institute Of Fashion Technology
    7         National institute of health & family welfare
    8                      National institute of immunology
    9     National Institute Of Plant Genome Research (N...
    10               National institute of business studies
    11    NISCAIR (National Institute of Science Communi...
    12                     National Institute Of Immunology
    13      National Institute of Public Finance and Policy
    14        National Institute of Business Studies (NIBS)
    15    NIFTEM National Institute Of Food Technology E...
    Name: name, dtype: object



*Now, generate the map.*


```python
venues_map = folium.Map(location=[latitude, longitude], zoom_start=10) # generate map centred around the Conrad Hotel

# add a red circle marker to represent the Delhi
folium.features.CircleMarker(
    [latitude, longitude],
    radius=10,
    color='red',
    popup='Delhi',
    fill = True,
    fill_color = 'red',
    fill_opacity = 0.6
).add_to(venues_map)

# add the Railway Stations as blue circle markers
for lat, lng, label in zip(dataframe_filtered.lat, dataframe_filtered.lng, dataframe_filtered.categories):
    folium.features.CircleMarker(
        [lat, lng],
        radius=3,
        color='blue',
        label=label,
        fill = True,
        fill_color='blue',
        fill_opacity=0.6
    ).add_to(venues_map)

# display map
venues_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfNDdjNmZmMmFkMzQ4NDA3ODgzMjk0MzEzOGM4NTBkZjcgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3IiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNycsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbMjguNjUxNzE3OCw3Ny4yMjE5Mzg4XSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHpvb206IDEwLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbWF4Qm91bmRzOiBib3VuZHMsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBsYXllcnM6IFtdLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3CiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0pOwogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl82Yjc4MGE3ZGI1MTA0YjRkOWMwODY4ZjU3ODIwMWQwMCA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgJ2h0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nJywKICAgICAgICAgICAgICAgIHsKICAiYXR0cmlidXRpb24iOiBudWxsLAogICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAibWF4Wm9vbSI6IDE4LAogICJtaW5ab29tIjogMSwKICAibm9XcmFwIjogZmFsc2UsCiAgInN1YmRvbWFpbnMiOiAiYWJjIgp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjIxNDM4N2Q0MzFhNGFkNWJlZThhOTQzMzg1NTRjNjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42NTE3MTc4LDc3LjIyMTkzODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAxMCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84Yzc3MTBhMzg0Njk0NWEzOWIzMDA4N2NiNzAxYTIyZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jNzFiNDgyY2E0MjY0MjJhODM3OWYwMDdmOTNlNzUwOCA9ICQoJzxkaXYgaWQ9Imh0bWxfYzcxYjQ4MmNhNDI2NDIyYTgzNzlmMDA3ZjkzZTc1MDgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlbGhpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84Yzc3MTBhMzg0Njk0NWEzOWIzMDA4N2NiNzAxYTIyZC5zZXRDb250ZW50KGh0bWxfYzcxYjQ4MmNhNDI2NDIyYTgzNzlmMDA3ZjkzZTc1MDgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZjIxNDM4N2Q0MzFhNGFkNWJlZThhOTQzMzg1NTRjNjYuYmluZFBvcHVwKHBvcHVwXzhjNzcxMGEzODQ2OTQ1YTM5YjMwMDg3Y2I3MDFhMjJkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzBmOTUxYzA1ZTYzYTRjOTJiZGIxMWQ3OGEyMzQ0MWVjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjI2MDUwMTM3MDg4NjgzLDc3LjIxNTM0MTE1OTk5NDU1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfM2Q3ZGEzZGFlNmY2NDdjZmJjOTAxNmE3ZWM3NWIzNGMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42Mjg2OTgzOTkyMzQzNzcsNzcuMzYyMjExODQxNjczNzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDdjNmZmMmFkMzQ4NDA3ODgzMjk0MzEzOGM4NTBkZjcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hMDlhYTZmYzRmYzU0ZDY4OTNiOGQyNTI3ZTFhODIyNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjYzMTM2MTg1ODEzMDE4Nyw3Ny4xNzUxNjQ4NTk2ODczOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU4MDJlYTI1ZDE3MzQ2ZmFhYzRiZTRiMmM5ODgxYjY2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjk2NDY4LDc3LjEwOTg0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTQ1NzQxYmM5MzZjNDIwZjhiMzJiYmJhNTBiMjhkMmMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC41NTYxMDA3ODA0NzkyMyw3Ny4yMDk0Nzk4MDY2NjI2N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RhNDkxMGU0OTY4MzQyYzliZWFiNDdlYmVkZDFhNjcwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNjc3Njc0MTM3Nzk3NDY3LDc3LjEwODcyNDExNzI3OTA1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmQ1OTRjZjQ0ODZkNDk1NmExNWZlYWFhZGVlZWQyYmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC41NTYwNDQ1MTc1NTk2OSw3Ny4yMDgxMjEwOTkwMDY4OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzE4OGE1ZDNkYTA2NDQyZTRiNzE1YmNmM2EwNzBlYTEzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNTU0MDUwOTI5MjA0MDc0LDc3LjE3NTczMjA4MzE0NzQ3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjBjZWY4ZDNmZDQzNDcxMGE0Njk2ODM0YmU2ZTI4ZDMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC41NTM4ODYwMjE2NzEzNTcsNzcuMTg1NjE1OTM4Mjc3ODZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDdjNmZmMmFkMzQ4NDA3ODgzMjk0MzEzOGM4NTBkZjcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84OWY1NGY4ZDRkMjM0ZGJhYWU0ZWQyZGQ2NmY2ZDlmNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjU1MzQyMjAyMjQzMDk4LDc3LjE3MjcyNzQ1MjgxNzc3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTk3MTM3M2VjNjNkNGU4NDhlNTMzZjk4OTczNzExNjEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42Nzg1ODg5MjA1NTk4Miw3Ny4xMDI0MzY4MDg3MTQyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWFhYWVmMTllMmZlNDVkMzgzNWEyMWJlODE2ZjAxYzggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC41NDIzODgwMjExOTQ0MSw3Ny4xODAwOTAwNjYxMjZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDdjNmZmMmFkMzQ4NDA3ODgzMjk0MzEzOGM4NTBkZjcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xOGE2MzJmYjRjY2I0N2Q1YmUwNmM1MjBhYWY3YzAyZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzI4LjU0MTU3LDc3LjE3NDYxNF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzMzM2FlZDMxOTk3NjRiNTRiZGRhMmE2NTMzMjhlZDY1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguNTQyMzUxMTUyMDM4MTQ3LDc3LjE3NjkxMTQ0OTk5MDA2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICJibHVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjYsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiAzLAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ3YzZmZjJhZDM0ODQwNzg4MzI5NDMxMzhjODUwZGY3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjZjNTU0MmI1NjdhNGU3Mjk3YjBhY2NkNGQ4ZDViZTQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFsyOC42Mjc2MjM5MzU3ODg5Nyw3Ny4wODI2MTAxMzAzMTAwNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiYmx1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC42LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogMywKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80N2M2ZmYyYWQzNDg0MDc4ODMyOTQzMTM4Yzg1MGRmNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFjMWFhYzdjYmRhYzQwMjg4MmNkZmY2NTY2YjY0ZjQ4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbMjguODc4MDAyNDM0ODc1MzA2LDc3LjEyNDgyMjg0NTM1NzJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogImJsdWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDMsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDdjNmZmMmFkMzQ4NDA3ODgzMjk0MzEzOGM4NTBkZjcpOwogICAgICAgICAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



Above plot displays that some of the best institutions of country are densely situated in Delhi, while few in surrounding. Even though NCR region is known for quantity of private institutions, still lack of best quality institutions as compared to Delhi make these places much less attractive than Delhi.

**SOLUTION:** With quantity, NCR and nearby cities also need some best quality education bodies to attract people as much as Delhi do.



Delhi is known for its medical & health care facilities. Delhi has many prominent bodies that provide medical services and education. This is another attraction for people from all over the country. Let's explore how, neighboring cities are doing in this field with respect to Delhi; do they also hold same potential to attract public?

*Start with importing data that contain most prominent hospitals of Delhi and its neighboring cities.*


```python
Hospitals = pd.read_csv("Hospitals NCR.csv")
Hospitals.head()
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
      <th>Hospitals</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>All India Institute of Medical Sciences</td>
      <td>Delhi</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dr. Mohan’s Diabetes Specialities Centre</td>
      <td>Delhi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dharamshila Cancer Hospital and Research Centre</td>
      <td>Delhi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Eden Hospital, East of Kailash</td>
      <td>Delhi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Guru Teg Bahadur Hospital</td>
      <td>Delhi</td>
    </tr>
  </tbody>
</table>
</div>



*To compare the number of these most prominent hospitals, plot a bar chart of location vs count of hospitals in these locations.*


```python
fig, ax = plt.subplots()
Hospitals['Location'].value_counts().plot(ax=ax, kind='bar')

#Label the y-axis and give title.
plt.ylabel('Number of Prominent Hospitals')
plt.title('Number of Prominent Hospitals in Delhi and neighboring cities')
```




    Text(0.5, 1.0, 'Number of Prominent Hospitals in Delhi and neighboring cities')




![png](output_30_1.png)


Graph shows, neighboring cities are incapable of attracting public, to their hospitals, as much as Delhi do.

**SOLUTION:** All the neighboring cities of Delhi need more quantity of prominent hospitals, to divert public from Delhi to these cities.


Now, the biggest issue is that Delhi provides magnificently higher count of employment, that most of the country. Thus, people live in Delhi, because their career is in Delhi. Let's see, how much is the difference in count of companies in Delhi and that of in it's neighboring states.


*Import the data that have count of active companies as of on 31st January 2016*


```python
#Import Data
Companies = pd.read_csv("No. of Companies.csv")
Companies.head()
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
      <th>State</th>
      <th>Active Companies as on January 31, 2016</th>
      <th>Area of State(Squared KM)</th>
      <th>Company per KM-Sq</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Delhi</td>
      <td>204429</td>
      <td>1483</td>
      <td>137.848300</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Haryana</td>
      <td>23642</td>
      <td>44212</td>
      <td>0.534742</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Uttar Pradesh</td>
      <td>53634</td>
      <td>243290</td>
      <td>0.220453</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rajasthan</td>
      <td>35789</td>
      <td>342238</td>
      <td>0.104573</td>
    </tr>
  </tbody>
</table>
</div>



*Plot Bar chart for Companies Counts of Delhi and Neighboring States*


```python
#Plot Bar chart for Companies Counts of Delhi and Neighboring States
y_pos = np.arange(len(Companies["State"]))
performance = Companies["Active Companies as on January 31, 2016"]

plt.barh(y_pos, performance, align='center', alpha=0.5)
plt.yticks(y_pos, Companies["State"])
plt.xlabel('Number of Companies')
plt.title('Companies Counts of Delhi and Neighboring States')
```




    Text(0.5, 1.0, 'Companies Counts of Delhi and Neighboring States')




![png](output_34_1.png)


Plot tells, there is a huge difference in the no. of companies in Delhi and that of in any of the neighboring state. But wait, Delhi is much smaller in area than any of these neighboring state. to get the true idea of the situation, let's plot average density of companies in these states.


```python
#Plot Bar chart for Companies per Square kilometers in Delhi and Neighboring States
y_pos = np.arange(len(Companies["State"]))
performance = Companies["Company per KM-Sq"]

plt.barh(y_pos, performance, align='center', alpha=0.5)
plt.yticks(y_pos, Companies["State"])
plt.xlabel('Companies per Square kilometers')
plt.title('Companies per Square kilometers in Delhi and Neighboring States')
```




    Text(0.5, 1.0, 'Companies per Square kilometers in Delhi and Neighboring States')




![png](output_36_1.png)


BOOM !!!

Companies' densities are almost negligible in these neighboring states when compared to companies density of Delhi.

**SOLUTION:** Policies should be made to attract new companies and old well settled companies to build their offices and provide employment in different cities of these neighboring states.



### **Let's Conclude**

**Problem 1:** No Railway Station in Noida.

    - **SOLUTION:** There must be a railway station in Noida, so that people do not need to enter Delhi, if Noida, with a railway station, is more convenient to them.
     
     
**Problem 2:** Denser quality Education Bodies in Delhi as compared to surroundings. 
     
    - **SOLUTION:** With quantity, NCR and nearby cities also need some best quality education bodies to attract people as much as Delhi do.
     
     
**Problem 3:** Denser prominent Medical Services Bodies in Delhi as compared to surroundings. 
    
    - **SOLUTION:** All the neighboring cities of Delhi need more quantity of prominent hospitals, to divert public from Delhi to these cities.
     
     
**Problem 4:** Employers are concentrated at Delhi.

    - **SOLUTION:** Policies should be made to attract new companies and old well settled companies to build their offices and provide employment in different cities of these neighboring states.






______________________
*Thank You*

*Uday Pratap Singh*

*udaychauhan94@gmail.com*

*8218870810*




_______



#### **Data Sources**

   - https://foursquare.com/
   - https://data.gov.in/
   - https://en.wikipedia.org/wiki/List_of_hospitals_in_India
   - https://propstory.com/top-5-hospitals-in-noida/
   - www.mca.gov.in/.../Monthly_Information_Bulletin_CorporateSector_Jan_2016.pdf




```python

```
