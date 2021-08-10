# Golan Team Compass Real Estate

#### Part One - Initial Cleaning & DataType Conversion
     
* After importing in pandas, numpy and reading in the csv, to see the full column values to better visualize the data I used ```pd.set_option('max_colwidth', 800)```
* Checked datatypes ```df.dtypes``` and converted year int64 to datetime  ```df['Year'] = pd.to_datetime(df['Year'], format='%Y')```
* The dataset also contained sales outside of NYC in NJ, MD and CT but our focus is New York. 
  
  ```
  # Filter data to only include transactions in New York using a Boolean Mask
  filtered = df['State'] == 'New York'
  full = df[filtered] 
  
  # Rename "Address" to "Street", reserving "Address" as the header in preparation to concat all the location values.
  full = full.rename(columns= {"Address": "Street"})
  
  # Create individual arrays to extract and all location column values individually
  
  a = np.char.array(full['Street'].values)
  b = np.char.array(full['City'].values)
  c = np.char.array(full['State'].values)
  d = np.char.array(full['Postalcode'].values)
  
  # Merge 
  full['Location'] = a.astype(str) + ', ' + b.astype(str) + ', ' + c.astype(str) + ', ' + d.astype(str)

#### Part Two - Transform - Geocode Latitude & Longitude 

```
  # Libraries used for API Call
  
  from geopy.geocoders import Nominatim
  geolocator = Nominatim(user_agent= 'youremailhere@gmail.com'
  from geopy.extra.rate_limiter import RateLimiter
  import webbrowser
  ```
  
* In order to geocode a pandas DataFrame with geopy you need to use RateLimiter. Geocode RateLimiter classes provides a convenient wrapper, which can be used to automatically add delays between geocoding calls to reduce the load. RateLimiter allows you perform bulk operations while handling error responses and adding delays to prevent time-outs.
* For Documentation see https://geopy.readthedocs.io/en/stable/

```
  geocode = RateLimiter(geolocator.geocode, min_delay_seconds=1)
 
  # Apply Geocode to the location column
  full['Address'] = full['Location'].apply(geocode)
 
  # Take the data responses from the API call and populate the data in a new DataFrame column called "Address"
  full['point'] = full['Address'].apply(lambda loc: tuple(loc.point) if loc else None)
  full.point
  
* The above code produces the following dataFrame:


