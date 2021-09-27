# influxdb_raspberry_pi
Workaround for starting influxdb on a Raspberry Pi

The last few updates to influxdb have had a deliterious effect on operations on my Raspberry Pi 3B.
Two things have determined this workaround:
 1. The provided startup system is complex with apparent interactions between `influxdb.service` and `init.sh`.
 2. Just running `influxd` on the command line results in the database working as expected.

I don't doubt that this complexity is useful in some installations, but is not necessary in mine. Using
`systemd` to start a simple daemon will suffice. Below is the `influxdb.service` file I'm using to work
around the issue. YMMV.

Some original lines are commented out, some of those are replaced by different versions.

```
# If you modify this, please also make sure to edit init.sh

[Unit]
Description=InfluxDB is an open-source, distributed, time series database
Documentation=https://docs.influxdata.com/influxdb/
After=network-online.target

[Service]
User=influxdb
Group=influxdb
#LimitNOFILE=65536
EnvironmentFile=-/etc/default/influxdb
#ExecStart=/usr/lib/influxdb/scripts/influxd-systemd-start.sh
ExecStart=/usr/bin/influxd
#KillMode=control-group
Restart=on-failure
#Type=forking
Type=simple
#PIDFile=/var/lib/influxdb/influxd.pid

[Install]
WantedBy=multi-user.target
Alias=influxd.service
```
