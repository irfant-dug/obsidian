* Install grafana and Influx oss v1
* sad
```
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8086/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8088/tcp --permanent
sudo firewall-cmd –-reload
```
* Create database and retention policy
```
CREATE DATABASE lustretest
CREATE RETENTION POLICY "90d_policy" ON "lustretest" DURATION 90d REPLICATION 1 DEFAULT
```