Setup the Switch
================

Setup network settings
----------------------

The next step is to check and setup network settings:

.. code::

    (Routing) #show network

.. code-block:: txt

    IP Address..................................... 0.0.0.0
    Subnet Mask.................................... 0.0.0.0
    Default Gateway................................ 0.0.0.0
    Burned In MAC Address.......................... 60:EB:69:A9:22:05
    Locally Administered MAC Address............... 00:00:00:00:00:00
    MAC Address Type............................... Burned In
    Network Configuration Protocol Current......... None
    Management VLAN ID............................. 1

    (Routing) #


All ports are turned off, so no DHCP assignment received yet.
To turn on all ports:

.. code::

    (Routing) #configure

    (Routing) (Config)#no shutdown all

    (Routing) (Config)#exit

    (Routing) #show port all


.. code-block:: txt

                   Admin   Physical   Physical   Link   Link    LACP   Actor
     Intf   Type    Mode    Mode       Status   Status  Trap    Mode   Timeout
    ------ ------ ------- ---------- ---------- ------ ------- ------ --------
    0/1           Enable   Auto       1000 Full  Up     Enable Enable long
    0/2           Enable   Auto                  Down   Enable Enable long
    0/3           Enable   Auto                  Down   Enable Enable long
    0/4           Enable   Auto                  Down   Enable Enable long
    0/5           Enable   Auto                  Down   Enable Enable long
    0/6           Enable   Auto                  Down   Enable Enable long
    0/7           Enable   Auto                  Down   Enable Enable long
    0/8           Enable   Auto                  Down   Enable Enable long
    0/9           Enable   Auto                  Down   Enable Enable long
    0/10          Enable   Auto                  Down   Enable Enable long
    0/11          Enable   Auto                  Down   Enable Enable long
    0/12          Enable   Auto                  Down   Enable Enable long
    0/13          Enable   Auto                  Down   Enable Enable long
    0/14          Enable   Auto                  Down   Enable Enable long
    0/15          Enable   Auto                  Down   Enable Enable long
    0/16          Enable   Auto                  Down   Enable Enable long
    0/17          Enable   Auto                  Down   Enable Enable long
    0/18          Enable   Auto                  Down   Enable Enable long

    --More-- or (q)uit


The next step is to setup the switch IP address:

.. code::

    (Routing) #network parms 192.168.50.91 255.255.255.0 192.168.0.1


Check the network settings:

.. code::

    (Routing) #show network

.. code-block:: txt

    IP Address..................................... 192.168.50.91
    Subnet Mask.................................... 255.255.255.0
    Default Gateway................................ 192.168.50.91
    Burned In MAC Address.......................... 60:EB:69:A9:22:05
    Locally Administered MAC Address............... 00:00:00:00:00:00
    MAC Address Type............................... Burned In
    Network Configuration Protocol Current......... None
    Management VLAN ID............................. 1

    (Routing) #


Write down the changes to router NVRAM. Otherwise all changes will be lost upon switch reset. This operation may take a few minutes. Management interfaces will not be available during this time.

.. code::

    (Routing) #write memory

.. code-block:: txt

    Are you sure you want to save? (y/n) y

    Config file 'startup-config' created successfully .

    Configuration Saved!

    (Routing) #


Enabling SSH access
-------------------

To enable the SSH access:

.. code::

    (Routing) >enable
    Passsword:

    (Switching) #ip ssh server enable



Enabling WEB inteface
---------------------

To enable the Web interface:

.. code::

    (Routing) >enable
    Passsword:

    (Switching) #ip http server


Setup port mirroring
--------------------




