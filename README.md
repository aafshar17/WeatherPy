

```python
# Project Description

# Project WeatherPy is a learning project on how to use the Python requests library,citipy library, and openweathermapy module in conjuction with Pandas and Matplotlib.  

# The purpose of the project is to create a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. Build a series of
# scatter plots to show the relationship between latitude and a few other city variables.
```


```python
#Directory:

# Data/city_weather_data.csv

# Images/City Latitude vs. Cloudiness (03.16.18).png

# Images/City Latitude vs. Humidity (03.16.18).png

# Images/City Latitude vs. Max Temperature (03.16.18).png

# Images/ City Latitude vs. Wind Speed (03.16.18).png 

# README.md

# WeatherPy.ipynb

```


```python
#Observations

# 1. As expected, the max temperature of cities increases as their latitude gets closer to zero/closer to the equator and decreases as
# their distance increases from the equator/latitude of zero.

# 2. Cities that are closer to the equator or have a latitude close to zero have lower wind speeds than those that are farther away from
# the equator.

# 3. No observable trend in cloudiness percentage when it comes to latitude of cities relative to their distance from the equator.
```


```python
!pip install citipy
!pip install openweathermapy
```

    Requirement already satisfied: citipy in c:\users\aafsh\anaconda3\lib\site-packages
    Requirement already satisfied: kdtree>=0.12 in c:\users\aafsh\anaconda3\lib\site-packages (from citipy)
    Requirement already satisfied: openweathermapy in c:\users\aafsh\anaconda3\lib\site-packages
    


```python
# import dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import requests as req
import json
from citipy import citipy
```

# Generate the city list


```python
# randomly generate our lat and long values
lats = np.random.uniform(low=-90, high=90, size=1500)
lngs = np.random.uniform(low=-180, high=180, size=1500)
```


```python
# pairs the lats and longs
lat_lngs = zip(lats, lngs)
```


```python
# create a list to save city names
#cities_list = []

#for i in range(0, len(lats), 1):
#    city = citipy.nearest_city(lats[i], lngs[i]).city_name
    
#    if city not in cities_list:
#        cities_list.append(city)
```


```python
# create a list to save city names
cities_list = []

for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    if city not in cities_list:
        cities_list.append(city)
```


```python
len(cities_list)
```




    624




```python
import openweathermapy.core as ow
import urllib
```


```python
# perform API calls
api_key = "0d632d3554840214b23b57727985763e"

# build url for api call
url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=" + api_key

# create list to store city data
city_data = []

# create counter variables
record_count = 1
set_count = 1

#print logger
print("Beginning Data Retrieval")
print("------------------------")

for i, city in enumerate(cities_list):

    # group cities in sets of 50 for logging purposes
    if (i % 50 == 0 and i >= 50):
        set_count += 1
        record_count = 0

    # build the query url
    city_url = url + "&q=" + urllib.request.pathname2url(city)

    print("Processing Record %s of Set %s | %s" % (record_count, set_count, city))
    print(city_url)

    # update record count
    record_count += 1

    try:
        # grab json data using API request
        city_weather = req.get(city_url).json()

        # parse the data
        city_lat = city_weather["coord"]["lat"]
        city_lng = city_weather["coord"]["lon"]
        city_max_temp = city_weather["main"]["temp_max"]
        city_hum_percent = city_weather["main"]["humidity"]
        city_cloud_percent = city_weather["clouds"]["all"]
        city_wind_speed = city_weather["wind"]["speed"]

        city_data.append({"city": city,
                  "lat": city_lat,
                  "lon": city_lng,
                  "max temp": city_max_temp,
                  "humidity perc": city_hum_percent,
                  "cloudiness perc": city_cloud_percent,
                  "wind speed": city_wind_speed})


    except:
        print("city not found")
        pass
    
```

    Beginning Data Retrieval
    ------------------------
    Processing Record 1 of Set 1 | kilindoni
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kilindoni
    Processing Record 2 of Set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ushuaia
    Processing Record 3 of Set 1 | sawtell
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sawtell
    Processing Record 4 of Set 1 | snezhnogorsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=snezhnogorsk
    Processing Record 5 of Set 1 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yellowknife
    Processing Record 6 of Set 1 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20elizabeth
    Processing Record 7 of Set 1 | asau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=asau
    city not found
    Processing Record 8 of Set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20alfred
    Processing Record 9 of Set 1 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=taolanaro
    city not found
    Processing Record 10 of Set 1 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cherskiy
    Processing Record 11 of Set 1 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=klaksvik
    Processing Record 12 of Set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=busselton
    Processing Record 13 of Set 1 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=belushya%20guba
    city not found
    Processing Record 14 of Set 1 | luena
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=luena
    Processing Record 15 of Set 1 | ngukurr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ngukurr
    city not found
    Processing Record 16 of Set 1 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saskylakh
    Processing Record 17 of Set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vaini
    Processing Record 18 of Set 1 | caranavi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=caranavi
    Processing Record 19 of Set 1 | morgantown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=morgantown
    Processing Record 20 of Set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rikitea
    Processing Record 21 of Set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=albany
    Processing Record 22 of Set 1 | samusu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=samusu
    city not found
    Processing Record 23 of Set 1 | portland
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=portland
    Processing Record 24 of Set 1 | manali
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=manali
    Processing Record 25 of Set 1 | rajpipla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rajpipla
    Processing Record 26 of Set 1 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=victoria
    Processing Record 27 of Set 1 | chinu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chinu
    Processing Record 28 of Set 1 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=illoqqortoormiut
    city not found
    Processing Record 29 of Set 1 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=punta%20arenas
    Processing Record 30 of Set 1 | menongue
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=menongue
    Processing Record 31 of Set 1 | saryshagan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saryshagan
    city not found
    Processing Record 32 of Set 1 | adrar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=adrar
    Processing Record 33 of Set 1 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hermanus
    Processing Record 34 of Set 1 | dinguiraye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dinguiraye
    Processing Record 35 of Set 1 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=komsomolskiy
    Processing Record 36 of Set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bredasdorp
    Processing Record 37 of Set 1 | zaysan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zaysan
    Processing Record 38 of Set 1 | the pas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=the%20pas
    Processing Record 39 of Set 1 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tiksi
    Processing Record 40 of Set 1 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sinnamary
    Processing Record 41 of Set 1 | east london
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=east%20london
    Processing Record 42 of Set 1 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nikolskoye
    Processing Record 43 of Set 1 | abaza
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=abaza
    Processing Record 44 of Set 1 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bluff
    Processing Record 45 of Set 1 | lagoa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lagoa
    Processing Record 46 of Set 1 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=severo-kurilsk
    Processing Record 47 of Set 1 | flinders
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=flinders
    Processing Record 48 of Set 1 | abu kamal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=abu%20kamal
    Processing Record 49 of Set 1 | sitka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sitka
    Processing Record 50 of Set 1 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=jamestown
    Processing Record 0 of Set 2 | luancheng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=luancheng
    Processing Record 1 of Set 2 | astana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=astana
    Processing Record 2 of Set 2 | dianopolis
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dianopolis
    city not found
    Processing Record 3 of Set 2 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ostrovnoy
    Processing Record 4 of Set 2 | kavieng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kavieng
    Processing Record 5 of Set 2 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tasiilaq
    Processing Record 6 of Set 2 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=butaritari
    Processing Record 7 of Set 2 | anloga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=anloga
    Processing Record 8 of Set 2 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=grand%20river%20south%20east
    city not found
    Processing Record 9 of Set 2 | cayenne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cayenne
    Processing Record 10 of Set 2 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=meulaboh
    Processing Record 11 of Set 2 | lethem
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lethem
    Processing Record 12 of Set 2 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kodiak
    Processing Record 13 of Set 2 | lop buri
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lop%20buri
    Processing Record 14 of Set 2 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=avarua
    Processing Record 15 of Set 2 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chokurdakh
    Processing Record 16 of Set 2 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=palabuhanratu
    city not found
    Processing Record 17 of Set 2 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puerto%20ayora
    Processing Record 18 of Set 2 | panguna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=panguna
    Processing Record 19 of Set 2 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=longyearbyen
    Processing Record 20 of Set 2 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ponta%20do%20sol
    Processing Record 21 of Set 2 | boquira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=boquira
    Processing Record 22 of Set 2 | hirara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hirara
    Processing Record 23 of Set 2 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=new%20norfolk
    Processing Record 24 of Set 2 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lebu
    Processing Record 25 of Set 2 | vao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vao
    Processing Record 26 of Set 2 | ipixuna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ipixuna
    Processing Record 27 of Set 2 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hilo
    Processing Record 28 of Set 2 | salalah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=salalah
    Processing Record 29 of Set 2 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=constitucion
    Processing Record 30 of Set 2 | marcona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marcona
    city not found
    Processing Record 31 of Set 2 | namatanai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=namatanai
    Processing Record 32 of Set 2 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hobart
    Processing Record 33 of Set 2 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint-philippe
    Processing Record 34 of Set 2 | maloshuyka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=maloshuyka
    city not found
    Processing Record 35 of Set 2 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=faanui
    Processing Record 36 of Set 2 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=esperance
    Processing Record 37 of Set 2 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sentyabrskiy
    city not found
    Processing Record 38 of Set 2 | cape town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cape%20town
    Processing Record 39 of Set 2 | thai binh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=thai%20binh
    Processing Record 40 of Set 2 | sabang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sabang
    Processing Record 41 of Set 2 | pontianak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pontianak
    Processing Record 42 of Set 2 | te anau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=te%20anau
    Processing Record 43 of Set 2 | hobyo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hobyo
    Processing Record 44 of Set 2 | shache
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=shache
    Processing Record 45 of Set 2 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kaitangata
    Processing Record 46 of Set 2 | maarianhamina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=maarianhamina
    city not found
    Processing Record 47 of Set 2 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hasaki
    Processing Record 48 of Set 2 | kirakira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kirakira
    Processing Record 49 of Set 2 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tabiauea
    city not found
    Processing Record 0 of Set 3 | providencia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=providencia
    Processing Record 1 of Set 3 | sekoma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sekoma
    Processing Record 2 of Set 3 | college
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=college
    Processing Record 3 of Set 3 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=arraial%20do%20cabo
    Processing Record 4 of Set 3 | bethel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bethel
    Processing Record 5 of Set 3 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kapaa
    Processing Record 6 of Set 3 | riyadh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=riyadh
    Processing Record 7 of Set 3 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mys%20shmidta
    city not found
    Processing Record 8 of Set 3 | cervo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cervo
    Processing Record 9 of Set 3 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=attawapiskat
    city not found
    Processing Record 10 of Set 3 | sao jose da coroa grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sao%20jose%20da%20coroa%20grande
    Processing Record 11 of Set 3 | pathein
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pathein
    Processing Record 12 of Set 3 | carlsbad
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=carlsbad
    Processing Record 13 of Set 3 | dikson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dikson
    Processing Record 14 of Set 3 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mataura
    Processing Record 15 of Set 3 | chaozhou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chaozhou
    Processing Record 16 of Set 3 | castro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=castro
    Processing Record 17 of Set 3 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cockburn%20town
    Processing Record 18 of Set 3 | kitui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kitui
    Processing Record 19 of Set 3 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mar%20del%20plata
    Processing Record 20 of Set 3 | russell
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=russell
    Processing Record 21 of Set 3 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ribeira%20grande
    Processing Record 22 of Set 3 | oranjemund
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=oranjemund
    Processing Record 23 of Set 3 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=narsaq
    Processing Record 24 of Set 3 | abancay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=abancay
    Processing Record 25 of Set 3 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vestmannaeyjar
    Processing Record 26 of Set 3 | nan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nan
    Processing Record 27 of Set 3 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=atuona
    Processing Record 28 of Set 3 | goundi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=goundi
    Processing Record 29 of Set 3 | dwarka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dwarka
    Processing Record 30 of Set 3 | usinsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=usinsk
    Processing Record 31 of Set 3 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bambous%20virieux
    Processing Record 32 of Set 3 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20lincoln
    Processing Record 33 of Set 3 | dingle
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dingle
    Processing Record 34 of Set 3 | bajil
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bajil
    Processing Record 35 of Set 3 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nanortalik
    Processing Record 36 of Set 3 | nantucket
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nantucket
    Processing Record 37 of Set 3 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=comodoro%20rivadavia
    Processing Record 38 of Set 3 | dayong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dayong
    Processing Record 39 of Set 3 | archidona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=archidona
    Processing Record 40 of Set 3 | kieta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kieta
    Processing Record 41 of Set 3 | hami
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hami
    Processing Record 42 of Set 3 | sabile
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sabile
    Processing Record 43 of Set 3 | diffa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=diffa
    Processing Record 44 of Set 3 | santa engracia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=santa%20engracia
    Processing Record 45 of Set 3 | jiazi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=jiazi
    Processing Record 46 of Set 3 | gaspe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=gaspe
    Processing Record 47 of Set 3 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nizhneyansk
    city not found
    Processing Record 48 of Set 3 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tuktoyaktuk
    Processing Record 49 of Set 3 | faya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=faya
    Processing Record 0 of Set 4 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cabo%20san%20lucas
    Processing Record 1 of Set 4 | henties bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=henties%20bay
    Processing Record 2 of Set 4 | alofi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=alofi
    Processing Record 3 of Set 4 | chicama
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chicama
    Processing Record 4 of Set 4 | ballina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ballina
    Processing Record 5 of Set 4 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=san%20cristobal
    Processing Record 6 of Set 4 | bonavista
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bonavista
    Processing Record 7 of Set 4 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mahebourg
    Processing Record 8 of Set 4 | kahului
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kahului
    Processing Record 9 of Set 4 | buchanan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=buchanan
    Processing Record 10 of Set 4 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rio%20gallegos
    Processing Record 11 of Set 4 | sostanj
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sostanj
    Processing Record 12 of Set 4 | fortuna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fortuna
    Processing Record 13 of Set 4 | pokhara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pokhara
    Processing Record 14 of Set 4 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khatanga
    Processing Record 15 of Set 4 | hays
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hays
    Processing Record 16 of Set 4 | mount isa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mount%20isa
    Processing Record 17 of Set 4 | fengrun
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fengrun
    Processing Record 18 of Set 4 | gizo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=gizo
    Processing Record 19 of Set 4 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tsihombe
    city not found
    Processing Record 20 of Set 4 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=carnarvon
    Processing Record 21 of Set 4 | husavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=husavik
    Processing Record 22 of Set 4 | mugango
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mugango
    Processing Record 23 of Set 4 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=banda%20aceh
    Processing Record 24 of Set 4 | high level
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=high%20level
    Processing Record 25 of Set 4 | nouakchott
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nouakchott
    Processing Record 26 of Set 4 | malko tarnovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=malko%20tarnovo
    Processing Record 27 of Set 4 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tuatapere
    Processing Record 28 of Set 4 | taltal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=taltal
    Processing Record 29 of Set 4 | allapalli
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=allapalli
    Processing Record 30 of Set 4 | bundaberg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bundaberg
    Processing Record 31 of Set 4 | magadan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=magadan
    Processing Record 32 of Set 4 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sao%20filipe
    Processing Record 33 of Set 4 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tilichiki
    Processing Record 34 of Set 4 | shingu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=shingu
    Processing Record 35 of Set 4 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=georgetown
    Processing Record 36 of Set 4 | el retorno
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=el%20retorno
    Processing Record 37 of Set 4 | saryozek
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saryozek
    Processing Record 38 of Set 4 | kutum
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kutum
    Processing Record 39 of Set 4 | kralendijk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kralendijk
    Processing Record 40 of Set 4 | ahipara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ahipara
    Processing Record 41 of Set 4 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=barra%20do%20garcas
    Processing Record 42 of Set 4 | sao gabriel da cachoeira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sao%20gabriel%20da%20cachoeira
    Processing Record 43 of Set 4 | karuzi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=karuzi
    Processing Record 44 of Set 4 | beira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=beira
    Processing Record 45 of Set 4 | airai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=airai
    Processing Record 46 of Set 4 | westport
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=westport
    Processing Record 47 of Set 4 | bonthe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bonthe
    Processing Record 48 of Set 4 | novikovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=novikovo
    Processing Record 49 of Set 4 | stillwater
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=stillwater
    Processing Record 0 of Set 5 | kaeo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kaeo
    Processing Record 1 of Set 5 | berlevag
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=berlevag
    Processing Record 2 of Set 5 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pevek
    Processing Record 3 of Set 5 | vostok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vostok
    Processing Record 4 of Set 5 | tonantins
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tonantins
    Processing Record 5 of Set 5 | copan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=copan
    Processing Record 6 of Set 5 | ola
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ola
    Processing Record 7 of Set 5 | ranau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ranau
    Processing Record 8 of Set 5 | lata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lata
    Processing Record 9 of Set 5 | khonuu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khonuu
    city not found
    Processing Record 10 of Set 5 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mount%20gambier
    Processing Record 11 of Set 5 | roald
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=roald
    Processing Record 12 of Set 5 | la ronge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=la%20ronge
    Processing Record 13 of Set 5 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=thompson
    Processing Record 14 of Set 5 | padang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=padang
    Processing Record 15 of Set 5 | limbang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=limbang
    Processing Record 16 of Set 5 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=barentsburg
    city not found
    Processing Record 17 of Set 5 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=torbay
    Processing Record 18 of Set 5 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=norman%20wells
    Processing Record 19 of Set 5 | aykhal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=aykhal
    Processing Record 20 of Set 5 | mehamn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mehamn
    Processing Record 21 of Set 5 | mocambique
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mocambique
    city not found
    Processing Record 22 of Set 5 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=clyde%20river
    Processing Record 23 of Set 5 | bogorodskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bogorodskoye
    Processing Record 24 of Set 5 | bell ville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bell%20ville
    Processing Record 25 of Set 5 | vanavara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vanavara
    Processing Record 26 of Set 5 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cidreira
    Processing Record 27 of Set 5 | vreed en hoop
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vreed%20en%20hoop
    city not found
    Processing Record 28 of Set 5 | chagda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chagda
    city not found
    Processing Record 29 of Set 5 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kruisfontein
    Processing Record 30 of Set 5 | paita
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=paita
    Processing Record 31 of Set 5 | vardo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vardo
    Processing Record 32 of Set 5 | mitrofanovka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mitrofanovka
    Processing Record 33 of Set 5 | karauzyak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=karauzyak
    city not found
    Processing Record 34 of Set 5 | alice springs
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=alice%20springs
    Processing Record 35 of Set 5 | marrakesh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marrakesh
    Processing Record 36 of Set 5 | kabelvag
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kabelvag
    Processing Record 37 of Set 5 | port hardy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20hardy
    Processing Record 38 of Set 5 | vagay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vagay
    Processing Record 39 of Set 5 | svetlaya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=svetlaya
    Processing Record 40 of Set 5 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bandarbeyla
    Processing Record 41 of Set 5 | colinas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=colinas
    Processing Record 42 of Set 5 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vila%20velha
    Processing Record 43 of Set 5 | mount pleasant
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mount%20pleasant
    Processing Record 44 of Set 5 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ilulissat
    Processing Record 45 of Set 5 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=half%20moon%20bay
    Processing Record 46 of Set 5 | upernavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=upernavik
    Processing Record 47 of Set 5 | nabire
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nabire
    Processing Record 48 of Set 5 | tidore
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tidore
    city not found
    Processing Record 49 of Set 5 | chara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chara
    Processing Record 0 of Set 6 | saleaula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saleaula
    city not found
    Processing Record 1 of Set 6 | sfantu gheorghe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sfantu%20gheorghe
    Processing Record 2 of Set 6 | marmande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marmande
    Processing Record 3 of Set 6 | sukumo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sukumo
    Processing Record 4 of Set 6 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bathsheba
    Processing Record 5 of Set 6 | utiroa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=utiroa
    city not found
    Processing Record 6 of Set 6 | srandakan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=srandakan
    Processing Record 7 of Set 6 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=barrow
    Processing Record 8 of Set 6 | dali
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dali
    Processing Record 9 of Set 6 | oum hadjer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=oum%20hadjer
    Processing Record 10 of Set 6 | deep river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=deep%20river
    Processing Record 11 of Set 6 | torzhok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=torzhok
    Processing Record 12 of Set 6 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=los%20llanos%20de%20aridane
    Processing Record 13 of Set 6 | panjab
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=panjab
    Processing Record 14 of Set 6 | longlac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=longlac
    city not found
    Processing Record 15 of Set 6 | newton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=newton
    Processing Record 16 of Set 6 | porto novo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=porto%20novo
    Processing Record 17 of Set 6 | urubicha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=urubicha
    Processing Record 18 of Set 6 | abeokuta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=abeokuta
    Processing Record 19 of Set 6 | pisco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pisco
    Processing Record 20 of Set 6 | lokoja
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lokoja
    Processing Record 21 of Set 6 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=qaanaaq
    Processing Record 22 of Set 6 | tromso
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tromso
    Processing Record 23 of Set 6 | south sioux city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=south%20sioux%20city
    Processing Record 24 of Set 6 | baleshwar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=baleshwar
    Processing Record 25 of Set 6 | kassala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kassala
    Processing Record 26 of Set 6 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=coquimbo
    Processing Record 27 of Set 6 | beitbridge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=beitbridge
    Processing Record 28 of Set 6 | stromness
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=stromness
    Processing Record 29 of Set 6 | yulara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yulara
    Processing Record 30 of Set 6 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=beringovskiy
    Processing Record 31 of Set 6 | yerbogachen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yerbogachen
    Processing Record 32 of Set 6 | de aar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=de%20aar
    Processing Record 33 of Set 6 | touros
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=touros
    Processing Record 34 of Set 6 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saldanha
    Processing Record 35 of Set 6 | korla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=korla
    city not found
    Processing Record 36 of Set 6 | arrecife
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=arrecife
    city not found
    Processing Record 37 of Set 6 | hit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hit
    Processing Record 38 of Set 6 | jalu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=jalu
    Processing Record 39 of Set 6 | saint george
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint%20george
    Processing Record 40 of Set 6 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=quatre%20cocos
    Processing Record 41 of Set 6 | mutoko
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mutoko
    Processing Record 42 of Set 6 | billings
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=billings
    Processing Record 43 of Set 6 | wanxian
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=wanxian
    Processing Record 44 of Set 6 | meadow lake
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=meadow%20lake
    Processing Record 45 of Set 6 | orlik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=orlik
    Processing Record 46 of Set 6 | sorland
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sorland
    Processing Record 47 of Set 6 | tchaourou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tchaourou
    Processing Record 48 of Set 6 | zhezkazgan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zhezkazgan
    Processing Record 49 of Set 6 | opuwo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=opuwo
    Processing Record 0 of Set 7 | terre-de-bas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=terre-de-bas
    Processing Record 1 of Set 7 | sioux lookout
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sioux%20lookout
    Processing Record 2 of Set 7 | sartell
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sartell
    Processing Record 3 of Set 7 | ducheng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ducheng
    Processing Record 4 of Set 7 | markova
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=markova
    Processing Record 5 of Set 7 | omsukchan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=omsukchan
    Processing Record 6 of Set 7 | bowen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bowen
    Processing Record 7 of Set 7 | vyshchetarasivka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vyshchetarasivka
    Processing Record 8 of Set 7 | saint-louis
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint-louis
    Processing Record 9 of Set 7 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bengkulu
    city not found
    Processing Record 10 of Set 7 | talcahuano
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=talcahuano
    Processing Record 11 of Set 7 | ventspils
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ventspils
    Processing Record 12 of Set 7 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=isangel
    Processing Record 13 of Set 7 | katsuura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=katsuura
    Processing Record 14 of Set 7 | bojnurd
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bojnurd
    Processing Record 15 of Set 7 | leca da palmeira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=leca%20da%20palmeira
    Processing Record 16 of Set 7 | myitkyina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=myitkyina
    Processing Record 17 of Set 7 | el dorado
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=el%20dorado
    Processing Record 18 of Set 7 | palauig
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=palauig
    Processing Record 19 of Set 7 | khash
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khash
    Processing Record 20 of Set 7 | muros
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=muros
    Processing Record 21 of Set 7 | homer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=homer
    Processing Record 22 of Set 7 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hithadhoo
    Processing Record 23 of Set 7 | canico
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=canico
    Processing Record 24 of Set 7 | sumoto
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sumoto
    Processing Record 25 of Set 7 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cabedelo
    Processing Record 26 of Set 7 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zhigansk
    Processing Record 27 of Set 7 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=umm%20lajj
    Processing Record 28 of Set 7 | haines junction
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=haines%20junction
    Processing Record 29 of Set 7 | zonguldak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zonguldak
    Processing Record 30 of Set 7 | mayumba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mayumba
    Processing Record 31 of Set 7 | grindavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=grindavik
    Processing Record 32 of Set 7 | bud
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bud
    Processing Record 33 of Set 7 | buala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=buala
    Processing Record 34 of Set 7 | hofn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hofn
    Processing Record 35 of Set 7 | callaway
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=callaway
    Processing Record 36 of Set 7 | vanimo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vanimo
    Processing Record 37 of Set 7 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint-joseph
    Processing Record 38 of Set 7 | ust-nera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ust-nera
    Processing Record 39 of Set 7 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ust-kuyga
    Processing Record 40 of Set 7 | miraflores
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=miraflores
    Processing Record 41 of Set 7 | amderma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=amderma
    city not found
    Processing Record 42 of Set 7 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fairbanks
    Processing Record 43 of Set 7 | athens
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=athens
    Processing Record 44 of Set 7 | marawi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marawi
    Processing Record 45 of Set 7 | beni
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=beni
    Processing Record 46 of Set 7 | bogovarovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bogovarovo
    Processing Record 47 of Set 7 | kampene
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kampene
    Processing Record 48 of Set 7 | grande-riviere
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=grande-riviere
    city not found
    Processing Record 49 of Set 7 | zhicheng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zhicheng
    Processing Record 0 of Set 8 | caravelas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=caravelas
    Processing Record 1 of Set 8 | marsa matruh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marsa%20matruh
    Processing Record 2 of Set 8 | sindou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sindou
    Processing Record 3 of Set 8 | talnakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=talnakh
    Processing Record 4 of Set 8 | sikasso
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sikasso
    Processing Record 5 of Set 8 | camalu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=camalu
    Processing Record 6 of Set 8 | puerto colombia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puerto%20colombia
    Processing Record 7 of Set 8 | puerto carreno
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puerto%20carreno
    Processing Record 8 of Set 8 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=provideniya
    Processing Record 9 of Set 8 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kloulklubed
    Processing Record 10 of Set 8 | viedma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=viedma
    Processing Record 11 of Set 8 | deshna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=deshna
    city not found
    Processing Record 12 of Set 8 | kumluca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kumluca
    Processing Record 13 of Set 8 | lorengau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lorengau
    Processing Record 14 of Set 8 | formoso do araguaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=formoso%20do%20araguaia
    city not found
    Processing Record 15 of Set 8 | luderitz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=luderitz
    Processing Record 16 of Set 8 | rayon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rayon
    Processing Record 17 of Set 8 | kimbe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kimbe
    Processing Record 18 of Set 8 | raudeberg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=raudeberg
    Processing Record 19 of Set 8 | aguimes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=aguimes
    Processing Record 20 of Set 8 | batangafo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=batangafo
    Processing Record 21 of Set 8 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=guerrero%20negro
    Processing Record 22 of Set 8 | livingston
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=livingston
    Processing Record 23 of Set 8 | eureka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=eureka
    Processing Record 24 of Set 8 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port-gentil
    Processing Record 25 of Set 8 | dir
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dir
    Processing Record 26 of Set 8 | tiruvottiyur
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tiruvottiyur
    city not found
    Processing Record 27 of Set 8 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vaitupu
    city not found
    Processing Record 28 of Set 8 | bargal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bargal
    city not found
    Processing Record 29 of Set 8 | maham
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=maham
    Processing Record 30 of Set 8 | puga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puga
    Processing Record 31 of Set 8 | arkhangelsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=arkhangelsk
    Processing Record 32 of Set 8 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=iqaluit
    Processing Record 33 of Set 8 | karamea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=karamea
    city not found
    Processing Record 34 of Set 8 | verkhnevilyuysk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=verkhnevilyuysk
    Processing Record 35 of Set 8 | bull savanna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bull%20savanna
    Processing Record 36 of Set 8 | dalvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dalvik
    Processing Record 37 of Set 8 | broken hill
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=broken%20hill
    Processing Record 38 of Set 8 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=souillac
    Processing Record 39 of Set 8 | praxedis guerrero
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=praxedis%20guerrero
    Processing Record 40 of Set 8 | louth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=louth
    Processing Record 41 of Set 8 | burica
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=burica
    city not found
    Processing Record 42 of Set 8 | ozieri
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ozieri
    Processing Record 43 of Set 8 | kamenskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kamenskoye
    city not found
    Processing Record 44 of Set 8 | hovd
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hovd
    Processing Record 45 of Set 8 | dharchula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dharchula
    Processing Record 46 of Set 8 | avera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=avera
    Processing Record 47 of Set 8 | atakpame
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=atakpame
    Processing Record 48 of Set 8 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint-francois
    Processing Record 49 of Set 8 | broome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=broome
    Processing Record 0 of Set 9 | bolshoy tsaryn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bolshoy%20tsaryn
    city not found
    Processing Record 1 of Set 9 | kentville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kentville
    Processing Record 2 of Set 9 | tecoanapa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tecoanapa
    Processing Record 3 of Set 9 | rajshahi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rajshahi
    Processing Record 4 of Set 9 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=saint-augustin
    Processing Record 5 of Set 9 | varna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=varna
    Processing Record 6 of Set 9 | bubaque
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bubaque
    Processing Record 7 of Set 9 | mandera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mandera
    Processing Record 8 of Set 9 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=santa%20cruz
    Processing Record 9 of Set 9 | tabou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tabou
    Processing Record 10 of Set 9 | halalo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=halalo
    city not found
    Processing Record 11 of Set 9 | acari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=acari
    Processing Record 12 of Set 9 | rio grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rio%20grande
    Processing Record 13 of Set 9 | cookeville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cookeville
    Processing Record 14 of Set 9 | ust-ilimsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ust-ilimsk
    Processing Record 15 of Set 9 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20macquarie
    Processing Record 16 of Set 9 | hun
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hun
    Processing Record 17 of Set 9 | muroto
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=muroto
    Processing Record 18 of Set 9 | ankang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ankang
    Processing Record 19 of Set 9 | port blair
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=port%20blair
    Processing Record 20 of Set 9 | chuy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chuy
    Processing Record 21 of Set 9 | kankon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kankon
    Processing Record 22 of Set 9 | twentynine palms
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=twentynine%20palms
    Processing Record 23 of Set 9 | igarka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=igarka
    Processing Record 24 of Set 9 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=praia%20da%20vitoria
    Processing Record 25 of Set 9 | laguna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=laguna
    Processing Record 26 of Set 9 | gat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=gat
    Processing Record 27 of Set 9 | borama
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=borama
    city not found
    Processing Record 28 of Set 9 | alice town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=alice%20town
    Processing Record 29 of Set 9 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lavrentiya
    Processing Record 30 of Set 9 | aklavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=aklavik
    Processing Record 31 of Set 9 | santa maria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=santa%20maria
    Processing Record 32 of Set 9 | paamiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=paamiut
    Processing Record 33 of Set 9 | khammam
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khammam
    Processing Record 34 of Set 9 | wa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=wa
    city not found
    Processing Record 35 of Set 9 | dzilam gonzalez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dzilam%20gonzalez
    Processing Record 36 of Set 9 | safaga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=safaga
    city not found
    Processing Record 37 of Set 9 | gberia fotombu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=gberia%20fotombu
    Processing Record 38 of Set 9 | sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sao%20felix%20do%20xingu
    Processing Record 39 of Set 9 | puerto del rosario
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puerto%20del%20rosario
    Processing Record 40 of Set 9 | lodwar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lodwar
    Processing Record 41 of Set 9 | puntarenas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puntarenas
    Processing Record 42 of Set 9 | biloli
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=biloli
    Processing Record 43 of Set 9 | rock sound
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rock%20sound
    Processing Record 44 of Set 9 | bimbo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bimbo
    Processing Record 45 of Set 9 | sassandra
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sassandra
    Processing Record 46 of Set 9 | uribia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=uribia
    Processing Record 47 of Set 9 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sobolevo
    Processing Record 48 of Set 9 | orapa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=orapa
    Processing Record 49 of Set 9 | ogre
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ogre
    Processing Record 0 of Set 10 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=okhotsk
    Processing Record 1 of Set 10 | lomovka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lomovka
    Processing Record 2 of Set 10 | sao miguel do araguaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sao%20miguel%20do%20araguaia
    Processing Record 3 of Set 10 | nome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nome
    Processing Record 4 of Set 10 | ormara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ormara
    Processing Record 5 of Set 10 | smithers
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=smithers
    Processing Record 6 of Set 10 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=olafsvik
    city not found
    Processing Record 7 of Set 10 | teya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=teya
    Processing Record 8 of Set 10 | santa vitoria do palmar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=santa%20vitoria%20do%20palmar
    Processing Record 9 of Set 10 | suda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=suda
    Processing Record 10 of Set 10 | nara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nara
    Processing Record 11 of Set 10 | belmonte
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=belmonte
    Processing Record 12 of Set 10 | ust-kamchatsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ust-kamchatsk
    city not found
    Processing Record 13 of Set 10 | fez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fez
    Processing Record 14 of Set 10 | pochutla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pochutla
    Processing Record 15 of Set 10 | tarko-sale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tarko-sale
    Processing Record 16 of Set 10 | bemidji
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bemidji
    Processing Record 17 of Set 10 | urfa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=urfa
    city not found
    Processing Record 18 of Set 10 | samarai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=samarai
    Processing Record 19 of Set 10 | rauma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rauma
    Processing Record 20 of Set 10 | amahai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=amahai
    Processing Record 21 of Set 10 | merauke
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=merauke
    Processing Record 22 of Set 10 | soyo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=soyo
    Processing Record 23 of Set 10 | mahibadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mahibadhoo
    Processing Record 24 of Set 10 | rungata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rungata
    city not found
    Processing Record 25 of Set 10 | axim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=axim
    Processing Record 26 of Set 10 | yermakovskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yermakovskoye
    Processing Record 27 of Set 10 | nadym
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nadym
    Processing Record 28 of Set 10 | san ramon de la nueva oran
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=san%20ramon%20de%20la%20nueva%20oran
    Processing Record 29 of Set 10 | neijiang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=neijiang
    Processing Record 30 of Set 10 | hay river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hay%20river
    Processing Record 31 of Set 10 | aleksandrov gay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=aleksandrov%20gay
    Processing Record 32 of Set 10 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=geraldton
    Processing Record 33 of Set 10 | kilmez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kilmez
    city not found
    Processing Record 34 of Set 10 | silay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=silay
    Processing Record 35 of Set 10 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fort%20nelson
    Processing Record 36 of Set 10 | cheremshan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cheremshan
    Processing Record 37 of Set 10 | kupang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kupang
    Processing Record 38 of Set 10 | monte cristi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=monte%20cristi
    city not found
    Processing Record 39 of Set 10 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yar-sale
    Processing Record 40 of Set 10 | honiara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=honiara
    Processing Record 41 of Set 10 | kautokeino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kautokeino
    Processing Record 42 of Set 10 | pasni
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pasni
    Processing Record 43 of Set 10 | north bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=north%20bay
    Processing Record 44 of Set 10 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=belaya%20gora
    Processing Record 45 of Set 10 | lasa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lasa
    Processing Record 46 of Set 10 | sistranda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sistranda
    Processing Record 47 of Set 10 | nizhniy baskunchak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nizhniy%20baskunchak
    Processing Record 48 of Set 10 | amapa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=amapa
    Processing Record 49 of Set 10 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=thinadhoo
    Processing Record 0 of Set 11 | goderich
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=goderich
    Processing Record 1 of Set 11 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=san%20patricio
    Processing Record 2 of Set 11 | demerval lobao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=demerval%20lobao
    Processing Record 3 of Set 11 | khandyga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khandyga
    Processing Record 4 of Set 11 | vuktyl
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=vuktyl
    Processing Record 5 of Set 11 | seymchan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=seymchan
    Processing Record 6 of Set 11 | blonduos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=blonduos
    city not found
    Processing Record 7 of Set 11 | bafoulabe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bafoulabe
    Processing Record 8 of Set 11 | mtsensk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mtsensk
    Processing Record 9 of Set 11 | nichinan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nichinan
    Processing Record 10 of Set 11 | fomboni
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=fomboni
    Processing Record 11 of Set 11 | carutapera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=carutapera
    Processing Record 12 of Set 11 | lalibela
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lalibela
    Processing Record 13 of Set 11 | bedesa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bedesa
    Processing Record 14 of Set 11 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=deputatskiy
    Processing Record 15 of Set 11 | shelburne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=shelburne
    Processing Record 16 of Set 11 | shimoda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=shimoda
    Processing Record 17 of Set 11 | balakovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=balakovo
    Processing Record 18 of Set 11 | coihaique
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=coihaique
    Processing Record 19 of Set 11 | pueblo bello
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pueblo%20bello
    Processing Record 20 of Set 11 | puerto baquerizo moreno
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=puerto%20baquerizo%20moreno
    Processing Record 21 of Set 11 | brewster
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=brewster
    Processing Record 22 of Set 11 | mookane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mookane
    Processing Record 23 of Set 11 | springbok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=springbok
    Processing Record 24 of Set 11 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=grand%20gaube
    Processing Record 25 of Set 11 | bismarck
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bismarck
    Processing Record 26 of Set 11 | auki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=auki
    Processing Record 27 of Set 11 | loikaw
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=loikaw
    Processing Record 28 of Set 11 | moerai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=moerai
    Processing Record 29 of Set 11 | silver city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=silver%20city
    Processing Record 30 of Set 11 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hamilton
    Processing Record 31 of Set 11 | rafaela
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rafaela
    Processing Record 32 of Set 11 | nelson bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nelson%20bay
    Processing Record 33 of Set 11 | chapais
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=chapais
    Processing Record 34 of Set 11 | naze
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=naze
    Processing Record 35 of Set 11 | griffith
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=griffith
    Processing Record 36 of Set 11 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=boyolangu
    Processing Record 37 of Set 11 | salekhard
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=salekhard
    Processing Record 38 of Set 11 | labuan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=labuan
    Processing Record 39 of Set 11 | pimentel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pimentel
    Processing Record 40 of Set 11 | ocos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ocos
    Processing Record 41 of Set 11 | laiagam
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=laiagam
    city not found
    Processing Record 42 of Set 11 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cap%20malheureux
    Processing Record 43 of Set 11 | zyryanovsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zyryanovsk
    Processing Record 44 of Set 11 | rybnaya sloboda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rybnaya%20sloboda
    Processing Record 45 of Set 11 | kushiro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kushiro
    Processing Record 46 of Set 11 | pandan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pandan
    Processing Record 47 of Set 11 | cap-aux-meules
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=cap-aux-meules
    Processing Record 48 of Set 11 | sibolga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sibolga
    Processing Record 49 of Set 11 | huejuquilla el alto
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=huejuquilla%20el%20alto
    Processing Record 0 of Set 12 | colac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=colac
    Processing Record 1 of Set 12 | lerwick
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=lerwick
    Processing Record 2 of Set 12 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=skalistyy
    city not found
    Processing Record 3 of Set 12 | alyangula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=alyangula
    Processing Record 4 of Set 12 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ketchikan
    Processing Record 5 of Set 12 | kaupanger
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kaupanger
    Processing Record 6 of Set 12 | umtata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=umtata
    Processing Record 7 of Set 12 | huarmey
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=huarmey
    Processing Record 8 of Set 12 | robertsport
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=robertsport
    Processing Record 9 of Set 12 | ancud
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ancud
    Processing Record 10 of Set 12 | macenta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=macenta
    Processing Record 11 of Set 12 | san vicente
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=san%20vicente
    Processing Record 12 of Set 12 | jimenez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=jimenez
    Processing Record 13 of Set 12 | madang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=madang
    Processing Record 14 of Set 12 | silopi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=silopi
    Processing Record 15 of Set 12 | rawson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=rawson
    Processing Record 16 of Set 12 | yongchang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yongchang
    Processing Record 17 of Set 12 | palmer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=palmer
    Processing Record 18 of Set 12 | yudong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yudong
    Processing Record 19 of Set 12 | brae
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=brae
    Processing Record 20 of Set 12 | jiaohe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=jiaohe
    Processing Record 21 of Set 12 | tessalit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tessalit
    Processing Record 22 of Set 12 | shizunai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=shizunai
    Processing Record 23 of Set 12 | sharanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=sharanga
    Processing Record 24 of Set 12 | johi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=johi
    Processing Record 25 of Set 12 | yefira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yefira
    city not found
    Processing Record 26 of Set 12 | pandaria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=pandaria
    Processing Record 27 of Set 12 | phan thiet
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=phan%20thiet
    Processing Record 28 of Set 12 | hambantota
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hambantota
    Processing Record 29 of Set 12 | bella vista
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=bella%20vista
    Processing Record 30 of Set 12 | along
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=along
    Processing Record 31 of Set 12 | hunza
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hunza
    city not found
    Processing Record 32 of Set 12 | moron
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=moron
    Processing Record 33 of Set 12 | taft
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=taft
    Processing Record 34 of Set 12 | farmington
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=farmington
    Processing Record 35 of Set 12 | tefe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tefe
    Processing Record 36 of Set 12 | kapit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kapit
    Processing Record 37 of Set 12 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yarmouth
    Processing Record 38 of Set 12 | otane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=otane
    Processing Record 39 of Set 12 | batticaloa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=batticaloa
    Processing Record 40 of Set 12 | mecca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=mecca
    Processing Record 41 of Set 12 | kishi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kishi
    Processing Record 42 of Set 12 | ravar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ravar
    Processing Record 43 of Set 12 | daru
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=daru
    Processing Record 44 of Set 12 | yumen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=yumen
    Processing Record 45 of Set 12 | marzuq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=marzuq
    Processing Record 46 of Set 12 | nipawin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=nipawin
    Processing Record 47 of Set 12 | key west
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=key%20west
    Processing Record 48 of Set 12 | ambatofinandrahana
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=ambatofinandrahana
    Processing Record 49 of Set 12 | dubti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=dubti
    Processing Record 0 of Set 13 | hannibal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=hannibal
    Processing Record 1 of Set 13 | barra patuca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=barra%20patuca
    Processing Record 2 of Set 13 | kon tum
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kon%20tum
    Processing Record 3 of Set 13 | a
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=a
    city not found
    Processing Record 4 of Set 13 | morsbach
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=morsbach
    Processing Record 5 of Set 13 | barpathar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=barpathar
    Processing Record 6 of Set 13 | iquique
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=iquique
    Processing Record 7 of Set 13 | staroaleyskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=staroaleyskoye
    Processing Record 8 of Set 13 | huilong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=huilong
    Processing Record 9 of Set 13 | iracoubo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=iracoubo
    Processing Record 10 of Set 13 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=codrington
    Processing Record 11 of Set 13 | kishapu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kishapu
    Processing Record 12 of Set 13 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=kavaratti
    Processing Record 13 of Set 13 | san ignacio
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=san%20ignacio
    Processing Record 14 of Set 13 | itarema
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=itarema
    Processing Record 15 of Set 13 | galesong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=galesong
    Processing Record 16 of Set 13 | valley city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=valley%20city
    Processing Record 17 of Set 13 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=richards%20bay
    Processing Record 18 of Set 13 | takoradi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=takoradi
    Processing Record 19 of Set 13 | imbituba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=imbituba
    Processing Record 20 of Set 13 | tianpeng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=tianpeng
    Processing Record 21 of Set 13 | khairpur nathan shah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=khairpur%20nathan%20shah
    Processing Record 22 of Set 13 | moorhead
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=moorhead
    Processing Record 23 of Set 13 | zeya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=0d632d3554840214b23b57727985763e&q=zeya
    


```python
city_weather_data_df = pd.DataFrame(city_data)
```


```python
city_weather_data_df.head()
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
      <th>city</th>
      <th>cloudiness perc</th>
      <th>humidity perc</th>
      <th>lat</th>
      <th>lon</th>
      <th>max temp</th>
      <th>wind speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>kilindoni</td>
      <td>12</td>
      <td>99</td>
      <td>-7.91</td>
      <td>39.67</td>
      <td>83.7</td>
      <td>8.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ushuaia</td>
      <td>75</td>
      <td>75</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>42.8</td>
      <td>27.51</td>
    </tr>
    <tr>
      <th>2</th>
      <td>sawtell</td>
      <td>40</td>
      <td>88</td>
      <td>-30.38</td>
      <td>153.10</td>
      <td>68.0</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>3</th>
      <td>snezhnogorsk</td>
      <td>75</td>
      <td>85</td>
      <td>69.19</td>
      <td>33.23</td>
      <td>19.4</td>
      <td>6.71</td>
    </tr>
    <tr>
      <th>4</th>
      <td>yellowknife</td>
      <td>75</td>
      <td>72</td>
      <td>62.45</td>
      <td>-114.38</td>
      <td>14.0</td>
      <td>13.87</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Build a scatter plot for each data type

plt.figure(figsize=(20,9))

plt.scatter(city_weather_data_df["lat"], city_weather_data_df["max temp"], marker="o")

# Incorporate the other graph properties
plt.title("City Latitude vs. Max Temperature (03/16/18)")
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Images/City Latitude vs. Max Temperature (03.16.18).png")

# Show plot
plt.show()
```


![png](output_15_0.png)



```python
# Build a scatter plot for each data type

plt.figure(figsize=(20,9))

plt.scatter(city_weather_data_df["lat"], city_weather_data_df["humidity perc"], marker="o")

# Incorporate the other graph properties
plt.title("City Latitude vs. Humidity (03/16/18)")
plt.ylabel("Humidity (%) ")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Images/City Latitude vs. Humidity (03.16.18).png")

# Show plot
plt.show()
```


![png](output_16_0.png)



```python
# Build a scatter plot for each data type

plt.figure(figsize=(20,9))

plt.scatter(city_weather_data_df["lat"], city_weather_data_df["cloudiness perc"], marker="o")

# Incorporate the other graph properties
plt.title("City Latitude vs. Cloudiness (03/16/18)")
plt.ylabel("Cloudiness (%) ")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Images/City Latitude vs. Cloudiness (03.16.18).png")

# Show plot
plt.show()
```


![png](output_17_0.png)



```python
# Build a scatter plot for each data type

plt.figure(figsize=(20,9))

plt.scatter(city_weather_data_df["lat"], city_weather_data_df["wind speed"], marker="o")

# Incorporate the other graph properties
plt.title("City Latitude vs. Wind Speed (03/16/18)")
plt.ylabel("Wind Speed (mph) ")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Images/City Latitude vs. Wind Speed (03.16.18).png")

# Show plot
plt.show()
```


![png](output_18_0.png)



```python
# export dataframe to csv

city_weather_data_df.to_csv("Data/city_weather_data.csv", sep=",")
```
