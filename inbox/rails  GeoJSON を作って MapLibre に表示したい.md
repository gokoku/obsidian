---
type: note
---

#ruby/rails 

---
2023-03-13  13:51

# rails  GeoJSON を作って MapLibre に表示したい

https://gis.stackexchange.com/questions/387954/output-in-geojson-data-format-using-ruby

source
https://github.com/phildionne/geojson_model

これを使ってみようか。

Gemfile
```ruby
gem 'geojson_model'
```

```ruby
    locations = []
    locations << GeojsonModel::Feature.new(
      properties: {},
      geometry: GeojsonModel::Geometry.new(
        type: 'LineString', coordinates: [0,0]
      )
    )
    locations << GeojsonModel::Feature.new(
      properties: {},
      geometry: GeojsonModel::Geometry.new(
        type: 'Point', coordinates: [20, 20]
      )
    )
    locations << GeojsonModel::Feature.new(
      properties: {},
      geometry: GeojsonModel::Geometry.new(
        type: 'Point', coordinates: [-0.001, -0.992]
      )
    )

    fc = GeojsonModel::FeatureCollection.new(features: locations)
    puts(JSON.pretty_generate(JSON.parse(fc.to_json)))
```

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          0,
          0
        ]
      },
      "properties": {
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          20,
          20
        ]
      },
      "properties": {
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -0.001,
          -0.992
        ]
      },
      "properties": {
      }
    }
  ]
}
```

