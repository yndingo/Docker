DEMO Version 
Docker-compose

You can collect data by grafana from influx with this request:

from(bucket: "jmeter")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
 
  |> filter(fn: (r) => r["_field"] == "pct95.0")
 
  |> filter(fn: (r) => r["application"] == "InfluxDB 2 integration")
 
  |> filter(fn: (r) => r["statut"] == "ok")
 
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
 
  |> yield(name: "mean")
 
