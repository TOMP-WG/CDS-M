# TOMP-CDS
City Data Specification

## General idea
We have to transmit a lot of information from transport operators (TO) to cities. Therefore we created this API. The TO can request the meta data from the city endpoint, so it can create a file with the requested data.

## Meta data
The meta data exists out of 3 parts: 1) the area's and 2) the dimensions 3) the measurements.

### Areas
the Area's are normal geojson features, containing (multi)polygons. For future usage we added also points (eHubs) and lines (links).

### Dimensions
The dimensions are the where the measurements are aggregated on. These should be in fixed domains, so they are clearly specified. An example (the city dimension):
<pre>
{
			"type": "Feature",
			"geometry": null,
			"properties": {
				"type": "Dimension",
				"name": "city",
				"description": "string",
				"dimensionElements": [
					"Amsterdam",
					"Eindhoven"
				]
			}
		}
</pre>
### Measurements
Now we have to specify the measured, aggregated values. Therefore a simple array should be sufficient. We expect that all measurements will be numbers.
<pre>
{
			"type": "Feature",
			"geometry": null,
			"properties": {
				"measumements": [
					"total_movements",
					"unique_users",
					"total_km",
					"total_duration_minutes",
					"total_co2",
					"total_pm10",
					"total_nox",
					"total_number_of_stops",
					"total_non_moving_assets",
					"total_parking_time",
					"total_charging_minutes",
					"total_out_of_geofence"
				]
			}
		}
</pre>
This array can be extended later on.

## Aggregated trip data
And finally the actual data, containing all dimensions and aggregated values per origin-destination combination.
<pre>
{
			"type": "Feature",
			"geometry": null,
			"properties": {
				"type": "TabularDatasetRow",
				"tabularDatasetId": 1,
				"city": "Amsterdam",
				"origin": 1,
				"destination": 1,
				"mode": "bike",
				"year": 2020,
				"month": 3,
				"day": 12,
				"hour": 8,
				"day_of_week": 1,
				"week": 11,
				"total_movements": 18,
				"unique_users": 16,
				"total_km": 68,
				"total_duration_minutes": 324,
				"total_co2": 392,
				"total_pm10": 0,
				"total_nox": 0,
				"total_number_of_stops": 58,
				"total_out_of_geofence": 3
			}
		}
</pre>

To communicate data about 1 specific area (f.i. total parking time), the destination should be -1.
<pre>
{
			"type": "Feature",
			"geometry": null,
			"properties": {
				"type": "TabularDatasetRow",
				"tabularDatasetId": 1,
				"city": "Amsterdam",
				"origin": 1,
				"destination": 1,
				"mode": "bike",
				"year": 2020,
				"month": 3,
				"day": 12,
				"hour": 8,
				"day_of_week": 1,
				"week": 11,
				"total_non_moving_assets": 3,
				"total_parking_time": 38,
				"total_charging_minutes": 44
			}
		}
</pre>