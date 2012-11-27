BitterBundle Documentation
==========================

Installation
------------
Use `Composer <https://github.com/composer/composer/>`_ to install: `rezzza/bitter-bundle`.

In your `composer.json` you should have:

.. code-block:: yaml

    {
        "require": {
            "rezzza/bitter-bundle": "*"
        }
    }

Then update your `AppKernel.php` to register the bundle with:

.. code-block:: php

    new Rezzza\BitterBundle\RezzzaBitterBundle()

Bitter uses `Redis <http://redis.io>`_ (version >=2.6).


Configuration
-------------

Using `SncRedisBundle <https://github.com/snc/SncRedisBundle>`_ redis client:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: snc_redis.default_client

Using custom redis client:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: your.very.best.redis.client

You can also configure custom values for `prefix_key` and `expire_timeout`:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: snc_redis.default
        prefix_key: jack_bauer
        expire_timeout: 404

.. note::
    Default values for `prefix_key` and `expire_timeout` are **bitter** and **60**.

Basic usage
-----------
Get Bitter:

.. code-block:: php

    $bitter = $this->container->get('rezzza.bitter');

Mark user 404 as active and has been kicked by Chuck Norris:

.. code-block:: php

    $bitter->mark('active', 404);
    $bitter->mark('kicked_by_chuck_norris', 404);

.. note::
    Please look at `Bitter <http://bitter.free-agent.fr/bitter.html>`_ for all examples.
