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

Mark user 404 as active and has been kicked by Chuck Norris:

.. code-block:: php

    $bitter->mark('active', 404);
    $bitter->mark('kicked_by_chuck_norris', 404);

How many users that were active yesterday are active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active', new \DateTime());
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $count = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->count('bit_op_example')
    ;
    echo $count . ' were active yesterday are active today.';

Test if user 13 was active yesterday and is active today:

.. code-block:: php

    $today     = new FreeAgent\Bitter\Event\Day('active', new \DateTime());
    $yesterday = new FreeAgent\Bitter\Event\Day('active', new \DateTime('yesterday'));

    $active = $bitter
        ->bitOpAnd('bit_op_example', $today, $yesterday)
        ->in(13, 'bit_op_example')
    ;
    if ($active) {
        echo 'User 13 was active yesterday and today.';
    } else {
        echo 'User 13 was not active yesterday and today.';
    }
