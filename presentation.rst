.. role:: hidden
   :class: hidden


Async is The New Default
========================


Why Async?
==========


To Save Memory
==============


:hidden:`Metrics`
=================

* 500 Mb process
* 100 ms response time
* **70%** serving requests
* 7 RPS
* 5% CPU usage


Sync → Async
=============

* 500 Mb process
* 103-105 ms response time
* 100 RPS
* 70% CPU usage


Failure
=======

* Latency: 50 ms → 3-15 sec
* Response: 50 ms → 0.5-1 sec
* Slow: 0.04 / 10 RPS


Warning: code ahead!
====================


Example
=======

.. code-block:: python

   async def combine_things(http, u1, u2):
        value1 = await get_json1()
        value2 = await get_json2()
        return combine(value1, value2)


Simultaneous
============

.. code-block:: python

   def combine_things(http, u1, u2):
        (value1, value2), _ = await asyncio.wait([
            get_json1(u1),
            get_json2(u2),
        ])
        return combine(value1, value2)


:hidden:`Pipelining`
====================

.. image:: pipe_vs_seq.svg


Keep-Alive
==========

Nginx → Backend


Request Timeout
===============

.. code-block:: python

    with aiohttp.Timeout(10):
        async with session.get(url) as response:
            assert response.status == 200
            return await response.read()


Connection Pool
===============

* 100 input connections
* 10 db connections


socket.close()
==============


Task Queues
===========

asyncio.async ≠ celery


Async -> Sync
=============

Pool Size = 1



WebSockets
==========

Browser → Gateway → Backend


GUI
===


Atomicity
=========

.. code-block:: python

    async def transfer(amount, payer, payee, server):
        if not payer.sufficient_funds(amount):
            raise InsufficientFunds()
        payee.deposit(amount)
        log_transfer(amount)
        payer.withdraw(amount)
        await server.update_balances([payer, payee])


Greenlets
=========

.. code-block:: python

    async def transfer(amount, payer, payee, server):
        with lock(payer, payee):
            if not payer.sufficient_funds(amount):
                raise InsufficientFunds()
            payee.deposit(amount)
            log_transfer(amount)
            payer.withdraw(amount)
        server.update_balances([payer, payee])


Greenlets: No Monkeypatching!
=============================


Q & A
=====
