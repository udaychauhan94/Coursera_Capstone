
Import Libraries


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

print('Libraries imported.')
```

    Collecting package metadata: done
    Solving environment: - 
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
    done
    
    ## Package Plan ##
    
      environment location: /home/jupyterlab/conda
    
      added / updated specs:
        - folium=0.5.0
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        ca-certificates-2019.6.16  |       hecc5488_0         145 KB  conda-forge
        certifi-2019.6.16          |           py36_0         148 KB  conda-forge
        conda-4.7.5                |           py36_0         3.0 MB  conda-forge
        conda-package-handling-1.3.10|           py36_0         257 KB  conda-forge
        ------------------------------------------------------------
                                               Total:         3.5 MB
    
    The following NEW packages will be INSTALLED:
    
      conda-package-han~ conda-forge/linux-64::conda-package-handling-1.3.10-py36_0
    
    The following packages will be UPDATED:
    
      ca-certificates                       2019.3.9-hecc5488_0 --> 2019.6.16-hecc5488_0
      certifi                                   2019.3.9-py36_0 --> 2019.6.16-py36_0
      conda                                       4.6.14-py36_0 --> 4.7.5-py36_0
    
    
    
    Downloading and Extracting Packages
    conda-4.7.5          | 3.0 MB    | ##################################### | 100% 
    conda-package-handli | 257 KB    | ##################################### | 100% 
    certifi-2019.6.16    | 148 KB    | ##################################### | 100% 
    ca-certificates-2019 | 145 KB    | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    Libraries imported.


Download Data


```python
url = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M').text
data = BeautifulSoup(url,'lxml')

print("Data downloaded!")

```

    Data downloaded!


Read data as Data Frame


```python
table = data.find('tbody')
rows = table.select('tr')
row = [r.get_text() for r in rows]
```


```python
df = pd.DataFrame(row)
df1 = df[0].str.split('\n', expand=True)
df2 = df1.rename(columns=df1.iloc[0])
df3 = df2.drop(df2.index[0])
df3.head()

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
      <th></th>
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td></td>
      <td>M1A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td></td>
      <td>M2A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td></td>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td></td>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td></td>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df3[df3.Borough != 'Not assigned']
df.head()

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
      <th></th>
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td></td>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td></td>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td></td>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront</td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td></td>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park</td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td></td>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



Drop all "Not Assigned" Borough rows.


```python

```

     


Arrange data in group and reset the index.


```python
df = df.groupby(['Postcode', 'Borough'], sort = False).agg(','.join)
df.reset_index(inplace = True)
df.head()
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront,Regent Park</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights,Lawrence Manor</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Not assigned</td>
    </tr>
  </tbody>
</table>
</div>



Replace "Not Assigned" in Neighbourhood.


```python
df2 = df.replace("Not assigned", "Queen's Park")
df2
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront,Regent Park</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights,Lawrence Manor</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Queen's Park</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M9A</td>
      <td>Etobicoke</td>
      <td>Islington Avenue</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Rouge,Malvern</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M3B</td>
      <td>North York</td>
      <td>Don Mills North</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M4B</td>
      <td>East York</td>
      <td>Woodbine Gardens,Parkview Hill</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Ryerson,Garden District</td>
    </tr>
    <tr>
      <th>10</th>
      <td>M6B</td>
      <td>North York</td>
      <td>Glencairn</td>
    </tr>
    <tr>
      <th>11</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>Cloverdale,Islington,Martin Grove,Princess Gar...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Highland Creek,Rouge Hill,Port Union</td>
    </tr>
    <tr>
      <th>13</th>
      <td>M3C</td>
      <td>North York</td>
      <td>Flemingdon Park,Don Mills South</td>
    </tr>
    <tr>
      <th>14</th>
      <td>M4C</td>
      <td>East York</td>
      <td>Woodbine Heights</td>
    </tr>
    <tr>
      <th>15</th>
      <td>M5C</td>
      <td>Downtown Toronto</td>
      <td>St. James Town</td>
    </tr>
    <tr>
      <th>16</th>
      <td>M6C</td>
      <td>York</td>
      <td>Humewood-Cedarvale</td>
    </tr>
    <tr>
      <th>17</th>
      <td>M9C</td>
      <td>Etobicoke</td>
      <td>Bloordale Gardens,Eringate,Markland Wood,Old B...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td>Guildwood,Morningside,West Hill</td>
    </tr>
    <tr>
      <th>19</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
    </tr>
    <tr>
      <th>20</th>
      <td>M5E</td>
      <td>Downtown Toronto</td>
      <td>Berczy Park</td>
    </tr>
    <tr>
      <th>21</th>
      <td>M6E</td>
      <td>York</td>
      <td>Caledonia-Fairbanks</td>
    </tr>
    <tr>
      <th>22</th>
      <td>M1G</td>
      <td>Scarborough</td>
      <td>Woburn</td>
    </tr>
    <tr>
      <th>23</th>
      <td>M4G</td>
      <td>East York</td>
      <td>Leaside</td>
    </tr>
    <tr>
      <th>24</th>
      <td>M5G</td>
      <td>Downtown Toronto</td>
      <td>Central Bay Street</td>
    </tr>
    <tr>
      <th>25</th>
      <td>M6G</td>
      <td>Downtown Toronto</td>
      <td>Christie</td>
    </tr>
    <tr>
      <th>26</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td>Cedarbrae</td>
    </tr>
    <tr>
      <th>27</th>
      <td>M2H</td>
      <td>North York</td>
      <td>Hillcrest Village</td>
    </tr>
    <tr>
      <th>28</th>
      <td>M3H</td>
      <td>North York</td>
      <td>Bathurst Manor,Downsview North,Wilson Heights</td>
    </tr>
    <tr>
      <th>29</th>
      <td>M4H</td>
      <td>East York</td>
      <td>Thorncliffe Park</td>
    </tr>
    <tr>
      <th>30</th>
      <td>M5H</td>
      <td>Downtown Toronto</td>
      <td>Adelaide,King,Richmond</td>
    </tr>
    <tr>
      <th>31</th>
      <td>M6H</td>
      <td>West Toronto</td>
      <td>Dovercourt Village,Dufferin</td>
    </tr>
    <tr>
      <th>32</th>
      <td>M1J</td>
      <td>Scarborough</td>
      <td>Scarborough Village</td>
    </tr>
    <tr>
      <th>33</th>
      <td>M2J</td>
      <td>North York</td>
      <td>Fairview,Henry Farm,Oriole</td>
    </tr>
    <tr>
      <th>34</th>
      <td>M3J</td>
      <td>North York</td>
      <td>Northwood Park,York University</td>
    </tr>
    <tr>
      <th>35</th>
      <td>M4J</td>
      <td>East York</td>
      <td>East Toronto</td>
    </tr>
    <tr>
      <th>36</th>
      <td>M5J</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront East,Toronto Islands,Union Station</td>
    </tr>
    <tr>
      <th>37</th>
      <td>M6J</td>
      <td>West Toronto</td>
      <td>Little Portugal,Trinity</td>
    </tr>
    <tr>
      <th>38</th>
      <td>M1K</td>
      <td>Scarborough</td>
      <td>East Birchmount Park,Ionview,Kennedy Park</td>
    </tr>
    <tr>
      <th>39</th>
      <td>M2K</td>
      <td>North York</td>
      <td>Bayview Village</td>
    </tr>
    <tr>
      <th>40</th>
      <td>M3K</td>
      <td>North York</td>
      <td>CFB Toronto,Downsview East</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West,Riverdale</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M5K</td>
      <td>Downtown Toronto</td>
      <td>Design Exchange,Toronto Dominion Centre</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M6K</td>
      <td>West Toronto</td>
      <td>Brockton,Exhibition Place,Parkdale Village</td>
    </tr>
    <tr>
      <th>44</th>
      <td>M1L</td>
      <td>Scarborough</td>
      <td>Clairlea,Golden Mile,Oakridge</td>
    </tr>
    <tr>
      <th>45</th>
      <td>M2L</td>
      <td>North York</td>
      <td>Silver Hills,York Mills</td>
    </tr>
    <tr>
      <th>46</th>
      <td>M3L</td>
      <td>North York</td>
      <td>Downsview West</td>
    </tr>
    <tr>
      <th>47</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>The Beaches West,India Bazaar</td>
    </tr>
    <tr>
      <th>48</th>
      <td>M5L</td>
      <td>Downtown Toronto</td>
      <td>Commerce Court,Victoria Hotel</td>
    </tr>
    <tr>
      <th>49</th>
      <td>M6L</td>
      <td>North York</td>
      <td>Downsview,North Park,Upwood Park</td>
    </tr>
    <tr>
      <th>50</th>
      <td>M9L</td>
      <td>North York</td>
      <td>Humber Summit</td>
    </tr>
    <tr>
      <th>51</th>
      <td>M1M</td>
      <td>Scarborough</td>
      <td>Cliffcrest,Cliffside,Scarborough Village West</td>
    </tr>
    <tr>
      <th>52</th>
      <td>M2M</td>
      <td>North York</td>
      <td>Newtonbrook,Willowdale</td>
    </tr>
    <tr>
      <th>53</th>
      <td>M3M</td>
      <td>North York</td>
      <td>Downsview Central</td>
    </tr>
    <tr>
      <th>54</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
    </tr>
    <tr>
      <th>55</th>
      <td>M5M</td>
      <td>North York</td>
      <td>Bedford Park,Lawrence Manor East</td>
    </tr>
    <tr>
      <th>56</th>
      <td>M6M</td>
      <td>York</td>
      <td>Del Ray,Keelesdale,Mount Dennis,Silverthorn</td>
    </tr>
    <tr>
      <th>57</th>
      <td>M9M</td>
      <td>North York</td>
      <td>Emery,Humberlea</td>
    </tr>
    <tr>
      <th>58</th>
      <td>M1N</td>
      <td>Scarborough</td>
      <td>Birch Cliff,Cliffside West</td>
    </tr>
    <tr>
      <th>59</th>
      <td>M2N</td>
      <td>North York</td>
      <td>Willowdale South</td>
    </tr>
    <tr>
      <th>60</th>
      <td>M3N</td>
      <td>North York</td>
      <td>Downsview Northwest</td>
    </tr>
    <tr>
      <th>61</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td>Lawrence Park</td>
    </tr>
    <tr>
      <th>62</th>
      <td>M5N</td>
      <td>Central Toronto</td>
      <td>Roselawn</td>
    </tr>
    <tr>
      <th>63</th>
      <td>M6N</td>
      <td>York</td>
      <td>The Junction North,Runnymede</td>
    </tr>
    <tr>
      <th>64</th>
      <td>M9N</td>
      <td>York</td>
      <td>Weston</td>
    </tr>
    <tr>
      <th>65</th>
      <td>M1P</td>
      <td>Scarborough</td>
      <td>Dorset Park,Scarborough Town Centre,Wexford He...</td>
    </tr>
    <tr>
      <th>66</th>
      <td>M2P</td>
      <td>North York</td>
      <td>York Mills West</td>
    </tr>
    <tr>
      <th>67</th>
      <td>M4P</td>
      <td>Central Toronto</td>
      <td>Davisville North</td>
    </tr>
    <tr>
      <th>68</th>
      <td>M5P</td>
      <td>Central Toronto</td>
      <td>Forest Hill North,Forest Hill West</td>
    </tr>
    <tr>
      <th>69</th>
      <td>M6P</td>
      <td>West Toronto</td>
      <td>High Park,The Junction South</td>
    </tr>
    <tr>
      <th>70</th>
      <td>M9P</td>
      <td>Etobicoke</td>
      <td>Westmount</td>
    </tr>
    <tr>
      <th>71</th>
      <td>M1R</td>
      <td>Scarborough</td>
      <td>Maryvale,Wexford</td>
    </tr>
    <tr>
      <th>72</th>
      <td>M2R</td>
      <td>North York</td>
      <td>Willowdale West</td>
    </tr>
    <tr>
      <th>73</th>
      <td>M4R</td>
      <td>Central Toronto</td>
      <td>North Toronto West</td>
    </tr>
    <tr>
      <th>74</th>
      <td>M5R</td>
      <td>Central Toronto</td>
      <td>The Annex,North Midtown,Yorkville</td>
    </tr>
    <tr>
      <th>75</th>
      <td>M6R</td>
      <td>West Toronto</td>
      <td>Parkdale,Roncesvalles</td>
    </tr>
    <tr>
      <th>76</th>
      <td>M7R</td>
      <td>Mississauga</td>
      <td>Canada Post Gateway Processing Centre</td>
    </tr>
    <tr>
      <th>77</th>
      <td>M9R</td>
      <td>Etobicoke</td>
      <td>Kingsview Village,Martin Grove Gardens,Richvie...</td>
    </tr>
    <tr>
      <th>78</th>
      <td>M1S</td>
      <td>Scarborough</td>
      <td>Agincourt</td>
    </tr>
    <tr>
      <th>79</th>
      <td>M4S</td>
      <td>Central Toronto</td>
      <td>Davisville</td>
    </tr>
    <tr>
      <th>80</th>
      <td>M5S</td>
      <td>Downtown Toronto</td>
      <td>Harbord,University of Toronto</td>
    </tr>
    <tr>
      <th>81</th>
      <td>M6S</td>
      <td>West Toronto</td>
      <td>Runnymede,Swansea</td>
    </tr>
    <tr>
      <th>82</th>
      <td>M1T</td>
      <td>Scarborough</td>
      <td>Clarks Corners,Sullivan,Tam O'Shanter</td>
    </tr>
    <tr>
      <th>83</th>
      <td>M4T</td>
      <td>Central Toronto</td>
      <td>Moore Park,Summerhill East</td>
    </tr>
    <tr>
      <th>84</th>
      <td>M5T</td>
      <td>Downtown Toronto</td>
      <td>Chinatown,Grange Park,Kensington Market</td>
    </tr>
    <tr>
      <th>85</th>
      <td>M1V</td>
      <td>Scarborough</td>
      <td>Agincourt North,L'Amoreaux East,Milliken,Steel...</td>
    </tr>
    <tr>
      <th>86</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>Deer Park,Forest Hill SE,Rathnelly,South Hill,...</td>
    </tr>
    <tr>
      <th>87</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>CN Tower,Bathurst Quay,Island airport,Harbourf...</td>
    </tr>
    <tr>
      <th>88</th>
      <td>M8V</td>
      <td>Etobicoke</td>
      <td>Humber Bay Shores,Mimico South,New Toronto</td>
    </tr>
    <tr>
      <th>89</th>
      <td>M9V</td>
      <td>Etobicoke</td>
      <td>Albion Gardens,Beaumond Heights,Humbergate,Jam...</td>
    </tr>
    <tr>
      <th>90</th>
      <td>M1W</td>
      <td>Scarborough</td>
      <td>L'Amoreaux West</td>
    </tr>
    <tr>
      <th>91</th>
      <td>M4W</td>
      <td>Downtown Toronto</td>
      <td>Rosedale</td>
    </tr>
    <tr>
      <th>92</th>
      <td>M5W</td>
      <td>Downtown Toronto</td>
      <td>Stn A PO Boxes 25 The Esplanade</td>
    </tr>
    <tr>
      <th>93</th>
      <td>M8W</td>
      <td>Etobicoke</td>
      <td>Alderwood,Long Branch</td>
    </tr>
    <tr>
      <th>94</th>
      <td>M9W</td>
      <td>Etobicoke</td>
      <td>Northwest</td>
    </tr>
    <tr>
      <th>95</th>
      <td>M1X</td>
      <td>Scarborough</td>
      <td>Upper Rouge</td>
    </tr>
    <tr>
      <th>96</th>
      <td>M4X</td>
      <td>Downtown Toronto</td>
      <td>Cabbagetown,St. James Town</td>
    </tr>
    <tr>
      <th>97</th>
      <td>M5X</td>
      <td>Downtown Toronto</td>
      <td>First Canadian Place,Underground city</td>
    </tr>
    <tr>
      <th>98</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>The Kingsway,Montgomery Road,Old Mill North</td>
    </tr>
    <tr>
      <th>99</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
    </tr>
    <tr>
      <th>100</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business Reply Mail Processing Centre 969 Eastern</td>
    </tr>
    <tr>
      <th>101</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Humber Bay,King's Mill Park,Kingsway Park Sout...</td>
    </tr>
    <tr>
      <th>102</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Kingsway Park South West,Mimico NW,The Queensw...</td>
    </tr>
  </tbody>
</table>
</div>



Print number of rows of the dataframe using .shape method


```python
df2.shape
```




    (103, 3)




```python

```
