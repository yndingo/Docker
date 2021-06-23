<p align="center">
  <b>DEMO Version</b></br>
  <b>Docker-compose</b>
</p>

<b>How this means to work:</b>
1. Install docker-containers
2. JMeter uses CSV file for IP address InfluxDB. So correct .csv file with correct IP address of InfluxDB
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
 
<b>Some pictures with tune and results:</b>

![InfluxDB get data from JMeter](Test_Results/1.1_influx_get_data.png?raw=true "InfluxDB get data from JMeter")

![InfluxDB get data from Telegraph](Test_Results/1.2_influx_get_data.png?raw=true "InfluxDB get data from Telegraph")

![Grafana settings №1](Test_Results/2.1_grafana_settings.png?raw=true "Grafana settings №1")

![Grafana settings №2](Test_Results/2.2_grafana_settings.png?raw=true "Grafana settings №2")

![Grafana get data №2](Test_Results/2.3_grafana_get_data.png?raw=true "Grafana get data")
