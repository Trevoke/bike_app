#peddlr

peddlr helps you plan your Citi Bike trips around New York City.

#Description

Citi Bike is a system of publically available bikes placed at stations throughout New York City. But planning a trip using Citi Bike can be complicated. Where is the closest station? Is it active? Does it have any bikes availabe?

peddlr helps you answer these questions and allows you to plan you trips around the city. Users plan a trip by entering origin and destination addresses then peddlr  looks up the nearest Citi Bike station with available bikes and provides usres with a map to the station. peddlr also directs you to the Citi Bike station nearest your destination. In addition, peddlr allows you to look at past trips you've gone on.

Take the confusion out of Citi Bike. User peddlr.

#Screenshots

![screenshot](https://dl.dropboxusercontent.com/u/1361162/Screen%20Shot%202014-05-22%20at%209.12.07%20AMEDT.jpg)

![screenshot](https://dl.dropboxusercontent.com/u/1361162/Screen%20Shot%202014-05-22%20at%204.21.09%20PMEDT.jpg)

#Code Sample

```ruby

def self.nearest_station(address_coordinates)
  all_stations = Citibikenyc.branches["results"]
  station_array = []

  all_stations.each do |station|

    distance_from_address = Geocoder::Calculations.distance_between(address_coordinates, [(station['latitude']),(station['longitude'])])

    station_info = { 
      :station_id => (station['id']), 
      :label => (station['label']), 
      :latitude => (station['latitude']),
      :longitude => (station['longitude']),
      :distance => distance_from_address 
    }

    station_array << station_info
  end

  sorted = station_array.sort{ |x, y| x[:distance] <=> y[:distance] }

  sorted.each do |station|
    return station if station_state(station[:station_id]).fetch("status", "not active") == "Active" &&  station_state(station[:station_id]).fetch("availableBikes", 0).to_i > 0
  end

end


```

###RESOURCES

#### Trello

- https://trello.com/b/Q5YkldhD/bike-app

#### Heroku

- http://thawing-atoll-1044.herokuapp.com/

#### Citi Bike System Data

- http://www.citibikenyc.com/system-data

#### Citibikenyc gem

- gets realtime data on Citi Bike stations throughout the city

- https://github.com/edgar/citibikenyc

- Get a list of all stations
-- Citibikenyc.stations

- Get only the current status for all stations
-- Citibikenyc.stations_status

- Get helmets info
-- Citibikenyc.helmets

- Get branches info
-- Citibikenyc.branches

#### Geocoder gem

- http://www.rubygeocoder.com/
- http://rdoc.info/github/alexreisner/geocoder/master/frames

- uses Google Maps API to allow for various forms of spatial analysis, including distance calculations between two location and geocoding street address to lat/long

##### Useful commands for Geocoder

- look up coordinates of some location (like searching Google Maps)

```ruby
Geocoder.coordinates("25 Main St, Cooperstown, NY")
 => [42.700149, -74.922767]
 ```

- distance (in miles) between Eiffel Tower and Empire State Building

```ruby
Geocoder::Calculations.distance_between([47.858205,2.294359], [40.748433,-73.985655])
 => 3619.77359999382
```



