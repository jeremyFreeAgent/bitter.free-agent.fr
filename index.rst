Bitter
======

    "Use Bitter and you have time to drink a bitter beer !"

-- **Jérémy Romey**

Introduction
------------

Bitter can answer following questions:

* Has user X been online today? This week? This month?
* Has user X performed action "Y"?
* How many users have been active have this month? This hour?
* How many unique users have performed action "Y" this week?
* How many % of users that were active last week are still active?
* How many % of users that were active last month are still active this month?

Bitter is very easy to use and enables you to create your own reports easily.

Basic usage
-----------
Create a Bitter with a Redis client (Predis as example):

.. code-block:: php

    $redisClient = new \Predis\Client();
    $bitter = new FreeAgent\Bitter($redisClient);

Mark user 123 as active and has played a song:

.. code-block:: php

    $bitter->mark('active', 123);
    $bitter->mark('song:played', 123);

How many users that were active yesterday are active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active', new \DateTime());
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $count = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->count('bit_op_example')
    ;
    echo $count . ' users were active yesterday and today.';

Test if user 123 was active yesterday and is active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active', new \DateTime());
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $active = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->in(123, 'bit_op_example')
    ;
    if ($active) {
        echo 'User 123 was active yesterday and today.';
    } else {
        echo 'User 123 was not active yesterday and today.';
    }


How many users that were active during a given date period:

.. code-block:: php

    $from = new \DateTime('2010-14-02 20:15:30');
    $to   = new \DateTime('2012-21-12 13:30:45');

    $count = $bitter
        ->bitDatePeriod('active', 'active_period_example', $from, $to)
        ->count('active_period_example')
    ;
    echo $count . ' users were active from "2010-14-02 20:15:30" to "2012-21-12 13:30:45".';
