# Base Info

## The base url
Name | base endpoint
------------ | ------------
market-api | `https://api-future.trubit.com/market/api/v1`
trade-api | `https://api-future.trubit.com/trade/api/v1`
market-websocket | `wss://api-future.trubit.com/ws/market`
trade-websocket | `wss://api-future.trubit.com/ws/trade`

## Time zone
The time zone for all times is UTC+0

## WebSocket Overview
WebSocket is a new protocol for HTML5, which enables full-duplex communication between the client and the server, allowing data to travel in both directions quickly. The client and server connections can be established through a simple handshake, and the server can actively push information to the client according to the business rules. The advantages are as follows:

- When the client and the server perform data transmission, the request header information is relatively small, about 2 bytes.
- Both the client and the server can actively send data to the other party.
- There is no need to create TCP requests and destroy multiple times, saving bandwidth and server resources.

# Market Endpoint
## `Contract info`
Exchange trading rules and symbol information
### **Limit:**
Speed limit: 1 times/second, Speed limit rule: IP
### **Request Url:**
```bash
GET /basic/refData
```

### **Parameters：**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ----
------------ | ------------ | ------------ | ------------ | ----

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`symbol`|string|`XRPUSDT`|Contract name
`lotSize`|float|`1.0`|  Defines the minimum quantity allowed
`tick`|float|`0.00001`| Minimal price value
`type`|string|`USDT`| Contract type, PERP => Perpetual Contract.
```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "lotSize": 1.0,
      "symbol": "MASKUSDT",
      "tick": 0.001,
      "type": "PERP"
    }
  ]
}
```

## `Klines`

Obtain K line by from and step

### **Limit:**
Speed limit: 15 times/1min, Speed limit rule: IP

### **Request URL:**
```
GET /kLine/byFrom
```
### **Parameters：**
| Name|Type|Required|Default|Description |
| ------------ | ------------ | ------------ | ------------ | ---- |
|`symbol`|string|`YES`|`--`| The contract symbol
|`type`|string|`YES`|`--`| Interval of the kline. Identifiable values: 1M, 3M, 5M, 10M, 15M, 30M, 1H, 2H, 4H, 6H, 8H, 12H, D, 3D, W, MTH
|`from`|integer|`NO`|`0`| Start from of K line, 0 means from the latest one
|`step`|integer|`NO`|`--`|The number of K line, equal or less than 1500

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`[0]`|string|`26435.5`|Close Price
`[1]`|string|`26435.5`|High Price
`[2]`|string|`1686905040`|*1000 timestamp(ms)
`[3]`|string|`26435.5`|Low Price
`[4]`|string|`26435.5`|Open Price
`[6]`|string|`7.08`|Volume eg:BTCUSDT The unit is BTC
`[7]`|string|`181074.0`|Turnover eg:BTCUSDT The unit is USDT
### **Example:**
```json
{
    "code": 0,
        "message": "OK",
        "result": [
        [
            "25559.4",
            "25559.7",
            "1686905040",
            "25553.2",
            "25554.1",
            "1",
            "7.08",
            "181074.0"
        ]
    ]
}
```
## `Klines By Time`

Obtain K line by time and step

### **Limit:**
Speed limit: 15 times/1min, Speed limit rule: IP

### **Request URL:**
```
GET /kLine/byTime
```

### **Parameters：**
| Name|Type|Required|Default|Description |
| ------------ | ------------ | ------------ | ------------ | ---- |
|`symbol`|string|`YES`|`--`|The contract symbol
|`type`|string|`YES`|`--`|Interval of the kline. Identifiable values: 1M, 3M, 5M, 10M, 15M, 30M, 1H, 2H, 4H, 6H, 8H, 12H, D, 3D, W, MTH
|`from`|integer|`NO`|`currentTimeMillis`|Milli seconds of K line keyTime, query from the latest one if not provided
|`step`|integer|`YES`|`--`|The number of K line, equal or less than 1500

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`[0]`|string|`26435.5`|Close Price
`[1]`|string|`26435.5`|High Price
`[2]`|string|`1686905040`|*1000 timestamp(ms)
`[3]`|string|`26435.5`|Low Price
`[4]`|string|`26435.5`|Open Price
`[6]`|string|`7.08`|Volume eg:BTCUSDT The unit is BTC
`[7]`|string|`181074.0`|Turnover eg:BTCUSDT The unit is USDT

### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        [
            "25559.4",
            "25559.7",
            "1686905040",
            "25553.2",
            "25554.1",
            "1",
            "7.08",
            "181074.0"
        ]
    ]
}
```
## `Index`

Index price of underlying asset

### **Limit:**
Speed limit: 10 times/second, Speed limit rule: IP

### **Request Url:**
```bash
GET /basic/indexPrice
```
### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ----
symbols|string|`NO`|`--`| The contract symbol, multiple symbols separated by ',', eg: BTCUSDT,ETHUSDT

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`symbol`|string|`BTCUSDT`|The contract symbol
`price`|float|`8342.73`|The index price of the underlying
`time`|long|`1686196441142`|The time of the underlying

### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "symbol": "BTCUSDT",
            "price": 26442.32666666667,
            "time": 1686209050099
        }
    ]
}
```
## `Last Price`

Query last price of instrument(s)

### **Limit:**
Speed limit: 1 times/second, Speed limit rule: IP

### **Request Url:**
```bash
GET /basic/lastPrice
```
### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ----
symbols|string|`NO`|`--`| The contract symbol, multiple symbols separated by ',', eg: BTCUSDT,ETHUSDT

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`symbol`|string|`BTCUSDT`|The contract symbol
`price`|float|`8342.73`|The last price
`time`|long|`1686196441142`|The time

### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "price": 26627.7,
            "symbol": "BTCUSDT",
            "time": 1686304783617
        }
    ]
}
```
## `Mark Price`

Query mark price of instrument(s)

### **Limit:**
Speed limit: 10 times/second, Speed limit rule: IP

### **Request Url:**
```bash
GET /basic/markPrice
```
### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ----
symbols|string|`NO`| The contract symbol, multiple symbols separated by ',', eg: BTCUSDT,ETHUSDT

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`symbol`|string|`BTCUSDT`|The contract symbol
`price`|float|`8342.73`|The mark price
`time`|long|`1686196441142`|The time

### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "price": 26627.7,
            "symbol": "BTCUSDT",
            "time": 1686304783617
        }
    ]
}
```
## `Funding Rate`

Obtain funding rate of instrument

### **Limit:**
Speed limit: 5 times/second, Speed limit rule: IP

### **Request Url:**
```bash
GET /kLine/fundingRate
```
### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ----
symbols|string|`NO`| The contract symbol, multiple symbols separated by ',', eg: BTCUSDT,ETHUSDT

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`symbol`|string|`BTCUSDT`|The contract symbol
`rate`|double|`0.0001`|Funding rate
`date`|string|`2023-06-08T07:00:00.000+00:00`|Time
`time`|long|`1686196441142`|Timestamp

### **Example:**
```json
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "symbol": "BTCUSDT",
            "rate": 0.0001,
            "date": "2023-06-08T08:00:00.000+00:00",
            "timestamp": 1686207600020
        }
    ]
}
```

## `Depth`

Query market depth Snapshot & trades of instrument

### **Limit:**
Speed limit: 10 times/second, Speed limit rule: IP

### **Request Url:**
```
GET /depth/list
```

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`symbol`|string|`YES`|`--`|The contract symbol to be retrieved
`level`|integer|`NO`|`20` |The number(5/10/20) of entries returned for bids and asks

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`price`|double|26418.5| Price
`qty`|double|18873.0| Positive integer, 1Qty=1USDT
`count`|int|1|Order count
`iceCount`|int|1|Ice Count

### **Example:**

```js
{
    "code": 0,
        "message": "OK",
        "result": {
        "buyDepth": [
            {
                "price": 26418.5,
                "qty": 50056.0,
                "count": 6,
                "iceCount": 0
            },
            {
                "price": 26418.4,
                "qty": 80489.0,
                "count": 6,
                "iceCount": 0
            },
            {
                "price": 26418.1,
                "qty": 58500.0,
                "count": 4,
                "iceCount": 0
            }
        ],
            "sellDepth": [
            {
                "price": 26418.8,
                "qty": 7797.0,
                "count": 2,
                "iceCount": 0
            },
            {
                "price": 26418.9,
                "qty": 9891.0,
                "count": 2,
                "iceCount": 0
            },
            {
                "price": 26419.0,
                "qty": 17483.0,
                "count": 5,
                "iceCount": 0
            },
            {
                "price": 26419.1,
                "qty": 19006.0,
                "count": 6,
                "iceCount": 0
            },
            {
                "price": 26419.2,
                "qty": 17266.0,
                "count": 6,
                "iceCount": 0
            }
        ],
            "trades": null
    }
}
```

## `trades`

Retrieve the latest trades that have occurred for a specific contract.

### **Limit:**
Speed limit: 10 times/second, Speed limit rule: IP

### **Request URL:**
```
GET /depth/trades
```
### **Parameters：**
Parameter|type|required|default|description
------------ | ------------ | ------------ | ------------ | ------
`symbol`|string|`YES`||The contract symbol
`sequence`|string|`NO` |`--`|Sequence of last trade id, retrieve all trades if value is null

### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`1686213904404000035`|Trade id
`symbol`|string|`BTCUSDT`|The contract symbol
`price`|double|`1295.0`|Price
`qty`|double|`1295.0`|The quantity traded
`buyActive`|boolean|`true`| True is the buyer's active order, false is the seller's active order
`tms`|long|`1537797044116`|timestamp (ms)


### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "id": "1686213904404000035",
            "symbol": "BTCUSDT",
            "price": 26428.9,
            "qty": 1295.0,
            "buyActive": false,
            "timestamp": "2023-06-08T08:45:04.404+00:00",
            "tms": "1686213904404"
        },
        {
            "id": "1686213899234000003",
            "symbol": "BTCUSDT",
            "price": 26438.2,
            "qty": 2000.0,
            "buyActive": false,
            "timestamp": "2023-06-08T08:44:59.234+00:00",
            "tms": "1686213899234"
        },
        {
            "id": "1686213884863000003",
            "symbol": "BTCUSDT",
            "price": 26433.3,
            "qty": 9100.0,
            "buyActive": true,
            "timestamp": "2023-06-08T08:44:44.863+00:00",
            "tms": "1686213884863"
        }
    ]
}
```

## `Trade Statistics in latest 24 hours`

Trade Statistics in latest 24 hours

### **Limit:**
Speed limit: 10 times/second, Speed limit rule: IP

### **Request URL:**
```
GET /kLine/tradeStatistics
```

### **Parameters：**
| Name|Type|Required|Default|Description |
| ------------ | ------------ | ------------ | ------------ | ---- |
|`symbols`|string|`YES`|`--`| The contract symbol, multiple symbols separated by ',', eg: BTCUSDT,ETHUSDT

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`'symbol'`|string|`BTCUSDT`|The contract symbol
`'lastPrice'`|double|`BTCUSDT`|The contract symbol
`'maxPrice'`|double|`26420.9`|Max price
`'minPrice'`|double|`25713.4`|Min price
`'priceChange'`|double|`-142.0`|Price change
`'priceChangeRatio'`|double|`-0.005437842019200944`|Price change ratio
`'volume'`|double|`6.5697459E7`| eg:BTCUSDT The unit is BTC
`'turnover'`|double|`2483.5865212475437`|eg:BTCUSDT The unit is USDT
### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "lastPrice": 25971.3,
            "maxPrice": 26420.9,
            "minPrice": 25713.4,
            "priceChange": -142.0,
            "priceChangeRatio": -0.005437842019200944,
            "symbol": "BTCUSDT",
            "turnover": 16171.504782381482,
            "volume": 4.19809141E8
        }
    ]
}
```
## `Open Position In Exchange`

Total Open Position In Exchange

### **Limit:**
Speed limit: 5 times/second, Speed limit rule: IP

### **Request URL:**
```
GET /kLine/openInterest
```

### **Parameters：**
| Name|Type|Required|Default|Description |
| ------------ | ------------ | ------------ | ------------ | ---- |
|`symbol`|string|`YES`|`--`|The contract symbol

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`'symbol'`|string|`BTCUSDT`|The contract symbol
`'date'`|double|`2023-06-19T02:37:53.617+00:00`|Time
`'qty'`|double|`1084388.0`| eg:BTCUSDT The unit is USDT
`'value'`|double|`41.035743951380205`|eg:BTCUSDT The unit is BTC
### **Example:**
```js
{
    "code": 0,
        "message": "OK",
        "result": {
        "symbol": "BTCUSDT",
            "value": 41.035743951380205,
            "date": "2023-06-19T02:37:53.617+00:00",
            "qty": 1084388.0
    }
}
```
# Public Websocket Endpoints

##  Receiving data requires an event subscription
## `Kline`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`| subscribe/unsubscribe
`key`|string|`YES`|`--`| The contract symbol
`type`|string|`YES`|`--`| 1M, 3M, 5M, 10M, 15M, 30M, 1H, 2H, 4H, 6H, 8H, 12H, D, 3D, W, MTH
`channel`|string|`YES`|`--`| Channel
### **Example:**
```json
{
  "op": "subscribe",
  "key": "BTCUSDT",
  "type": "1M",
  "channel": "kLine"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`[0]`|string|`26435.5`|Close Price
`[1]`|string|`26435.5`|High Price
`[2]`|string|`1686905040`|*1000 timestamp(ms)
`[3]`|string|`26435.5`|Low Price
`[4]`|string|`26435.5`|Open Price
`[6]`|string|`7.08`|Volume eg:BTCUSDT The unit is BTC
`[7]`|string|`181074.0`|Turnover eg:BTCUSDT The unit is USDT
### **Example:**
```js
{
    "hisPrice": [
    "25557.4",
    "25557.7",
    "1686905100",
    "25557.4",
    "25557.5",
    "1",
    "0.69",
    "17681.0"
],
    "isNew": false,
    "key": "MEXO_BTCUSDT_1M"
}
```
## `Depth`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`| subscribe/unsubscribe
`key`|string|`YES`|`--`| The contract symbol
`channel`|string|`YES`|`--`| Channel
### **Example:**
```json
{
  "op": "subscribe",
  "key": "BTCUSDT",
  "channel": "depthUpdate"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`price`|double|26418.5| Price
`qty`|double|18873.0| Positive integer, 1Qty=1USDT
`count`|int|1|Order count
`iceCount`|int|1|Ice Count
### **Example:**

```json
{
  "buyDepth": [
    {
      "price": 25916.1,
      "qty": 26144.0,
      "count": 4,
      "iceCount": 0
    },
    {
      "price": 25915.9,
      "qty": 82419.0,
      "count": 7,
      "iceCount": 0
    }
  ],
  "sellDepth": [
    {
      "price": 25916.4,
      "qty": 6233.0,
      "count": 2,
      "iceCount": 0
    },
    {
      "price": 25917.3,
      "qty": 33746.0,
      "count": 5,
      "iceCount": 0
    }
  ],
  "trades": [
    {
      "id": "1686733223325000025",
      "symbol": "BTCUSDT",
      "price": 25916.4,
      "qty": 3648.0,
      "buyActive": true,
      "timestamp": "Jun 14, 2023 09:00:23 AM",
      "tms": "1686733223325"
    },
    {
      "id": "1686733223325000023",
      "symbol": "BTCUSDT",
      "price": 25916.4,
      "qty": 4289.0,
      "buyActive": true,
      "timestamp": "Jun 14, 2023 09:00:23 AM",
      "tms": "1686733223325"
    }

  ],
  "key": "BTCUSDT"
}
```
## `Trade Statistics`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`| subscribe/unsubscribe
`key`|string|`YES`|`--`| The contract symbol
`channel`|string|`YES`|`--`| Channel
### **Example:**
```json
{
  "op": "subscribe",
  "key": "BTCUSDT",
  "channel": "tradeStatistics"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`'symbol'`|string|`BTCUSDT`|The contract symbol
`'lastPrice'`|double|`BTCUSDT`|The contract symbol
`'maxPrice'`|double|`26420.9`|Max price
`'minPrice'`|double|`25713.4`|Min price
`'priceChange'`|double|`-142.0`|Price change
`'priceChangeRatio'`|double|`-0.005437842019200944`|Price change ratio
`'volume'`|double|`6.5697459E7`| eg:BTCUSDT The unit is BTC
`'turnover'`|double|`2483.5865212475437`|eg:BTCUSDT The unit is USDT
### **Example:**

```json
{
  "tradeStatistics": {
    "symbol": "BTCUSDT",
    "maxPrice": 26081.8,
    "minPrice": 24800.2,
    "priceChange": -919.1000000000022,
    "priceChangeRatio": -0.03533002494743365,
    "volume": 4.55661746E8,
    "turnover": 17834.7449180242,
    "lastPrice": 25095.6
  },
  "key": "BTCUSDT"
}
```

## `Open Interest`

Total Open Position In Exchange

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`| subscribe/unsubscribe
`key`|string|`YES`|`--`| The contract symbol
`channel`|string|`YES`|`--`| Channel
### **Example:**
```json
{
  "op": "subscribe",
  "key": "BTCUSDT",
  "channel": "openInterest"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`'key'`|string|`BTCUSDT`|The contract symbol
`'date'`|double|`2023-06-19T02:37:53.617+00:00`|Time
`'qty'`|double|`1084388.0`| eg:BTCUSDT The unit is USDT
`'value'`|double|`41.035743951380205`|eg:BTCUSDT The unit is BTC

### **Example:**

```json
{
  "key": "BTCUSDT",
  "event": "openInterest",
  "value": 45.36721464659362,
  "date": "Jun 19, 2023 03:06:10 AM",
  "qty": 1199212.0
}
```

# Trade Http Endpoints

Trade Endpoints Check signature. Check parameters are passed in header

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`apiKey`|string|`YES`|`--`| Apply at the individual center, apikey/apiSecret
`signature`|string|`YES`|`--`| signature=sha256(secret+method+path+expires)Note: The + needs to be removed to generate a signature
`expires`|long|`YES`|`--`| Custom UTC millisecond timestamp, if the request service time is greater than expires, a timeout will be returned
## `Order`

Place order for a contract. This API endpoint requires your signed request.

### **Request Weight:**

Speed limit: 20 times/second Speed limit rule: apiKey

### **Request URL:**
```bash
POST /trade/enterOrder
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`symbol`|string|`YES`|`--`|The contract symbol, eg: BTCUSDT
`side`|string|`YES`|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`orderType`|string|`YES`|`--`|The order type, includes: `LIMIT`, `Market`
`currency `|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT
`qty`|double|`YES`|`0`|The number of contracts to buy, USDT units to calculate the quantity
`openPosition `|boolean|`YES`|`true`|Open position or Close position order, eg: true or false
`price`|double|`NO`. **REQUIRED** for (`LIMIT`) orders|`--`|Order price
`triggerPrice`|double|`NO`|`--`|Trigger price for conditional order
`trailingStop`|double|`NO`|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`stopLossPrice`|double|`NO`|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`NO`|`--`|Stop win price for advanced order
`tif`|string|`NO`|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`triggerType`|string|`NO`|`--`|Trigger type of price for conditional order, eg: LAST or INDEX, it will be LAST if not providedAvailable values : INDEX, LAST, MARK
`stopWinType`|string|`NO`|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`clientOrderId`|string|`NO`|`--`|An unique ID for the order (user defined), Must satisfy regular: ^[\\.A-Z\\:/a-z0-9_-]{1,36}$"

**NOTE** For **Market Orders**, you need to set `orderType` as **`MARKET`**.

You can get contract price and quantity precision configuration data in the `exchange` endpoint.

Note: if your balance does not meet the margin requirement (which is the minimum margin requirement + open position fee + close position fee), "*insufficient balance*" error message will be returned.


### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`created`|long|`1570759718825`|Timestamp when the order is created
`modified`|long|`1551062936784`|Timestamp when this order was updated last time
`id`|string|`O101-20230531-025722-174-1016`|The order ID
`clientOrderId`|string|`213443`|An unique order ID which is defined by user
`symbol`|string|`BTC-PERP-REV`|The contract symbol
`price`|float|`8200`|The order price
`qty`|float|`1.01`|Order quantity
`cumQty`|float|`1.01`|The number of orders that has been executed
`avgPx`|float|`4754.24`|Average price of filled orders
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`type`|string|`--`|The order type, includes: `LIMIT`, `Market`
`tif`|string|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`fee`|double|`--`|Fees incurred for this order.

```json
{
  "code": 0,
  "message": "OK",
  "result": {
    "ordStatus": "PENDING_NEW",
    "symbol": "BTCUSDT",
    "clientOrderId": "213443",
    "showQty": 0.0,
    "side": "Sell",
    "created": 1685699501000,
    "fee": 0.0,
    "cumQty": 0.0,
    "source": "Normal",
    "type": "Market",
    "pnl": 0.0,
    "tif": "GOOD_TILL_CANCEL",
    "openPosition": true,
    "uid": "1323056886107491328",
    "avgPx": 0.0,
    "price": 0.0,
    "qty": 100.0,
    "iceberg": false,
    "modified": 1685699501000,
    "currency": "BTC",
    "id": "O101-20230602-095141-711-1878"
  }
}
```
## `Query Order`

Request order by id

### **Limit:**

Speed limit: 20 times/second, Speed limit rule: apiKey

### **Request URL:**
```bash
GET /trade/queryOrderById
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`orderId`|string|`O101-20230531-025722-174-1016`|`NO`|The order ID
`clientOrderId`|string|`O101-20230531-025722-174-1016`|`NO`|An unique order ID which is defined by user

**NOTE** orderId and clientOrderId must pick one of two
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`O101-20230531-025722-174-1016`|The order ID
`uid`|string|`1323056886107491328`|The User ID
`symbol`|string|`BTCUSDT`|The contract symbol
`created`|long|`1570759718825`|Timestamp when the order is created
`modified`|long|`1551062936784`|Timestamp when this order was updated last time
`clientOrderId`|string|`213443`|An unique order ID which is defined by user
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`type`|string|`--`|The order type, includes: `LIMIT`, `Market`
`tif`|string|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`price`|double|`8200`|The order price
`qty`|double|`11.0`|Order quantity
`openPosition`|boolean|`true`|Open position or Close position order, eg: true or false
`ordStatus`|boolean|`true`|Open position or Close position order, eg: true or false
`cumQty`|double|`11.0`|The number of orders that has been executed
`avgPx`|double|`4754.24`|Average price of filled orders
`ordStatus`|string|`FILLED`|The order status, includes: `PENDING_NEW`, `NEW`, `PARTIALLY_FILLED`, `FILLED`, `PENDING_CANCEL`, `CANCELED`, `REJECTED`, `PENDING_REPLACE`, `REPLACED`, `WAITING`
`iceberg`|boolean|`false`|The order is iceberg
`showQty`|double|`1.0`|The order showQty
`pnl`|double|`1.1`|Pnl incurred for this order.
`fee`|double|`1.1`|Fees incurred for this order.
`triggerPrice`|double|`--`|Trigger price for conditional order
`triggerType`|string|`--`|Trigger type of price for conditional order, eg: LAST or INDEX, it will be LAST if not providedAvailable values : INDEX, LAST, MARK
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
```json
{
  "code": 0,
  "message": "OK",
  "result": {
    "id": "O101-20230531-040439-546-1837",
    "uid": "1323056886107491328",
    "symbol": "BTCUSDT",
    "created": 1685505879000,
    "modified": 1685505879000,
    "side": "Buy",
    "type": "Market",
    "tif": "GOOD_TILL_CANCEL",
    "currency": "BTC",
    "price": 0.0,
    "qty": 100.0,
    "openPosition": false,
    "cumQty": 100.0,
    "avgPx": 27620.2,
    "ordStatus": "FILLED",
    "iceberg": false,
    "showQty": 0.0,
    "source": "Normal",
    "pnl": 1.3365529862569942E-6,
    "fee": -1.448215436528338E-6,
    "triggerPrice": null,
    "triggerType": null,
    "stopLossPrice": null,
    "stopWinType": null,
    "stopWinPrice": null,
    "trailingStop": null
  }
}
```
## `Query Orders`

Request order by time

### **Limit:**

Speed limit: 10 times /1 minutes, Speed limit rule: apiKey, Response data limitation: 300

### **Request URL:**
```bash
GET /trade/queryOrders
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`prevTimeMillis`|long|`1685505879000`|`NO`|The order created, Query the data from the prevTimeMillis to the current time

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`O101-20230531-025722-174-1016`|The order ID
`uid`|string|`1323056886107491328`|The User ID
`symbol`|string|`BTCUSDT`|The contract symbol
`created`|long|`1570759718825`|Timestamp when the order is created
`modified`|long|`1551062936784`|Timestamp when this order was updated last time
`clientOrderId`|string|`213443`|An unique order ID which  is defined by user
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`type`|string|`--`|The order type, includes: `LIMIT`, `Market`
`tif`|string|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`price`|double|`8200`|The order price
`qty`|double|`11.0`|Order quantity
`openPosition`|boolean|`true`|Open position or Close position order, eg: true or false
`ordStatus`|boolean|`true`|Open position or Close position order, eg: true or false
`cumQty`|double|`11.0`|The number of orders that has been executed
`avgPx`|double|`4754.24`|Average price of filled orders
`ordStatus`|string|`FILLED`|The order status, includes: `PENDING_NEW`, `NEW`, `PARTIALLY_FILLED`, `FILLED`, `PENDING_CANCEL`, `CANCELED`, `REJECTED`, `PENDING_REPLACE`, `REPLACED`, `WAITING`
`iceberg`|boolean|`false`|The order is iceberg
`showQty`|double|`1.0`|The order showQty
`pnl`|double|`1.1`|Pnl incurred for this order.
`fee`|double|`1.1`|Fees incurred for this order.
`triggerPrice`|double|`--`|Trigger price for conditional order
`triggerType`|string|`--`|Trigger type of price for conditional order, eg: LAST or INDEX, it will be LAST if not providedAvailable values : INDEX, LAST, MARK
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "id": "O101-20230531-040439-546-1837",
      "uid": "1323056886107491328",
      "symbol": "BTCUSDT",
      "created": 1685505879000,
      "modified": 1685505879000,
      "side": "Buy",
      "type": "Market",
      "tif": "GOOD_TILL_CANCEL",
      "currency": "BTC",
      "price": 0.0,
      "qty": 100.0,
      "openPosition": false,
      "cumQty": 100.0,
      "avgPx": 27620.2,
      "ordStatus": "FILLED",
      "iceberg": false,
      "showQty": 0.0,
      "source": "Normal",
      "pnl": 1.3365529862569942E-6,
      "fee": -1.448215436528338E-6,
      "triggerPrice": null,
      "triggerType": null,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null
    }
  ]
}
```
## `Query Trades`

Request trades by order id

### **Limit:**

Speed limit: 10 times/1min, Speed limit rule: apiKey

### **Request URL:**
```bash
GET /trade/queryTrades
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`orderId`|string|`O101-20230531-025722-174-1016`|`YES`|The order ID

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`T101-20230531-040439-548-1608`|The trade ID
`symbol`|string|`BTCUSDT`|The contract symbol
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`orderId`|string|`O101-20230531-025722-174-1016`|The order ID
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`openPosition`|boolean|`true`|Open position or Close position order, eg: true or false
`userId`|string|`1323056886107491328`|The user id
`accountId`|string|`1323056886107491328`|The account id
`price`|double|`8200`|The trade price
`qty`|double|`11.0`|Order quantity
`fee`|double|`1.1`|Fees incurred for this order.
`feeRate`|double|`0.0002`|Fee rate
`createdTimestamp`|long|`1570759718825`|Timestamp when the order is created
```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "id": "T101-20230531-040439-548-1608",
      "symbol": "BTCUSDT",
      "orderId": "O101-20230531-040439-546-1837",
      "side": "Buy",
      "openPosition": false,
      "userId": "1323056886107491328",
      "accountId": "1323056886107491328-CCF-BTC",
      "price": 27620.2,
      "qty": 100.0,
      "fee": -1.448215436528338E-6,
      "feeRate": -4.0E-4,
      "createdTimestamp": "1685505880000",
      "currency": "BTC"
    }
  ]
}
```

## `Query Active Orders`



### **Limit:**

Speed limit: 60 times /1 minutes, Speed limit rule: apiKey

### **Request URL:**
```bash
GET /trade/queryActiveOrders
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`symbol`|long|`BTCUSDT`|`NO`|Retrieve active orders of symbol, eg: BTCUSDT; Will return all active orders of user if empty

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`O101-20230531-025722-174-1016`|The order ID
`uid`|string|`1323056886107491328`|The User ID
`symbol`|string|`BTCUSDT`|The contract symbol
`created`|long|`1570759718825`|Timestamp when the order is created
`modified`|long|`1551062936784`|Timestamp when this order was updated last time
`clientOrderId`|string|`213443`|An unique order ID which is defined by user
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`type`|string|`--`|The order type, includes: `LIMIT`, `Market`
`tif`|string|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`price`|double|`8200`|The order price
`qty`|double|`11.0`|Order quantity
`openPosition`|boolean|`true`|Open position or Close position order, eg: true or false
`ordStatus`|boolean|`true`|Open position or Close position order, eg: true or false
`cumQty`|double|`11.0`|The number of orders that has been executed
`avgPx`|double|`4754.24`|Average price of filled orders
`ordStatus`|string|`FILLED`|The order status, includes: `PENDING_NEW`, `NEW`, `PARTIALLY_FILLED`, `FILLED`, `PENDING_CANCEL`, `CANCELED`, `REJECTED`, `PENDING_REPLACE`, `REPLACED`, `WAITING`
`iceberg`|boolean|`false`|The order is iceberg
`showQty`|double|`1.0`|The order showQty
`pnl`|double|`1.1`|Pnl incurred for this order.
`fee`|double|`1.1`|Fees incurred for this order.
`triggerPrice`|double|`--`|Trigger price for conditional order
`triggerType`|string|`--`|Trigger type of price for conditional order, eg: LAST or INDEX, it will be LAST if not providedAvailable values : INDEX, LAST, MARK
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "id": "O101-20230531-040439-546-1837",
      "uid": "1323056886107491328",
      "symbol": "BTCUSDT",
      "created": 1685505879000,
      "modified": 1685505879000,
      "side": "Buy",
      "type": "Market",
      "tif": "GOOD_TILL_CANCEL",
      "currency": "BTC",
      "price": 0.0,
      "qty": 100.0,
      "openPosition": false,
      "cumQty": 100.0,
      "avgPx": 27620.2,
      "ordStatus": "FILLED",
      "iceberg": false,
      "showQty": 0.0,
      "source": "Normal",
      "pnl": 1.3365529862569942E-6,
      "fee": -1.448215436528338E-6,
      "triggerPrice": null,
      "triggerType": null,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null
    }
  ]
}
```

## `Cancel`

Cancel an order, `orderId` or `clientOrderId` is required. This API endpoint requires your signed request.

### **Request Weight:**

Speed limit: 40 times/second Speed limit rule: apiKey

### **Request Url:**
```bash
POST /trade/cancelOrder
```

### **Parameter:**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`orderId`|integer|`NO`|`--`|The order ID
`clientOrderId`|string|`NO`|`--`|Unique customized client ID for the order

`orderId` and `clientOrderId`, at least one **MUST** be provided.

### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`result`|integer|`O101-20230531-023520-732-0610`|The order ID or An unique order ID defined by client

```json
{
  "code": 0,
  "message": "OK",
  "result": "O101-20230531-023520-732-0610"
}
```
## `Close Position`

Close all position with market price

### **Request Weight:**

Speed limit: 8 times/second Speed limit rule: apiKey

### **Request URL:**
```bash
POST /trade/closePosition
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`symbol`|string|`YES`|`--`|The contract symbol, eg: BTCUSDT
`side`|string|`YES`|`--`|Position side, eg: `Long` or `Short`
`currency `|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT

### **Response:**
```json
{
  "code": 0,
  "message": "OK",
  "result": null
}
```
## `Query Position`

Query all the position of the user

### **Limit:**

Speed limit: 60 times /1 minutes, Speed limit rule: apiKey

### **Request URL:**
```bash
GET /trade/queryPosition
```

### **Parameters：**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
------------ | ------------ | ------------ | ------------ | ------------

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`uid`|string|`1323056886107491328`|The User ID
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`symbol`|string|`BTCUSDT`|The contract symbol
`side`|string|`--`|Direction of the position. Direction type: `Long`, `Short`
`qty`|double|`11.0`|Position quantity
`price`|double|`4754.24`|Price of position
`closableQty`|double|`11.0`|Closable quantity
`pnlRate`|double|`-0.001`|Pnl rate
`value`|double|`11.0`|Position value by currency
`positionLeverage`|double|`11.0`|Position leverage
`liquidationPrice`|double|`11.0`|Liquidation price
`pnl`|double|`1.1`|Realized pnl
`urPnL`|double|`1.1`|Unrealized pnl
`deposit`|double|`1.1`|deposit
`lastPrice`|double|`1.1`|Last price
`created`|long|`1570759718825`|Timestamp when the position is created
`trailingStopPrice`|double|`--`|Trailing stop price
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "uid": "1323056886107491328",
      "currency": "ETH",
      "symbol": "BTCUSDT",
      "side": "Long",
      "qty": 0.0,
      "individualPosition": true,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-02-15T11:40:13.000+00:00",
      "lastPrice": 27128.5
    },
    {
      "uid": "1323056886107491328",
      "currency": "ETH",
      "symbol": "BTCUSDT",
      "side": "Short",
      "qty": 0.0,
      "individualPosition": true,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-02-15T11:40:13.000+00:00",
      "lastPrice": 27128.5
    },
    {
      "uid": "1323056886107491328",
      "currency": "USDT",
      "symbol": "BTCUSDT",
      "side": "Long",
      "qty": 0.0,
      "individualPosition": false,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-05-19T09:08:55.000+00:00",
      "lastPrice": 27128.5
    },
    {
      "uid": "1323056886107491328",
      "currency": "USDT",
      "symbol": "BTCUSDT",
      "side": "Short",
      "qty": 100.0,
      "individualPosition": false,
      "price": 27117.1,
      "closableQty": 100.0,
      "pnlRate": -0.013644526885249529,
      "value": 100.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": -0.04,
      "urPnL": -0.09742929738062658,
      "liquidationPrice": 162026.91766730743,
      "deposit": 5.0,
      "created": "2023-05-31T08:03:45.000+00:00",
      "lastPrice": 27128.5
    },
    {
      "uid": "1323056886107491328",
      "currency": "TUSDT",
      "symbol": "TBTCTUSDT",
      "side": "Long",
      "qty": 0.0,
      "individualPosition": true,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-02-17T09:06:52.000+00:00",
      "lastPrice": 27135.0
    },
    {
      "uid": "1323056886107491328",
      "currency": "TUSDT",
      "symbol": "TBTCTUSDT",
      "side": "Short",
      "qty": 0.0,
      "individualPosition": true,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 20.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-02-15T11:05:54.000+00:00",
      "lastPrice": 27135.0
    },
    {
      "uid": "1323056886107491328",
      "currency": "BTC",
      "symbol": "BTCUSDT",
      "side": "Long",
      "qty": 0.0,
      "individualPosition": false,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 16.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-02-15T11:08:13.000+00:00",
      "lastPrice": 27128.5
    },
    {
      "uid": "1323056886107491328",
      "currency": "BTC",
      "symbol": "BTCUSDT",
      "side": "Short",
      "qty": 0.0,
      "individualPosition": false,
      "price": 0.0,
      "closableQty": 0.0,
      "pnlRate": 0.0,
      "value": 0.0,
      "positionLeverage": 16.0,
      "trailingStopPrice": 0.0,
      "stopLossPrice": null,
      "stopWinType": null,
      "stopWinPrice": null,
      "trailingStop": null,
      "pnl": 0.0,
      "urPnL": 0.0,
      "liquidationPrice": 0.0,
      "deposit": 0.0,
      "created": "2023-05-31T04:04:07.000+00:00",
      "lastPrice": 27128.5
    }
  ]
}
```
## `Switch PosSide`

Switch position side

### **Limit:**

Speed limit: 8 times/second, Speed limit rule: apiKey

### **Request Url:**
```bash
POST /trade/switchPosSide
```

### **Parameter:**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`twoSidePosition`|boolean|`YES`|`--`|Switch to two side position mode if true, otherwise, switch to one side position mode
`currency`|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT
### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
------------ | ------------ | ------------ | ------------

```json
{
  "code": 0,
  "message": "OK",
  "result": null
}
```
## `Change Position Mode`

Change position mode between Individual and Cross

### **Limit:**

Speed limit: 8 times/second, Speed limit rule: apiKey

### **Request Url:**
```bash
POST /trade/changePosMode
```

### **Parameter:**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`individual`|boolean|`YES`|`--`|Position mode, true -> Individual; false -> Cross
`currency`|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT
### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
------------ | ------------ | ------------ | ------------

```json
{
  "code": 0,
  "message": "OK",
  "result": null
}
```
## `Change PosLeverage`

Change position leverage

### **Limit:**

Speed limit: 8 times/second, Speed limit rule: apiKey

### **Request Url:**
```bash
POST /trade/changePosLeverage
```

### **Parameter:**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`leverage`|double|`YES`|`--`|Position leverage
`symbol`|string|`YES`|`--`|The contract symbol, eg: BTCUSDT
`currency`|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT
### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
------------ | ------------ | ------------ | ------------

```json
{
  "code": 0,
  "message": "OK",
  "result": null
}
```
## `Change Risk`

Change risk setting of position

### **Limit:**

Speed limit: 10 times/second, Speed limit rule: apiKey

### **Request Url:**
```bash
POST /trade/riskSetting
```

### **Parameter:**

Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | -----
`symbol`|string|`YES`|`--`|The contract symbol, eg: BTCUSDT
`currency`|string|`YES`|`--`|Currency of account, eg: BTC, ETH, USDT
`side`|string|`YES`|`--`|Position side, eg: Long or Short
`addDeposit`|double|`NO`|`--`|Increase or decrease extra deposit for individual position
`stopLossPrice`|double|`NO`|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`NO`|`--`|Stop win price for advanced order
`stopWinType`|string|`NO`|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`NO`|`--`|Trailing stop value for advanced order, conflict with stopLossPrice

### **Response:**

Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
------------ | ------------ | ------------ | ------------

```json
{
  "code": 0,
  "message": "OK",
  "result": null
}
```
## `Query Accounts`

This endpoint is used to retrieve contract account balance. This endpoint requires
your signed request.

### **Limit:**

Speed limit: 60 times/min, Speed limit rule: apiKey

### **Request Url:**
```bash
GET  /trade/queryAccounts
```

### **Parameters:**
None

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`uid`|string|`1323056886107491328`|User id
`currency`|string|`USDT`|currency
`cash`|double|`0.01215991`|cash
`withdrawableCash`|double|`0`| Withdrawable cash
`frozenCash`|double|`0`| Frozen cash
`urPnl`|double|`0`| Unrealized pnl
`cashAvailable`|double|`0`| Available cash

### **Example:**

```json
{
    "code": 0,
        "message": "OK",
        "result": [
        {
            "uid": "1323056886107491328",
            "currency": "ETH",
            "cash": 0.0,
            "withdrawableCash": 0.0,
            "frozenCash": 0.0,
            "urPnl": 0.0,
            "cashAvailable": 0.0
        },
        {
            "uid": "1323056886107491328",
            "currency": "USDT",
            "cash": 499.90922659263174,
            "withdrawableCash": 499.90922659263174,
            "frozenCash": 0.0,
            "urPnl": 0.0,
            "cashAvailable": 499.90922659263174
        },
        {
            "uid": "1323056886107491328",
            "currency": "TUSDT",
            "cash": 1000.0,
            "withdrawableCash": 1000.0,
            "frozenCash": 0.0,
            "urPnl": 0.0,
            "cashAvailable": 1000.0
        },
        {
            "uid": "1323056886107491328",
            "currency": "BTC",
            "cash": 9.999288458956643,
            "withdrawableCash": 0.0,
            "frozenCash": 9.999288458956643,
            "urPnl": 2.573443933625344E-5,
            "cashAvailable": 9.999190074639152
        }
    ]
}
```
## `Query Account Settings`


### **Limit:**

Speed limit: 60 times/1min, Speed limit rule: apiKey

### **Request Url:**
```bash
GET  /trade/queryAccountSettings
```

### **Parameters:**
None

### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`uid`|string|`1323056886107491328`|User id
`currency`|string|`USDT`|currency
`twoWayPosition`|boolean|`true`|Two way position
`individualPosition`|boolean|`false`| Individual position

### **Example:**

```json
{
  "code": 0,
  "message": "OK",
  "result": [
    {
      "uid": "1323056886107491328",
      "currency": "ETH",
      "twoWayPosition": false,
      "individualPosition": true
    },
    {
      "uid": "1323056886107491328",
      "currency": "USDT",
      "twoWayPosition": true,
      "individualPosition": false
    },
    {
      "uid": "1323056886107491328",
      "currency": "TUSDT",
      "twoWayPosition": false,
      "individualPosition": true
    },
    {
      "uid": "1323056886107491328",
      "currency": "BTC",
      "twoWayPosition": true,
      "individualPosition": false
    }
  ]
}
```
# Private Websocket Endpoints


##  Receiving data requires two steps：

- Step 1: User login
- Step 2：Subscription event

## `User Login`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`login`| Operation event login
`txId`|string|`YES`|`--`| txId is a unique string, format: TXyyyyMMdd-HHmmss-ms-serial number, TX year-month-day-hour-minute-second-millisecond-unique serial number, for example TX20220809-142413-671-1
`apiKey`|string|`YES`|`--`| Apply at the individual center, apikey/apiSecret
`signature`|string|`YES`|`--`| signature=sha256(secret+op+expires)Note: The + needs to be removed to generate a signature
`expires`|long|`YES`|`--`| Custom UTC millisecond timestamp, if the request service time is greater than expires, a timeout will be returned

### **Example:**
```json
{
	"op": "login",
	"txId" :"TX20200317-133934-511-458",
    "apiKey": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
	"signature": "6532a891aa0b950bf28d82cf71bbce4a379029417d855de76e793654a1247e89",
	"expires": "1787218464317"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`txId`|string|`TX20200317-133934-511-458`| txId is a unique string, format: TXyyyyMMdd-HHmmss-ms-serial number, TX year-month-day-hour-minute-second-millisecond-unique serial number, for example TX20220809-142413-671-1

### **Example:**

```json
{
  "txId": "TX20200317-133934-511-458",
  "errCode": 0,
  "key": null
}
```

## `User Account Update`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`|subscribe/unsubscribe
`channel`|string|`YES`|`channel`| Event name
`apiKey`|string|`YES`|`--`| Apply at the individual center, apikey/apiSecret
`signature`|string|`YES`|`--`| signature=sha256(secret+channel+expires)Note: The + needs to be removed to generate a signature
`expires`|long|`YES`|`--`| Custom UTC millisecond timestamp, if the request service time is greater than expires, a timeout will be returned
### **Example:**
```json
{
  "op": "subscribe",
  "apiKey": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "signature": "6942114e809638701c001f1f2f633fe92727e42b54376a7c7fd594fc170e5819",
  "expires": "1787218464317",
  "channel": "accountUpdate"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`uid`|string|`1323056886107491328`|User id
`currency`|string|`USDT`|currency
`cash`|double|`0.01215991`|cash
`withdrawableCash`|double|`0`| Withdrawable cash
`frozenCash`|double|`0`| Frozen cash
`urPnl`|double|`0`| Unrealized pnl
`cashAvailable`|double|`0`| Available cash
### **Example:**
```json
{
  "account": {
    "uid": "1323056886107491328",
    "currency": "USDT",
    "cash": 499.6878667490653,
    "withdrawableCash": 0.0,
    "frozenCash": 499.6878667490653,
    "urPnl": 0.03929670132261469,
    "cashAvailable": 494.72519861532174
  },
  "key": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "event": "accountUpdate"
}
```

## `User Position Update`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`|subscribe/unsubscribe
`channel`|string|`YES`|`positionUpdate`| Event name
`apiKey`|string|`YES`|`--`| Apply at the individual center, apikey/apiSecret
`signature`|string|`YES`|`--`| signature=sha256(secret+channel+expires)Note: The + needs to be removed to generate a signature
`expires`|long|`YES`|`--`| Custom UTC millisecond timestamp, if the request service time is greater than expires, a timeout will be returned
### **Example:**
```json
{
  "op": "subscribe",
  "apiKey": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "signature": "6942114e809638701c001f1f2f633fe92727e42b54376a7c7fd594fc170e5819",
  "expires": "1787218464317",
  "channel": "positionUpdate"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`uid`|string|`1323056886107491328`|The User ID
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`symbol`|string|`BTCUSDT`|The contract symbol
`side`|string|`--`|Direction of the position. Direction type: `Long`, `Short`
`qty`|double|`11.0`|Position quantity
`price`|double|`4754.24`|Price of position
`closableQty`|double|`11.0`|Closable quantity
`pnlRate`|double|`-0.001`|Pnl rate
`value`|double|`11.0`|Position value by currency
`positionLeverage`|double|`11.0`|Position leverage
`liquidationPrice`|double|`11.0`|Liquidation price
`pnl`|double|`1.1`|Realized pnl
`urPnL`|double|`1.1`|Unrealized pnl
`deposit`|double|`1.1`|deposit
`lastPrice`|double|`1.1`|Last price
`created`|long|`1570759718825`|Timestamp when the position is created
`trailingStopPrice`|double|`--`|Trailing stop price
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`individualPosition`|boolean|false|Individual position
### **Example:**

```json
{
  "position": {
    "uid": "1323056886107491328",
    "currency": "BTC",
    "symbol": "BTCUSDT",
    "side": "Short",
    "qty": 100.0,
    "individualPosition": false,
    "price": 25736.8,
    "closableQty": 100.0,
    "pnlRate": -1.1547384662514797,
    "value": 0.0037344734597817746,
    "positionLeverage": 30.0,
    "trailingStopPrice": 0.0,
    "stopLossPrice": 0.0,
    "stopWinType": null,
    "stopWinPrice": 0.0,
    "trailingStop": 0.0,
    "pnl": -4.389851219720317E-6,
    "urPnL": -1.5101346944019543E-4,
    "liquidationPrice": 0.0,
    "deposit": 1.2951623097406568E-4,
    "created": 1686049269000,
    "lastPrice": 26766.9
  },
  "key": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "event": "positionUpdate"
}
```
## `User Order Update`

### **Parameters:**
Name|Type|Required|Default|Description
------------ | ------------ | ------------ | ------------ | ------------
`op`|string|`YES`|`--`|subscribe/unsubscribe
`channel`|string|`YES`|`orderUpdate`| Event name
`apiKey`|string|`YES`|`--`| Apply at the individual center, apikey/apiSecret
`signature`|string|`YES`|`--`| signature=sha256(secret+channel+expires)Note: The + needs to be removed to generate a signature
`expires`|long|`YES`|`--`| Custom UTC millisecond timestamp, if the request service time is greater than expires, a timeout will be returned
### **Example:**
```json
{
  "op": "subscribe",
  "apiKey": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "signature": "6942114e809638701c001f1f2f633fe92727e42b54376a7c7fd594fc170e5819",
  "expires": "1787218464317",
  "channel": "orderUpdate"
}
```
### **Response:**
Name|Type|Example|Description
------------ | ------------ | ------------ | ------------
`id`|string|`O101-20230531-025722-174-1016`|The order ID
`uid`|string|`1323056886107491328`|The User ID
`symbol`|string|`BTCUSDT`|The contract symbol
`created`|long|`1570759718825`|Timestamp when the order is created
`modified`|long|`1551062936784`|Timestamp when this order was updated last time
`clientOrderId`|string|`213443`|An unique order ID which is defined by user
`side`|string|`--`|Direction of the order. Direction type: `Buy`, `Sell`
`type`|string|`--`|The order type, includes: `LIMIT`, `Market`
`tif`|string|`GTC`|Type of advanced order, Available values : FILL_OR_KILL, GOOD_TILL_CANCEL, IMMEDIATE_OR_CANCEL, QUEUE_OR_CANCEL
`currency`|string|`USDT`|Currency of account, eg: BTC, ETH, USDT
`price`|double|`8200`|The order price
`qty`|double|`11.0`|Order quantity
`openPosition`|boolean|`true`|Open position or Close position order, eg: true or false
`ordStatus`|boolean|`true`|Open position or Close position order, eg: true or false
`cumQty`|double|`11.0`|The number of orders that have been executed
`avgPx`|double|`4754.24`|Average price of filled orders
`ordStatus`|string|`FILLED`|The order status, includes: `PENDING_NEW`, `NEW`, `PARTIALLY_FILLED`, `FILLED`, `PENDING_CANCEL`, `CANCELED`, `REJECTED`, `PENDING_REPLACE`, `REPLACED`, `WAITING`
`iceberg`|boolean|`false`|The order is iceberg
`showQty`|double|`1.0`|The order showQty
`pnl`|double|`1.1`|Pnl incurred for this order.
`fee`|double|`1.1`|Fees incurred for this order.
`triggerPrice`|double|`--`|Trigger price for conditional order
`triggerType`|string|`--`|Trigger type of price for conditional order, eg: LAST or INDEX, it will be LAST if not providedAvailable values : INDEX, LAST, MARK
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
`stopLossPrice`|double|`--`|Stop loss price for advanced order, conflict with trailingStop
`stopWinPrice`|double|`--`|Stop win price for advanced order
`stopWinType`|string|`--`|Stop win type of price for advanced order, eg: Limit or Market, it's required if stopWinPrice is set Available values : Limit, Market
`trailingStop`|double|`--`|Trailing stop value for advanced order, conflict with stopLossPrice
### **Example:**
```json
{
  "order": {
    "id": "O101-20230607-085005-846-2198",
    "clientOrderId": "213443",
    "uid": "1323056886107491328",
    "symbol": "BTCUSDT",
    "created": 1686127805000,
    "modified": 1686127805000,
    "side": "Buy",
    "type": "Market",
    "tif": "GOOD_TILL_CANCEL",
    "currency": "BTC",
    "price": 0.0,
    "qty": 100.0,
    "openPosition": false,
    "cumQty": 0.0,
    "avgPx": 0.0,
    "ordStatus": "NEW",
    "iceberg": false,
    "showQty": 0.0,
    "source": "Normal",
    "pnl": 0.0,
    "fee": 0.0,
    "triggerPrice": null,
    "triggerType": null,
    "stopLossPrice": null,
    "stopWinType": null,
    "stopWinPrice": null,
    "trailingStop": null
  },
  "key": "Ffs9IfPXEgeZa0aDebgkS4h1In73w2a0IyqYcfnVraGCcjLFKPR6bpGxw0iiMVzl",
  "event": "orderUpdate"
}
```

# Code Description


Code|Description|
------------ | ------------ | 
`29014`|Request too frequent, please try again later|
