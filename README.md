# ![LOGO](logo.png) Adafruit IO **flow**ground Connector

## Description

A generated **flow**ground connector for the Adafruit IO API (version 2.0.0).

Generated from: https://api.apis.guru/v2/specs/adafruit.com/2.0.0/swagger.json<br/>
Generated at: 2019-06-06T16:11:22+03:00

## API Description

### The Internet of Things for Everyone

The Adafruit IO HTTP API provides access to your Adafruit IO data from any programming language or hardware environment that can speak HTTP. The easiest way to get started is with [an Adafruit IO learn guide](https://learn.adafruit.com/series/adafruit-io-basics) and [a simple Internet of Things capable device like the Feather Huzzah](https://www.adafruit.com/product/2821).

This API documentation is hosted on GitHub Pages and is available at [https://github.com/adafruit/io-api](https://github.com/adafruit/io-api). For questions or comments visit the [Adafruit IO Forums](https://forums.adafruit.com/viewforum.php?f=56) or the [adafruit-io channel on the Adafruit Discord server](https://discord.gg/adafruit).

#### Authentication

Authentication for every API request happens through the `X-AIO-Key` header or query parameter and your IO API key. A simple cURL request to get all available feeds for a user with the username "io_username" and the key "io_key_12345" could look like this:

    $ curl -H "X-AIO-Key: io_key_12345" https://io.adafruit.com/api/v2/io_username/feeds

Or like this:

    $ curl "https://io.adafruit.com/api/v2/io_username/feeds?X-AIO-Key=io_key_12345

Using the node.js [request](https://github.com/request/request) library, IO HTTP requests are as easy as:

```js
var request = require('request');

var options = {
  url: 'https://io.adafruit.com/api/v2/io_username/feeds',
  headers: {
    'X-AIO-Key': 'io_key_12345',
    'Content-Type': 'application/json'
  }
};

function callback(error, response, body) {
  if (!error && response.statusCode == 200) {
    var feeds = JSON.parse(body);
    console.log(feeds.length + " FEEDS AVAILABLE");

    feeds.forEach(function (feed) {
      console.log(feed.name, feed.key);
    })
  }
}

request(options, callback);
```

Using the ESP8266 Arduino HTTPClient library, an HTTPS GET request would look like this (replacing `---` with your own values in the appropriate locations):

```arduino
/// based on
/// https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266HTTPClient/examples/Authorization/Authorization.ino

#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <ESP8266WiFiMulti.h>
#include <ESP8266HTTPClient.h>

ESP8266WiFiMulti WiFiMulti;

const char* ssid = "---";
const char* password = "---";

const char* host = "io.adafruit.com";

const char* io_key = "---";
const char* path_with_username = "/api/v2/---/dashboards";

// Use web browser to view and copy
// SHA1 fingerprint of the certificate
const char* fingerprint = "77 00 54 2D DA E7 D8 03 27 31 23 99 EB 27 DB CB A5 4C 57 18";

void setup() {
  Serial.begin(115200);

  for(uint8_t t = 4; t > 0; t--) {
    Serial.printf("[SETUP] WAIT %d...\n", t);
    Serial.flush();
    delay(1000);
  }

  WiFi.mode(WIFI_STA);
  WiFiMulti.addAP(ssid, password);

  // wait for WiFi connection
  while(WiFiMulti.run() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }

  Serial.println("[WIFI] connected!");

  HTTPClient http;

  // start request with URL and TLS cert fingerprint for verification
  http.begin("https://" + String(host) + String(path_with_username), fingerprint);

  // IO API authentication
  http.addHeader("X-AIO-Key", io_key);

  // start connection and send HTTP header
  int httpCode = http.GET();

  // httpCode will be negative on error
  if(httpCode > 0) {
    // HTTP header has been send and Server response header has been handled
    Serial.printf("[HTTP] GET response: %d\n", httpCode);

    // HTTP 200 OK
    if(httpCode == HTTP_CODE_OK) {
      String payload = http.getString();
      Serial.println(payload);
    }

    http.end();
  }
}

void loop() {}
```

#### Client Libraries

We have client libraries to help you get started with your project: [Python](https://github.com/adafruit/io-client-python), [Ruby](https://github.com/adafruit/io-client-ruby), [Arduino C++](https://github.com/adafruit/Adafruit_IO_Arduino), [Javascript](https://github.com/adafruit/adafruit-io-node), and [Go](https://github.com/adafruit/io-client-go) are available. They're all open source, so if they don't already do what you want, you can fork and add any feature you'd like.



## Authorization

Supported authorization schemes:
- API Key- API Key- API Key
## Actions

### Get information about the current user

*Tags:* `Users`

### Send data to a feed via webhook URL.

*Tags:* `Webhooks` `Data`

### Send arbitrary data to a feed via webhook URL.

> The raw data webhook receiver accepts POST requests and stores the raw request body on your feed. This is useful when you don't have control of the webhook sender. If feed history is turned on, payloads will be truncated at 1024 bytes. If feed history is turned off, payloads will be truncated at 100KB.

*Tags:* `Webhooks` `Data`

### All activities for current user

> Delete all your activities.

*Tags:* `Activities`

#### Input Parameters
* `username` - _required_ - a valid username string

### All activities for current user

> The Activities endpoint returns information about the user's activities.

*Tags:* `Activities`

#### Input Parameters
* `username` - _required_ - a valid username string
* `start_time` - _optional_ - Start time for filtering, returns records created after given time.
* `end_time` - _optional_ - End time for filtering, returns records created before give time.
* `limit` - _optional_ - Limit the number of records returned.

### Get activities by type for current user

> The Activities endpoint returns information about the user's activities.

*Tags:* `Activities`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `start_time` - _optional_ - Start time for filtering, returns records created after given time.
* `end_time` - _optional_ - End time for filtering, returns records created before give time.
* `limit` - _optional_ - Limit the number of records returned.

### All dashboards for current user

> The Dashboards endpoint returns information about the user's dashboards.

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string

### Create a new Dashboard

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string

### All blocks for current user

> The Blocks endpoint returns information about the user's blocks.

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_

### Create a new Block

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_

### Delete an existing Block

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_
* `id` - _required_

### Returns Block based on ID

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_
* `id` - _required_

### Update properties of an existing Block

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_
* `id` - _required_

### Replace an existing Block

*Tags:* `Blocks`

#### Input Parameters
* `username` - _required_ - a valid username string
* `dashboard_id` - _required_
* `id` - _required_

### Delete an existing Dashboard

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Returns Dashboard based on ID

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Update properties of an existing Dashboard

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Replace an existing Dashboard

*Tags:* `Dashboards`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### All feeds for current user

> The Feeds endpoint returns information about the user's feeds. The response includes the latest value of each feed, and other metadata about each feed.

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string

### Create a new Feed

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _optional_

### Delete an existing Feed

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Get feed by feed key

> Returns feed based on the feed key

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Update properties of an existing Feed

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Replace an existing Feed

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Get all data for the given feed

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `start_time` - _optional_ - Start time for filtering, returns records created after given time.
* `end_time` - _optional_ - End time for filtering, returns records created before give time.
* `limit` - _optional_ - Limit the number of records returned.
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Create new Data

> Create new data records on the given feed.<br/>
> <br/>
> **NOTE:** when feed history is on, data `value` size is limited to 1KB, when feed history is turned off data value size is limited to 100KB.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Create multiple new Data records

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Chart data for current feed

> The Chart API is what we use on io.adafruit.com to populate charts over varying timespans with a consistent number of data points. The maximum number of points returned is 480. This API works by aggregating slices of time into a single value by averaging.<br/>
> <br/>
> All time-based parameters are optional, if none are given it will default to 1 hour at the finest-grained resolution possible.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `start_time` - _optional_ - Start time for filtering, returns records created after given time.
* `end_time` - _optional_ - End time for filtering, returns records created before give time.
* `resolution` - _optional_ - A resolution size in minutes. By giving a resolution value you will get back grouped data points aggregated over resolution-sized intervals. NOTE: time span is preferred over resolution, so if you request a span of time that includes more than max limit points you may get a larger resolution than you requested. Valid resolutions are 1, 5, 10, 30, 60, and 120.
* `hours` - _optional_ - The number of hours the chart should cover.

### First Data in Queue

> Get the oldest data point in the feed. This request sets the queue pointer to the beginning of the feed.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Last Data in Queue

> Get the most recent data point in the feed. This request sets the queue pointer to the end of the feed.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Next Data in Queue

> Get the next newest data point in the feed. If queue processing hasn't been started, the first data point in the feed will be returned.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Previous Data in Queue

> Get the previously processed data point in the feed. NOTE: this method doesn't move the processing queue pointer.

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Last Data in MQTT CSV format

> Get the most recent data point in the feed in an MQTT compatible CSV format: `value,lat,lon,ele`

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### Delete existing Data

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `id` - _required_

### Returns data based on feed key

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `id` - _required_
* `include` - _optional_ - List of Data record fields to include in response as comma separated list. Acceptable values are: `value`, `lat`, `lon`, `ele`, `id`, and `created_at`. 

### Update properties of existing Data

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `id` - _required_

### Replace existing Data

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key
* `id` - _required_

### Get detailed feed by feed key

> Returns more detailed feed record based on the feed key

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `feed_key` - _required_ - a valid feed key

### All groups for current user

> The Groups endpoint returns information about the user's groups. The response includes the latest value of each feed in the group, and other metadata about the group.

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string

### Create a new Group

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string

### Delete an existing Group

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### Returns Group based on ID

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### Update properties of an existing Group

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### Replace an existing Group

*Tags:* `Groups`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### Add an existing Feed to a Group

*Tags:* `Groups` `Feeds`

#### Input Parameters
* `group_key` - _required_
* `username` - _required_ - a valid username string
* `feed_key` - _optional_

### Create new data for multiple feeds in a group

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### All feeds for current user in a given group

> The Group Feeds endpoint returns information about the user's feeds. The response includes the latest value of each feed, and other metadata about each feed, but only for feeds within the given group.

*Tags:* `Groups` `Feeds`

#### Input Parameters
* `group_key` - _required_
* `username` - _required_ - a valid username string

### Create a new Feed in a Group

*Tags:* `Feeds`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_

### All data for current feed in a specific group

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_
* `feed_key` - _required_ - a valid feed key
* `start_time` - _optional_ - Start time for filtering data. Returns data created after given time.
* `end_time` - _optional_ - End time for filtering data. Returns data created before give time.
* `limit` - _optional_ - Limit the number of records returned.

### Create new Data in a feed belonging to a particular group

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_
* `feed_key` - _required_ - a valid feed key

### Create multiple new Data records in a feed belonging to a particular group

*Tags:* `Data`

#### Input Parameters
* `username` - _required_ - a valid username string
* `group_key` - _required_
* `feed_key` - _required_ - a valid feed key

### Remove a Feed from a Group

*Tags:* `Groups` `Feeds`

#### Input Parameters
* `group_key` - _required_
* `username` - _required_ - a valid username string
* `feed_key` - _optional_

### Get the user's data rate limit and current activity level.

*Tags:* `Users`

#### Input Parameters
* `username` - _required_ - a valid username string

### All tokens for current user

> The Tokens endpoint returns information about the user's tokens.

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string

### Create a new Token

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string

### Delete an existing Token

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Returns Token based on ID

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Update properties of an existing Token

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Replace an existing Token

*Tags:* `Tokens`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### All triggers for current user

> The Triggers endpoint returns information about the user's triggers.

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string

### Create a new Trigger

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string

### Delete an existing Trigger

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Returns Trigger based on ID

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Update properties of an existing Trigger

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### Replace an existing Trigger

*Tags:* `Triggers`

#### Input Parameters
* `username` - _required_ - a valid username string
* `id` - _required_

### All permissions for current user and type

> The Permissions endpoint returns information about the user's permissions.

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_

### Create a new Permission

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_

### Delete an existing Permission

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_
* `id` - _required_

### Returns Permission based on ID

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_
* `id` - _required_

### Update properties of an existing Permission

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_
* `id` - _required_

### Replace an existing Permission

*Tags:* `Permissions`

#### Input Parameters
* `username` - _required_ - a valid username string
* `type` - _required_
* `type_id` - _required_
* `id` - _required_

## License

**flow**ground :- Telekom iPaaS / adafruit-com-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
