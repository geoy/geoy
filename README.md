# Geoy

The point of Geoy is for geo-location resolution and querying.

## Resolution

We should be able to resolve the geo-location from these data points:

1. IP Address
2. Geo-coordinate
3. String (of a city, neighbourhood)
4. Polygon

The idea is that we can transform between any of those four. ie. Having an IP address we can return the city string and polygon. Having a geo-coordinate we can return the city name and neighbourhood (I guess we don't need to return the IP blocks).

## Querying

Ability to store a geo-coordinate data point and then query it by finding it within a city, neighbourhood, from a specific point + radius, or a polygon.

## API Examples

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
  "polygon":[1.0,1.1,1.1,1.2,1.2,1.0]
}
```

Look up by geo-cordinate
```
Request:
GET http://geoy.io/resolve?coord=1.0,1.2

Response:
{
  "city":"Toronto", etc.
}
```

Look up by string
```
Request:
GET http://geoy.io/resolve?name=Toronto

Response:
{
  "city": "Toronto",
  "polygon": [...],
  "center": [1.1,1.2]
}
```

### Data

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
  * whatever makes querying easier.. 4 points will be accurate enough
