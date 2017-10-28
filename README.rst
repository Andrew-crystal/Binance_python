================================
Welcome to python-binance v0.3.0
================================

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

Source code
  https://github.com/sammchardy/python-binance

Documentation
  https://python-binance.readthedocs.io/en/latest/

Features
--------

- Implementation of General, Market Data and Account endpoints.
- Simple handling of authentication
- No need to generate timestamps yourself, the wrapper does it for you
- Response exception handling
- Websocket handling
- Symbol Depth Cache
- Withdraw functionality
- Order parameter validation based on Trade Rules

Quick Start
-----------

.. code:: bash

    pip install python-binance


.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

    # get market depth
    depth = client.get_order_book(symbol='BNBBTC')

    # place market buy order
    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_MARKET,
        quantity=100)

    # get all symbol prices
    prices = client.get_all_tickers()

    # withdraw 100 ETH
    result = client.withdraw(
        asset='ETH',
        address='<eth_address>',
        amount=100)

    # fetch list of withdrawals
    withdraws = client.get_withdraw_history()

    # start trade websocket
    def process_message(msg):
        print("message type:" + msg[e])
        print(msg)
        # do something

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    bm.start_trade_socket(symbol='BNBBTC')
    bm.start()

For more `check out the documentation <https://python-binance.readthedocs.io/en/latest/>`_.

Donate
------

If this library helped you out feel free to donate.

- ETH: 0xD7a7fDdCfA687073d7cC93E9E51829a727f9fE70
- NEO: AVJB4ZgN7VgSUtArCt94y7ZYT6d5NDfpBo
- BTC: 1Dknp6L6oRZrHDECRedihPzx2sSfmvEBys
