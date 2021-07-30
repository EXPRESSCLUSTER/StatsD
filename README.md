# StatsD

## Index 
- [Overview](#overview)
- [Prerequisite](#prerequisite)
- [Installing StatsD](#installing-statsd)
- [Enabling EXPRESSCLUSTER to Send Metrics](#enabling-expresscluster-to-send-metrics)

## Overview
- From EXPRESSCLUSTER X 4.3 for Linux (internal version: 4.3.0-1), EXPRESSCLUSTER can send metrics data of monitor resources to StatsD. This article shows how to setup StatsD and EXPRESSCLUSTER.

## Prerequisite
- Create a cluster following the Installation and Configuration Guide.
  - https://www.manuals.nec.co.jp/contents/system/files/nec_manuals/node/540/L43_IG_EN/index.html

## Installing StatsD
1. Install Node.js on the cluster nodes.
1. Clone StatsD repository.
   ```sh
   git clone https://github.com/etsy/statsd.git
   ```
1. Copy exampleConfig.js to localConfig.js and edit as below.
   ```js
   {
     address: "127.0.0.1",
     port: 8125,
     mgmt_address: "127.0.0.1",
     mgmt_port: 8125,
     backends: ["./backends/console"]
   }
   ```
1. Move to stasd directory and run the following command.
   ```sh
   node stats.js localConfig.js
   ```
1. Run the following command for test.
   ```sh
   echo "foo:1|c" | nc -w 1 -u 127.0.0.1 8125
   ```
1. You can see the following output on the console.
   ```
   Flushing stats at  Fri Apr 03 2020 15:18:13 GMT+0900 (Japan Standard Time)
   {
     counters: {
       'statsd.bad_lines_seen': 0,
       'statsd.packets_received': 1,
       'statsd.metrics_received': 1,
       foo: 1
     },
     timers: {},
     gauges: { 'statsd.timestamp_lag': 0 },
     timer_data: {},
     counter_rates: {
       'statsd.bad_lines_seen': 0,
       'statsd.packets_received': 0.1,
       'statsd.metrics_received': 0.1,
       foo: 0.1
     },
     sets: {},
     pctThreshold: [ 90 ]
   }   
   ```
## Enabling EXPRESSCLUSTER to Send Metrics
1. Create a directory and move there.
   ```sh
   mkdir tmp
   cd tmp
   ```
1. Get the current cluster configuration.
   ```sh
   clpcfctrl --pull -l -x .
   ```
1. Run the following commands.
   ```sh
   clpcfset add clsparam statsd/mode 1clpcfset add clsparam statsd/server/host localhost
   clpcfset add clsparam statsd/server/port 8125
   ```
1. If you install xmllint, run the following command.
   ```sh
   xmllint --format --output clp.conf clp.conf
   ```
1. Run the following command.
   ```sh
   clpcl --suspend
   clpcfctrl --push -l -x .
   clpcl --resume
   ```