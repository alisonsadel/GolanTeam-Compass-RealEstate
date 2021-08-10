# Golan Team Compass Real Estate

#### Initial Cleaning & DataType Conversion

* Import libraries
  ```import pandas as pd
     import numpy as np```
* After reading in the csv, to see the full column values to better visualize the data I used ```pd.set_option('max_colwidth', 800)```
* Checked datatypes ```df.dtypes``` and converted year int64 to datetime  ```df['Year'] = pd.to_datetime(df['Year'], format='%Y')```
* The dataset also contained sales outside of NYC in NJ, MD and CT. For the purposes of the visualizations, I filtered to include only data in NY using a Boolean Mask and renamed Address to Street, reserving "Address" as the header in preparation to concat all the location values.
  ```filtered = df['State'] == 'New York'
     full = df[filtered] 
     full = full.rename(columns= {"Address": "Street"})```
     
* Created individual arrays to extract and all location column values individually
  ```a = np.char.array(full['Street'].values)
     b = np.char.array(full['City'].values)
     c = np.char.array(full['State'].values)
     d = np.char.array(full['Postalcode'].values)```
     
* Merge 
  ```full['Location'] = a.astype(str) + ', ' + b.astype(str) + ', ' + c.astype(str) + ', ' + d.astype(str)```

#### Transform - Geocode Latitude & Longitude 
* Libraries used for API Call
  ```from geopy.geocoders import Nominatim
     geolocator = Nominatim(user_agent= 'youremailhere@gmail.com'
     from geopy.extra.rate_limiter import RateLimiter
     import webbrowser```
