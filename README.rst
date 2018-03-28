tcconfig
========

.. image:: https://img.shields.io/pypi/pyversions/tcconfig.svg
   :target: https://pypi.python.org/pypi/tcconfig

.. image:: https://travis-ci.org/thombashi/tcconfig.svg?branch=master
   :target: https://travis-ci.org/thombashi/tcconfig
   :alt: Linux CI test status

.. image:: https://img.shields.io/github/stars/thombashi/tcconfig.svg?style=social&label=Star
   :target: https://github.com/thombashi/tcconfig
   :alt: GitHub repository

Summary
-------

A Simple tc command wrapper tool. Easy to set up traffic control of network bandwidth/latency/packet-loss to a network interface.

Traffic control features
------------------------

Trafic shaping target
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Apply traffic shaping rules to specific target:

-  Outgoing/Incoming packets
-  Certain IP address/network or port

Available parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following parameters can be set to network interfaces.

-  Network bandwidth rate [G/M/K bps]
-  Network latency [milliseconds]
-  Packet loss rate [%]
-  Packet corruption rate [%]

.. image:: docs/gif/tcset_example.gif

Usage
=====

Set traffic control (``tcset`` command)
---------------------------------------

``tcset`` is a command to add traffic control rule to a network interface (device).

e.g. Set a limit on bandwidth up to 100Kbps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --rate 100k

e.g. Set 100ms network latency
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --delay 100

e.g. Set 0.1% packet loss
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --loss 0.1

e.g. All of the above at once
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --rate 100k --delay 100 --loss 0.1

e.g. Specify the IP address of traffic control
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --delay 100 --network 192.168.0.10

e.g. Specify the IP network and port of traffic control
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --delay 100 --network 192.168.0.0/24 --port 80

Delete traffic control (``tcdel`` command)
------------------------------------------

``tcdel`` is a command to delete traffic shaping rules from a network interface (device).

e.g. Delete traffic control of ``eth0``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcdel --device eth0


Display traffic control configurations (``tcshow`` command)
-----------------------------------------------------------

``tcshow`` is a command to display traffic control to network interface(s).

Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: console

    # tcset --device eth0 --delay 10 --delay-distro 2  --loss 0.01 --rate 0.25M --network 192.168.0.10 --port 8080
    # tcset --device eth0 --delay 1 --loss 0.02 --rate 500K --direction incoming
    # tcshow --device eth0
    {
        "eth0": {
            "outgoing": {
                "network=192.168.0.10/32, dst-port=8080": {
                    "delay": "10.0",
                    "loss": "0.01",
                    "rate": "250K",
                    "delay-distro": "2.0"
                },
                "network=0.0.0.0/0": {}
            },
            "incoming": {
                "network=0.0.0.0/0": {
                    "delay": "1.0",
                    "loss": "0.02",
                    "rate": "500K"
                }
            }
        }
    }

For more information
--------------------

More examples are available at 
http://tcconfig.rtfd.io/en/latest/pages/usage/index.html

Installation
============

The official release can be installed from pip package. Please refer to the official `README <README_OFFICIAL.rst>`__ for 
reference. We've made some modifications to the tool, and it should be invoked from within this repository rather than 
installing into system path.

To install, just type ``./init.sh``, and then use the wrappers ``./tcset``, ``./tcshow``, ``./tcdel``.

Example
-------

To display the underlying tc commands for adding a delay of 300ms and 
packet loss rate of 35% to a host named pano2.


.. code:: console

   # ./tcset --delay 300 --loss 35 --network pano2 --tc-command

To actually apply these commands remove the ``--tc-command``:

.. code:: console

   # ./tcset --delay 300 --loss 35 --network pano2
   # ./tcshow
   # ./tcdel
   # ./tcset --delay 10000 --network pano2 --src-port 2888 --tc-command
   # ./tcset --delay 500 --loss 35 --network order1 --device ens1f1 --direction outgoing

Documentation
=============

http://tcconfig.rtfd.io/
