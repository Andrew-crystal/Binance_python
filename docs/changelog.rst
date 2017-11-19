Changelog
=========

v0.4.0 - 2017-11-19
^^^^^^^^^^^^^^^^^^^

**Added**

- Get deposit address endpoint
- Upgraded withdraw endpoints to v3
- New exchange info endpoint with rate limits and full symbol info

**Removed**

- Order validation to return at a later date

v0.3.8 - 2017-11-17
^^^^^^^^^^^^^^^^^^^

**Fixes**

- Fix order validation for market orders
- WEBSOCKET_DEPTH_20 value, 20 instead of 5
- General tidy up

v0.3.7 - 2017-11-16
^^^^^^^^^^^^^^^^^^^

**Fixes**

- Fix multiple depth caches sharing a cache by initialising bid and ask objects each time

v0.3.6 - 2017-11-15
^^^^^^^^^^^^^^^^^^^

**Fixes**

- check if Reactor is already running

v0.3.5 - 2017-11-06
^^^^^^^^^^^^^^^^^^^

**Added**

- support for BNB market

**Fixes**

- fixed error if new market type is created that we don't know about

v0.3.4 - 2017-10-31
^^^^^^^^^^^^^^^^^^^

**Added**

- depth parameter to depth socket
- interval parameter to kline socket
- update time parameter for compatible sockets
- new enums for socket depth and update time values
- better websocket documentation

**Changed**

- Depth Cache Manager uses 0ms socket update time
- connection key returned when creating socket, this key is then used to stop it

**Fixes**

- General fixes

v0.3.3 - 2017-10-31
^^^^^^^^^^^^^^^^^^^

**Fixes**

- Fixes for broken tests

v0.3.2 - 2017-10-30
^^^^^^^^^^^^^^^^^^^

**Added**

- More test coverage of requests

**Fixes**

- Order quantity validation fix

v0.3.1 - 2017-10-29
^^^^^^^^^^^^^^^^^^^

**Added**

- Withdraw exception handler with translation of obscure error

**Fixes**

- Validation fixes

v0.3.0 - 2017-10-29
^^^^^^^^^^^^^^^^^^^

**Added**

- Withdraw endpoints
- Order helper functions

v0.2.0 - 2017-10-27
^^^^^^^^^^^^^^^^^^^

**Added**

- Symbol Depth Cache

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