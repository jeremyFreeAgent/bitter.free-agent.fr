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

Note: Bitter uses `Redis <http://redis.io>`_ (version >=2.6).

Configuration
-------------

Using `SncRedisBundle <https://github.com/snc/SncRedisBundle>`_ redis client:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: snc_redis.default

Using custom redis client:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: your.very.best.redis.client

You can also configure custom values for `prefix_key` and `expire_timeout`:

.. code-block:: yaml

    rezzza_bitter:
        redis_client: snc_redis.default
        prefix_key: myapp   # default - bitter
        expire_timeout: 300 # default - 60

Basic usage
-----------
Get Bitter:

.. code-block:: php

    $bitter = $this->container->get('rezzza.bitter');

Mark user 123 as active and has played a song:

.. code-block:: php

    $bitter->mark('active', 123);
    $bitter->mark('song:played', 123);

.. note::
    Please see the `Bitter Library <http://bitter.free-agent.fr/bitter.html>`_ documentation for more examples.
