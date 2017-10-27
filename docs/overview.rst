Getting Started
===============

Installation
------------

``python-binance`` is available on `PYPI <https://pypi.python.org/pypi/python-binance/>`_.
Install with ``pip``:

.. code:: bash

    pip install python-binance


Register on Binance
-------------------

Firstly `register an account with Binance <https://binance.com>`_.

Generate an API Key
-------------------

To use signed account methods you are required to `create an API Key  <https://www.binance.com/userCenter/createApi.html>`_.

Initialise the client
---------------------

Pass your API Key and Secret

.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

Making API Calls
----------------

Every method supports the passing of arbitrary parameters via keyword matching those in the`Binance API documentation <https://www.binance.com/restapipub.html>`_.
These keyword arguments will be sent directly to the relevant endpoint.

Each API method returns a dictionary of the JSON response as per the `Binance API documentation <https://www.binance.com/restapipub.html>`_.
The docstring of each method in the code references the endpoint it implements.

The Binance API documentation references a `timestamp` parameter, this is generated for you where required.

Some methods have a `recvWindow` parameter for `timing security, see Binance documentation <https://www.binance.com/restapipub.html#timing-security>`_.

API Endpoints are rate limited by Binance at 20 requests per second, ask them if you require more.
