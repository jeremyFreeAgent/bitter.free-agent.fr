Bitter Documentation
====================

Installation
------------
Use `Composer <https://github.com/composer/composer/>`_ to install: `free-agent/bitter`.

In your `composer.json` you should have:

.. code-block:: yaml

    {
        "require": {
            "free-agent/bitter": "1.1.*"
        }
    }

Bitter uses `Redis <http://redis.io>`_ (version >=2.6).

.. note::
    Every key created in `Redis` will be prefixed by *bitter:* ; temp keys by *bitter_temp:*.

Basic usage
-----------
Create a Bitter with a Redis client (Predis as example):

.. code-block:: php

    $redisClient = new \Predis\Client();
    $bitter = new FreeAgent\Bitter($redisClient);

Mark user 123 as active and has played a song:

.. code-block:: php

    $bitter
        ->mark('active', 123)
        ->mark('song:played', 123)
    ;

.. note::

    Please don't use huge ids (e.g. 2^32 or bigger) cause this will require large amounts of memory.

Pass a DateTime as third argument:

.. code-block:: php

    $bitter->mark('song:played', 123, new \DateTime('yesterday'));

Test if user 123 has played a song this week:

.. code-block:: php

    $currentWeek = new FreeAgent\Bitter\Event\Week('song:played');


    if ($bitter->in(123, $currentWeek) {
        echo 'User with id 123 has played a song this week.';
    } else {
        echo 'User with id 123 has not played a song this week.';
    }

How many users were active yesterday:

.. code-block:: php

    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    echo 'Yesterday: ' . $bitter->count($yesterday) . ' users were active.';

Using BitOp
-----------
How many users that were active yesterday are active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active');
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $count = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->count('bit_op_example')
    ;
    echo $count . ' users were active yesterday and today.';

.. note::
    The `bit_op_example` key will expire after 60 seconds.

Test if user 123 was active yesterday and is active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active');
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $active = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->in(123, 'bit_op_example')
    ;
    if ($active) {
        echo 'User with id 123 was active yesterday and today.';
    } else {
        echo 'User with id 123 was not active yesterday and today.';
    }

.. note::
    Please look at `Redis BITOP Command <http://redis.io/commands/bitop>`_ for performance considerations.

Custom date period stats
------------------------
How many users that were active during a given date period:

.. code-block:: php

    $from = new \DateTime('2010-14-02 20:15:30');
    $to   = new \DateTime('2012-21-12 13:30:45');

    $count = $bitter
        ->bitDatePeriod('active', 'active_period_example', $from, $to)
        ->count('active_period_example')
    ;
    echo $count . ' users were active from "2010-14-02 20:15:30" to "2012-21-12 13:30:45".';

Unit Tests
----------

You can run tests with:

.. code-block:: sh

    bin/atoum -d tests/units

Release notes
-------------
1.1.0

* Added date period stats with bitDatePeriod method.

Thanks
------
This library is a port of `bitmapist <https://github.com/Doist/bitmapist/>`_ (Python) by `Amir Salihefendic <http://amix.dk/>`_.
