DEMO Version 
Docker-compose

How this means to work:
1. Install docker-containers
2. JMeter uses CSV file for IP address InfluxDB
So correct .csv file with correct IP address of InfluxDB
3. Make sure that .csv file is in the same folder with .jmx script
4. Run jmeter script.
5. Check incoming data in InfluxDB
6. Make query from Grafana to InfluxDB and check data in Grafana

You can collect data by grafana from influx with this request:
may be u need to correct a bit, just check in influxDb fields to queue.

from(bucket: "jmeter")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
 
  |> filter(fn: (r) => r["_field"] == "pct95.0")
 
  |> filter(fn: (r) => r["application"] == "InfluxDB 2 integration")
 
  |> filter(fn: (r) => r["statut"] == "ok" or r["statut"] == "ko")
 
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
 
  |> yield(name: "mean")
 
