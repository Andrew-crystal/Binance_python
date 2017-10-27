Binance API
========

.. image:: https://img.shields.io/pypi/v/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/l/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/travis/sammchardy/python-binance.svg
    :target: https://travis-ci.org/sammchardy/python-binance

.. image:: https://img.shields.io/coveralls/sammchardy/python-binance.svg
    :target: https://coveralls.io/github/sammchardy/python-binance

.. image:: https://img.shields.io/pypi/wheel/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/pyversions/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

This is an unofficial Python wrapper for the `Binance exchange API v1 <https://www.binance.com/restapipub.html>`_. I am in no way affiliated with Binance, use at your own risk.

Features
--------

- Implementation of General, Market Data and Account endpoints.
- Simple handling of authentication
- No need to generate timestamps yourself, the wrapper does it for you
- Response exception handling
- Websocket handling
- Symbol Depth Cache
- Order parameter validation based on Trade Rules

Installation
------------

``binance`` is available on `PYPI <https://pypi.python.org/pypi/python-binance/>`_.
Install with ``pip``:

.. code:: bash

    pip install python-binance

Documentation
-------------

Firstly `register with Binance <https://binance.com>`_.

API Key + Secret
^^^^^^^^^^^^^^^^

If you want to use signed methods then `create an API Key  <https://www.binance.com/userCenter/createApi.html>`_.

Init Client
^^^^^^^^^^^

.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

Exceptions
^^^^^^^^^^

On an API call error a binance.exceptions.BinanceAPIException will be raised.
The exception provides access to the
- `status_code` - response status code
- `response` - response object
- `code` - Binance error code
- `message` - Binance error message
- `request` - request object if available


When placing an order parameters are validated to check they fit within the `Binance Trading Rules <https://binance.zendesk.com/hc/en-us/articles/115000594711>`_.

The following exceptions extend `BinanceOrderException`.

**BinanceOrderMinAmountException**

Raised if the specified amount isn't a multiple of the trade minimum amount.

**BinanceOrderMinPriceException**

Raised if the price is lower than the trade minimum price.

**BinanceOrderTotalPriceException**

Raised if the total is lower than the trade minimum total.


Making API Calls
^^^^^^^^^^^^^^^^

Every method supports the passing of arbitrary parameters via keyword.
These keyword arguments will be sent directly to the relevant endpoint.

Each API method returns a dictionary of the JSON response as per the `Binance API documentation <https://www.binance.com/restapipub.html>`_.
The docstring of each method in the code references the endpoint it implements.

Some methods require a `timestamp` parameter, this is generated for you where required.

Some methods have a `recvWindow` parameter for `timing security, see Binance documentation <https://www.binance.com/restapipub.html#timing-security>`_.

API Endpoints are rate limited by Binance at 20 requests per second.

Order Validation
^^^^^^^^^^^^^^^^

Binance has a number of rules around symbol pair orders with validation on minimum price, quantity and total order value.

These rules are fetched when the client is initialised.

The rules can be refreshed by calling the `get_products` API endpoint.

We can then validate if pairs are being actively traded on Binance as well.

ENUMs
^^^^^

Binance defines Enumerated Types for Order Types, Order Side, Time in Force and Kline intervals.

.. code:: python

    SYMBOL_TYPE_SPOT = 'SPOT'

    ORDER_STATUS_NEW = 'NEW'
    ORDER_STATUS_PARTIALLY_FILLED = 'PARTIALLY_FILLED'
    ORDER_STATUS_FILLED = 'FILLED'
    ORDER_STATUS_CANCELED = 'CANCELED'
    ORDER_STATUS_PENDING_CANCEL = 'PENDING_CANCEL'
    ORDER_STATUS_REJECTED = 'REJECTED'
    ORDER_STATUS_EXPIRED = 'EXPIRED'

    KLINE_INTERVAL_1MINUTE = '1m'
    KLINE_INTERVAL_2MINUTE = '3m'
    KLINE_INTERVAL_5MINUTE = '5m'
    KLINE_INTERVAL_15MINUTE = '15m'
    KLINE_INTERVAL_30MINUTE = '30m'
    KLINE_INTERVAL_1HOUR = '1h'
    KLINE_INTERVAL_2HOUR = '2h'
    KLINE_INTERVAL_4HOUR = '4h'
    KLINE_INTERVAL_6HOUR = '6h'
    KLINE_INTERVAL_8HOUR = '8h'
    KLINE_INTERVAL_12HOUR = '12h'
    KLINE_INTERVAL_1DAY = '1d'
    KLINE_INTERVAL_3DAY = '3d'
    KLINE_INTERVAL_1WEEK = '1w'
    KLINE_INTERVAL_1MONTH = '1M'

    SIDE_BUY = 'BUY'
    SIDE_SELL = 'SELL'

    ORDER_TYPE_LIMIT = 'LIMIT'
    ORDER_TYPE_MARKET = 'MARKET'

    TIME_IN_FORCE_GTC = 'GTC'
    TIME_IN_FORCE_IOC = 'IOC'


Examples
^^^^^^^^

**Get the server time**

.. code:: python

    time_res = client.get_server_time()


**Fetch all orders**

.. code:: python

    orders = client.get_all_orders(symbol='BNBBTC', limit=10)


**Place a limit order**

.. code:: python

    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_LIMIT,
        timeInForce=TIME_IN_FORCE_GTC,
        quantity=100,
        price='0.00001')


**Place a market order**

.. code:: python

    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_MARKET,
        quantity=100,)


**Check order status**

.. code:: python

    order = client.cancel_order(
        style='BNBBTC',
        orderId='orderId')


**Cancel an order**

.. code:: python

    order = client.get_order(
        symbol='BNBBTC',
        orderId='orderId')


**Disable Client Side Order Validation**

    Pass the optional `disable_validation` parameter to turn off client side validation of orders.

.. code:: python

    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_LIMIT,
        timeInForce=TIME_IN_FORCE_GTC,
        quantity=100,
        price='0.00001',
        disable_validation=True)


**Get all open orders**

.. code:: python

    order = client.get_open_orders(symbol='BNBBTC')


**Using Enumerated types**

.. code:: python

    from binance.enums import *
    candles = client.get_klines(symbol='BNBBTC', interval=KLINE_INTERVAL_30MINUTE)

**Error Handling**

.. code:: python

    try:
        client.get_all_orders()
    except BinanceAPIException as e:
        print e.status_code
        print e.message


Undocumented Endpoints
^^^^^^^^^^^^^^^^^^^^^^

**get_all_tickers()**

Get last price for all markets.

Response format

.. code:: json

    [
        {"price": "0.07074900", "symbol": "ETHBTC"},
        {"price": "0.01612500", "symbol": "LTCBTC"},
        {"price": "0.00024791", "symbol": "BNBBTC"}
    ]

**get_orderbook_tickers()**

Get first bid and ask entry in the order book for all markets.

Response format

.. code:: json

    [
        {"askQty": "1.25900000", "bidPrice": "0.07050000", "symbol": "ETHBTC", "bidQty": "0.18000000", "askPrice": "0.07074900"},
        {"askQty": "0.23000000", "bidPrice": "0.01611400", "symbol": "LTCBTC", "bidQty": "1.17000000", "askPrice": "0.01613900"},
        {"askQty": "49.00000000", "bidPrice": "0.00024604", "symbol": "BNBBTC", "bidQty": "32.00000000", "askPrice": "0.00024690"}
    ]


Websockets
^^^^^^^^^^

Sockets are handled through a Socket Manager `BinanceSocketManager`.
Multiple socket connections can be made through the manager.
Only one instance of each socket type will be created, i.e. only one BNBBTC Depth socket can be created
and there can be both a BNBBTC Depth and a BNBBTC Trade socket open at once.

Socket connections pass a callback function to receive messages.
Messages are received are dictionary objects relating to the message formats defined in the `Binance API documentation <https://www.binance.com/restapipub.html#wss-endpoint>`_.

Create the manager like so, passing the api client.

.. code:: python

    bm = BinanceSocketManager(client)
    # attach any sockets here then start
    bm.start()

A callback to process messages would take the format

.. code:: python

    def process_message(msg):
        print("message type:" + msg[e])
        print(msg)
        # do something

**Depth Socket**

.. code:: python

    bm.start_depth_socket('BNBBTC', process_message)

**Kline Socket**

.. code:: python

    bm.start_kline_socket('BNBBTC', process_message)

**Aggregated Trade Socket**

.. code:: python

    bm.start_trade_socket('BNBBTC', process_message)

**Ticker Socket**

.. code:: python

    bm.start_ticker_socket(process_message)

**User Socket**

This watches for 3 different events

- Account Update Event
- Order Update Event
- Trade Update Event

The Manager handles keeping the socket alive.

.. code:: python

    bm.start_user_socket(process_message)

**Close Socket**

To close an individual socket call the corresponding close function

- stop_depth_socket
- stop_kline_socket
- stop_trade_socket
- stop_ticker_socket
- stop_user_socket


To stop all sockets and end the manager call `close` after doing this a `start` call would be required to connect any new sockets.

.. code:: python

    bm.close()


Depth Cache
^^^^^^^^^^^

To follow the depth cache updates for a symbol use the `DepthCacheManager`

Create the manager like so, passing the api client, symbol and callback function.

.. code:: python

    dcm = DepthCacheManager(client, 'BNBBTC', process_depth)

The callback function receives the current `DepthCache` object which allows access to a pre-sorted
list of bids or asks able to be filtered as required.

.. code:: python

    def process_depth(depth_cache):
        print("top 5 bids")
        print(depth_cache.get_bids()[:5])
        print("top 5 asks")
        print(depth_cache.get_asks()[:5])

At any time the current `DepthCache` object can be retrieved from the `DepthCacheManager`

.. code:: python

    depth_cache = dcm.get_depth_cache()
    print("top 5 bids")
    print(depth_cache.get_bids()[:5])
    print("top 5 asks")
    print(depth_cache.get_asks()[:5])


FAQ
--------

*Q: Why do I get "Timestamp for this request is not valid"*

*A*: This occurs in 2 different cases.

The timestamp sent is outside of the serverTime - recvWindow value
The timestamp sent is more than 1000ms ahead of the server time

Check that your system time is in sync. See `this issue <https://github.com/sammchardy/python-binance/issues/2#issuecomment-324878152>`_ for some sample code to check the difference between your local
time and the Binance server time.


*Q: Why do I get "Signature for this request is not valid"*

*A1*: One of your parameters may not be in the correct format.

Check recvWindow is an integer and not a string.

*A2*: You may be attempting to access the API from a Chinese IP address, these are now restricted by Binance.

Donate
------

If this library helped you out feel free to donate.

- ETH: 0xD7a7fDdCfA687073d7cC93E9E51829a727f9fE70
- NEO: AVJB4ZgN7VgSUtArCt94y7ZYT6d5NDfpBo
- BTC: 1Dknp6L6oRZrHDECRedihPzx2sSfmvEBys

Changelog
---------

v0.1.6 - 2017-10-25
^^^^^^^^^^^^^^^^^^^

**Changes**

- Upgrade to v3 signed endpoints
- Update function documentation


v0.1.5 - 2017-09-12
^^^^^^^^^^^^^^^^^^^

**Changes**

- Added get_all_tickers call
- Added get_orderbook_tickers call
- Added some FAQs

**Fixes**

- Fix error in enum value

v0.1.4 - 2017-09-06
^^^^^^^^^^^^^^^^^^^

**Changes**

- Added parameter to disable client side order validation

v0.1.3 - 2017-08-26
^^^^^^^^^^^^^^^^^^^

**Changes**

- Updated documentation

**Fixes**

- Small bugfix

v0.1.2 - 2017-08-25
^^^^^^^^^^^^^^^^^^^

**Added**

- Travis.CI and Coveralls support

**Changes**

- Validation for pairs using public endpoint

v0.1.1 - 2017-08-17
^^^^^^^^^^^^^^^^^^^

**Added**

- Validation for HSR/BTC pair

v0.1.0 - 2017-08-16
^^^^^^^^^^^^^^^^^^^

Websocket release

**Added**

- Websocket manager
- Order parameter validation
- Order and Symbol enums
- API Endpoints for Data Streams

v0.0.2 - 2017-08-14
^^^^^^^^^^^^^^^^^^^

Initial version

**Added**

- General, Market Data and Account endpoints
