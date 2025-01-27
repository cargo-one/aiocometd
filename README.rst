aiocometd
=========

👉 cargo.one fork of the original ``aiocometd`` 👈
-------------------------------------------------

This fork integrates the changes to make the library compatible with Python 3.10+ and bumps the version to ``0.4.6``.
Changes heavily inspired by a pull request on the original repository (see https://github.com/robertmrk/aiocometd/pull/17). 👈

Intro
-----

aiocometd is a CometD_ client built using asyncio_, implementing the Bayeux_
protocol.

CometD_ is a scalable WebSocket and HTTP based event and message routing bus.
CometD_ makes use of WebSocket and HTTP push technologies known as Comet_ to
provide low-latency data from the server to browsers and client applications.

Features
--------

- Supported transports:
   - ``long-polling``
   - ``websocket``
- Automatic reconnection after network failures
- Extensions

Usage
-----

.. code-block:: python

    import asyncio

    from aiocometd import Client

    async def chat():
        nickname = "John"

        # connect to the server
        async with Client("http://example.com/cometd") as client:

                # subscribe to channels to receive chat messages and
                # notifications about new members
                await client.subscribe("/chat/demo")
                await client.subscribe("/members/demo")

                # send initial message
                await client.publish("/chat/demo", {
                    "user": nickname,
                    "membership": "join",
                    "chat": nickname + " has joined"
                })
                # add the user to the chat room's members
                await client.publish("/service/members", {
                    "user": nickname,
                    "room": "/chat/demo"
                })

                # listen for incoming messages
                async for message in client:
                    if message["channel"] == "/chat/demo":
                        data = message["data"]
                        print(f"{data['user']}: {data['chat']}")

    if __name__ == "__main__":
        loop = asyncio.get_event_loop()
        loop.run_until_complete(chat())

For more detailed usage examples take a look at the
`command line chat example <cli_example_>`_ or for a more complex example with
a GUI check out the aiocometd-chat-demo_.

Documentation
-------------

https://aiocometd.readthedocs.io/

Install
-------

.. code-block:: bash

    pip install aiocometd

Requirements
------------

- Python 3.6+
- aiohttp_

.. _aiohttp: https://github.com/aio-libs/aiohttp/
.. _CometD: https://cometd.org/
.. _Comet: https://en.wikipedia.org/wiki/Comet_(programming)
.. _asyncio: https://docs.python.org/3/library/asyncio.html
.. _Bayeux: https://docs.cometd.org/current/reference/#_bayeux
.. _ext: https://docs.cometd.org/current/reference/#_bayeux_ext
.. _cli_example: https://github.com/robertmrk/aiocometd/blob/develop/examples/chat.py
.. _aiocometd-chat-demo: https://github.com/robertmrk/aiocometd-chat-demo
