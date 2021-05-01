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

Firstly `register an account with Binance <https://www.binance.com/register.html?ref=10099792>`_.

Generate an API Key
-------------------

To use signed account methods you are required to `create an API Key  <https://www.binance.com/en/support/faq/360002502072>`_.

Initialise the client
---------------------

Pass your API Key and Secret

.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

Making API Calls
----------------

Every method supports the passing of arbitrary parameters via keyword matching those in the `Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_.
These keyword arguments will be sent directly to the relevant endpoint.

Each API method returns a dictionary of the JSON response as per the `Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_.
The docstring of each method in the code references the endpoint it implements.

The Binance API documentation references a `timestamp` parameter, this is generated for you where required.

Some methods have a `recvWindow` parameter for `timing security, see Binance documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#timing-security>`_.

API Endpoints are rate limited by Binance at 20 requests per second, ask them if you require more.

Async API Calls
---------------

aiohttp is used to handle asyncio REST requests.

Each function available in the normal client is available in the AsyncClient class.

The only difference is to run within an asyncio event loop and await the function like below.

.. code:: python

    from binance import AsyncClient

    async def main():
        client = await AsyncClient.create()

        # fetch exchange info
        res = await client.get_exchange_info()
        print(json.dumps(res, indent=2))

    if __name__ == "__main__":

        loop = asyncio.get_event_loop()
        loop.run_until_complete(main())


API Rate Limit
--------------

Check the `get_exchange_info() <binance.html#binance.client.Client.get_exchange_info>`_ call for up to date rate limits.

At the current time Binance rate limits are:

- 1200 requests per minute
- 10 orders per second
- 100,000 orders per 24hrs

Some calls have a higher weight than others especially if a call returns information about all symbols.
Read the `official Binance documentation <https://github.com/binance-exchange/binance-official-api-docs>`_ for specific information.

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/overview?pixel

Requests Settings
-----------------

`python-binance` uses the `requests <http://docs.python-requests.org/en/master/>`_ library.

You can set custom requests parameters for all API calls when creating the client.

.. code:: python

    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})

You may also pass custom requests parameters through any API call to override default settings or the above settingsspecify new ones like the example below.

.. code:: python

    # this would result in verify: False and timeout: 5 for the get_all_orders call
    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})
    client.get_all_orders(symbol='BNBBTC', requests_params={'timeout': 5})

Check out the `requests documentation <http://docs.python-requests.org/en/master/>`_ for all options.

**Proxy Settings**

You can use the Requests Settings method above

.. code:: python

    proxies = {
        'http': 'http://10.10.1.10:3128',
        'https': 'http://10.10.1.10:1080'
    }

    # in the Client instantiation
    client = Client("api-key", "api-secret", {'proxies': proxies})

    # or on an individual call
    client.get_all_orders(symbol='BNBBTC', requests_params={'proxies': proxies})

Or set an environment variable for your proxy if required to work across all requests.

An example for Linux environments from the `requests Proxies documentation <http://docs.python-requests.org/en/master/user/advanced/#proxies>`_ is as follows.

.. code-block:: bash

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"

For Windows environments

.. code-block:: bash

    C:\>set HTTP_PROXY=http://10.10.1.10:3128
    C:\>set HTTPS_PROXY=http://10.10.1.10:1080
