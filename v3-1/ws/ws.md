# WebSocket Streams for BitoPro

* [WebSocket Streams for BitoPro](ws.md#WebSocket-Streams-for-BitoPro)
  * [General WebSocket Information](ws.md#General-WebSocket-Information)
  * [Detailed Stream Information](ws.md#Detailed-Stream-Information)
    * [Order Book](ws.md#Order-Book)
    * [Ticker](ws.md#Ticker)
    * [Trade](ws.md#Trade)

## General WebSocket Information

* The base endpoint is: **wss://stream.bitopro.com:9443/ws**.
* Streams can be access either in `single pair` or `combinded pairs`.
* All pairs for streams are **uppercase** and with **underscore** between base and quote.
* The websocket server will send a `ping frame` every 20 seconds. If the websocket server doesn't receive a `pong frame` back from the client within a 5 seconds period, the connection will be disconnected.
* We'll always push the `latest` data when you successfully get the websocket connection.

## Detailed Stream Information

### Order Book

Order book pushed all data every second when updated. You can specifiy one or more pairs and the default limit is 5. Valid limit values are **1**, **5**, **10** or **20**.

* Request

| Path | Query | Type | Description |
| :--- | :--- | :--- | :--- |
| pair |  | string | Uppercase string literal of a pair, you can optionally a colon with limit. e.g. `BTC_TWD` or `BTC_TWD:5` |
|  | pairs | string | The same with `pair`, multi pairs are comma-delimited. e.g. `BTC_TWD:1,ETH_TWD:20,BITO_ETH` |

* Response

| 0 | Field | Type | Description |
| :--- | :--- | :--- | :--- |
| 0 | event | string | String literal for event name |
| 0 | pair | string | Uppercase string underscore-delimited literal of a pair of currencies |
| 0 | bids | object array |  |
| 0 | asks | object array |  |
| 0 | timestamp | long integer | Unix Timestamp in milliseconds \(seconds \* 1000\) |
| 0 | datetime | string | ISO8601 datetime string with milliseconds |
| 1 | price | string |  |
| 1 | amount | string |  |
| 1 | count | integer |  |
| 1 | total | string | Total amount |

* URL

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/order-books/{pair}**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/order-books/BTC\_TWD { "event": "ORDER\_BOOK", "pair": "BTC\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/order-books?pairs={pair},{pair},...**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/order-books?pairs=BTC\_TWD,ETH\_TWD { "event": "ORDER\_BOOK", "pair": "BTC\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } { "event": "ORDER\_BOOK", "pair": "ETH\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/order-books/{pair}:{limit}**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/order-books/BTC\_TWD:1 { "event": "ORDER\_BOOK", "pair": "BTC\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/order-books?pairs={pair}:{limit},{pair}:{limit},...**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/order-books?pairs=BTC\_TWD:1,ETH\_TWD:1 { "event": "ORDER\_BOOK", "pair": "BTC\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } { "event": "ORDER\_BOOK", "pair": "ETH\_TWD", "bids": \[ { "price": "1", "amount": "1", "count": 1, "total": "1" }, ... \], "asks": \[ { "price": "0.9", "amount": "1", "count": 2, "total": "2" }, ... \], "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

### Ticker

Ticker pushed 24hr rollwing window statistics when updated.

* Request

| Path | Query | Type | Description |
| :--- | :--- | :--- | :--- |
| pair |  | string | Uppercase string literal of a pair. e.g. `BTC_TWD` |
|  | pairs | string | The same with `pair`, multi pairs are comma-delimited. e.g. `BTC_TWD,ETH_TWD,BITO_ETH` |

* Response

| Field | Type | Description |
| :--- | :--- | :--- |
| event | string | String literal for event name |
| pair | string | Uppercase string underscore-delimited literal of a pair of currencies |
| isBuyer | boolean |  |
| priceChange24hr | string |  |
| volume24hr | string |  |
| high24hr | string |  |
| low24hr | string |  |
| timestamp | long integer | Unix Timestamp in milliseconds \(seconds \* 1000\) |
| datetime | string | ISO8601 datetime string with milliseconds |

* URL

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/tickers/{pair}**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/tickers/BTC\_TWD { "event": "TICKER", "pair": "BTC\_TWD", "lastPrice": "1", "isBuyer": true, "priceChange24hr": "1", "volume24hr": "1", "high24hr": "1", "low24hr": "1", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/tickers?pairs={pair},{pair},...**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/tickers?pairs=BTC\_TWD,BITO\_ETH { "event": "TICKER", "pair": "BTC\_TWD", "lastPrice": "1", "isBuyer": true, "priceChange24hr": "1", "volume24hr": "1", "high24hr": "1", "low24hr": "1", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } { "event": "TICKER", "pair": "BITO\_ETH", "lastPrice": "1", "isBuyer": true, "priceChange24hr": "1", "volume24hr": "1", "high24hr": "1", "low24hr": "1", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z" } \`\`\`

### Trade

Trade pushed full data when updated.

* Request

| Path | Query | Type | Description |
| :--- | :--- | :--- | :--- |
| pair |  | string | Uppercase string literal of a pair. e.g. `BTC_TWD` |
|  | pairs | string | The same with `pair`, multi pairs are comma-delimited. e.g. `BTC_TWD,ETH_TWD,BITO_ETH` |

* Response

| \# | Field | Type | Description |
| :--- | :--- | :--- | :--- |
| 0 | event | string | String literal for event name |
| 0 | pair | string | Uppercase string underscore-delimited literal of a pair of currencies |
| 0 | timestamp | long integer | Unix Timestamp in milliseconds \(seconds \* 1000\) |
| 0 | datetime | string | ISO8601 datetime string with milliseconds |
| 0 | data | object array |  |
| 1 | timestamp | long integer | Unix Timestamp in milliseconds |
| 1 | price | string |  |
| 1 | amount | string |  |
| 1 | isBuyer | boolean |  |

* URL

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/trades/{pair}**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/tickers/BTC\_TWD { "event": "TRADE", "pair": "BTC\_TWD", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z", "data": \[ { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :false }, { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :true }, ... \] } \`\`\`

`GET` **wss://stream.bitopro.com:9443/ws/v1/pub/trades?pairs={pair},{pair},...**

Sample \`\`\`json GET wss://stream.bitopro.com:9443/ws/v1/pub/tickers?pairs=BTC\_TWD,BITO\_ETH { "event": "TRADE", "pair": "BTC\_TWD", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z", "data": \[ { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :false }, { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :true }, ... \] } { "event": "TRADE", "pair": "BITO\_ETH", "timestamp": 1136185445000, "datetime": "2006-01-02T15:04:05.700Z", "data": \[ { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :false }, { "timestamp": 1136185445, "price": "1", "amount": "1", "isBuyer" :true }, ... \] } \`\`\`
