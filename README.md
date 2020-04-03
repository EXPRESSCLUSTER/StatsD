# StatsD

## Setup StatsD
1. Install Node.js
   ```sh
   # curl -sL https://rpm.nodesource.com/setup_13.x | bash -
   # yum install -y nodejs
   # node -v
   v13.12.0
   # npm -v
   6.14.4
   ```
1. Download StatsD.
   ```sh
   # git clone https://github.com/etsy/statsd.git
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
   # node stats.js localConfig.js
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
