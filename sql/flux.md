## InfluxQL to Flux
sin/cosなどの三角関数を使いたいが、fluxにしかない

https://www.influxdata.com/blog/tldr-influxdb-tech-tips-converting-influxql-queries-flux-queries/

```sql
SELECT "fieldKey1","fieldKey2" FROM "db"."measurement1" WHERE time >= '2022-02-22T20:22:02Z' and time < now() GROUP BY <tagKey1>
```

```
from(bucket: "db")
|> range(start: "2022-02-22T20:22:02Z", stop: now())
|> filter(fn: (r) => r._measurement == "measurement1")
|> filter(fn: (r) => r._field == "fieldKey1" or r._field == "fieldKey2")
|> yield(name: "my first flux query")
```
