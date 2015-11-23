# Geoy

Geoy is a geo-location resolution and querying HTTP service written in Go.

## Resolution

We should be able to resolve the geo-location from these data points:

1. IP Address
2. Geo-point [lat,lng]
3. String (city, state, country)
4. Geo-polygon [<north-point>,<east-point>,<south-point>,<west-point>]

The idea is that we can transform between any of those four data types. ie. From an IP address
we can return the city string and geo-polygon of the city. With a geo-coordinate point we can
return the city string name and polygon of the city boundaries.


## Querying (on roadmap?)

**NOTE: Querying is currently on hold, perhaps Geoy will offer this in future versions, but first
its used to normalize and transform between the different kinds of geo-location types.**

Ability to store a geo-coordinate data point and then query it by finding it within a city,
neighbourhood, from a specific point + radius, or a polygon.


## API

### Resolve

Locate by IP:
```
Request:
GET http://geoy.io/resolve?ip=4.2.2.2

Response:
* json structure of the country, province and city.. as close as we can make it
something like:
{
  "city":"Toronto",
  "state":"Ontario",
  "country":"Canada",
  "region":[[1.0,1.1],[1.1,1.2],[1.2,1.0],[1.3,1.0]]
}
```

Look up by geo-cordinate
```
Request:
GET http://geoy.io/resolve?point=1.0,1.2

Response:
{
  "city":"Toronto",
  "state":"Ontario",
  "country":"Canada",
  "region":[[1.0,1.1],[1.1,1.2],[1.2,1.0],[1.3,1.0]]
}
```

Look up by string
```
Request:
GET http://geoy.io/resolve?name=Toronto

Response:
{
  "city":"Toronto",
  "state":"Ontario",
  "country":"Canada",
  "region":[[1.0,1.1],[1.1,1.2],[1.2,1.0],[1.3,1.0]]
  "center":[1.1,1.2]
}

```

### Data (on roadmap?)

Adding a location
```
Request:
POST http://geoy.io/places
{
    ... suggestions?
}
```

Querying for locations
```
Request:
GET http://geoy.io/places?q=Toronto&at=1.2,1.3,r=1km

Response:
[
  {...}, {...},
]
```


## NOTES

1. Should we have a location id...? tracking a physical place, or a city, etc... (Instagam does this)
2. Should we limit the polygon points to 4 data points, like a bounding box (Twitter does this)
  * whatever makes querying easier.. 4 points will be accurate enough.. 5, 6 points?
  * advantage is we can keep it a simple bounding box for points North-East-South-West
