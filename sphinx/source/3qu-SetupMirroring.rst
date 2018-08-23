Setup the Switch
================

.. _setup-switch-IP:

Setup Network Settings
----------------------

The next step is to check and setup network settings:

.. code::

    (Routing) #show network

.. code-block:: guess

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


.. code-block:: guess

                   Admin   Physical   Physical   Link   Link    LACP   Actor
     Intf   Type    Mode    Mode       Status   Status  Trap    Mode   Timeout
    ------ ------ ------- ---------- ---------- ------ ------- ------ --------
    0/1           Enable   Auto       1000 Full  Up     Enable Enable long
    0/2           Enable   Auto       100 Full   Up     Enable Enable long
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

    (Routing) #network parms 192.168.50.91 255.255.255.0 192.168.50.1


Check the network settings:

.. code::

    (Routing) #show network

.. code-block:: guess

    IP Address..................................... 192.168.50.91
    Subnet Mask.................................... 255.255.255.0
    Default Gateway................................ 192.168.50.91
    Burned In MAC Address.......................... 60:EB:69:A9:22:05
    Locally Administered MAC Address............... 00:00:00:00:00:00
    MAC Address Type............................... Burned In
    Network Configuration Protocol Current......... None
    Management VLAN ID............................. 1

    (Routing) #


.. _enablig-ssh:

Enabling SSH Access
-------------------

To enable the SSH access:

.. code::

    (Routing) >enable
    Passsword:

    (Routing) #ip ssh server enable


.. _enablig-web:

Enabling WEB Inteface
---------------------

To enable the Web interface:

.. code::

    (Routing) >enable
    Passsword:

    (Routing) #ip http server


Saving Configuration Changes
----------------------------

.. warning:: **Write down the changes to router NVRAM.** Otherwise all changes will be lost upon switch reset! This operation may take a few minutes. Management interfaces will not be available during this time.

    .. code::

        (Routing) #write memory

    .. code-block:: guess

        Are you sure you want to save? (y/n) y

        Config file 'startup-config' created successfully .

        Configuration Saved!

        (Routing) #



.. _port-mirroring:

Configuring Port Mirroring
==========================

What is Port Mirroring?
-----------------------

The port mirroring feature allows the switch to copy the network traffic from one or several source port to a destination port. The destination port can mirror packets transmitted or
received by the source port(-s) or both. Only one port can be set as a destination port for the mirroring, but the source port can be one or more.


.. _port-mirroring-cli:

Configuring Port Mirroring via CLI
----------------------------------

Setting up a Port Mirroring Session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following commands enable port mirroring session and configure source and destination ports.

.. code::

  (Routing) #Config
  (Routing) (Config) #monitor session 1 mode
  (Routing) (Config) #monitor session 1 source interface 0/45 ?
  <cr>                     Press Enter to execute the command.
  rx                       Monitor ingress packets only.
  tx                       Monitor egress packets only.
  (Routing) (Config) #monitor session 1 source interface 0/45
  (Routing) (Config) #monitor session 1 destination interface 0/46
  (Routing) (Config) #exit


Show the Port Mirroring Session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To show the port mirroring session:

.. code::

  (Routing) #show monitor session 1

.. code-block:: guess

  Session ID   Admin Mode   Probe Port   Mirrored Port   Type
  ----------   ----------   ----------   -------------   -----
  1            Enable       0/46          0/45             Rx,Tx


  Monitor session ID “1” - “1” is a hardware limitation.


Show the Status of All Ports
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To show the port mirroring session:

.. code::

        (Routing) #show port all

.. code-block:: guess

                   Admin   Physical   Physical   Link   Link    LACP   Actor
     Intf   Type    Mode    Mode       Status   Status  Trap    Mode   Timeout
    ------ ------ ------- ---------- ---------- ------ ------- ------ --------
    0/1           Enable   Auto       1000 Full  Up     Enable Enable long
    0/2           Enable   Auto       100 Full   Up     Enable Enable long
    0/3           Enable   Auto                  Down   Enable Enable long

     --More-- or (q)uit

    0/40          Enable   Auto                  Down   Enable Enable long
    0/41          Enable   Auto                  Down   Enable Enable long
    0/42          Enable   Auto                  Down   Enable Enable long
    0/43          Enable   Auto                  Down   Enable Enable long
    0/44          Enable   Auto                  Down   Enable Enable long
    0/45  Mirror  Enable   Auto       100 Full   Up     Enable Enable long
    0/46  Probe   Enable   Auto       1000 Full  Up     Enable Enable long
    0/47          Enable   Auto                  Down   Enable Enable long
    0/48          Enable   Auto                  Down   Enable Enable long




Show the Status of the Source and Destination Ports
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use this command for a specific port. The output shows whether the port is the mirror or the probe port, what is enabled or disabled on the port, etc.

.. code::

    (Ethernet Fabric) #show port 0/45

.. code-block:: guess

                     Admin    Physical  Physical  Link     Link     LACP
    Intf   Type      Mode     Mode      Status    Status   Trap     Mode
    ----   ----      ------   --------  --------  ------   ----     ----
    0/45   Mirror    Enable   Auto      100 Full   Up      Enable   Enable


.. code::

    (Ethernet Fabric) #show port 0/46

.. code-block:: guess

                     Admin    Physical  Physical  Link     Link     LACP
    Intf   Type      Mode     Mode      Status    Status   Trap     Mode
    ----   ----      ------   --------  --------  ------   ----     ----
    0/46   Probe     Enable   Auto      1000 Full  Up      Enable   Enable



.. _port-mirroring-web:

Configuring Port Mirroring via Web Interface
--------------------------------------------

.. note:: Web interface needs to be enabled - please see the section :ref:`enablig-web`

1. Open the logon page in your browser using the switch IP address (according to the network settings you made - see section :ref:`setup-switch-IP`). In this case the address is ``192.168.50.91``.
For the user name type ``admin`` and fot the password - leave blank:

.. image:: /images/web-interface-1.PNG
   :alt: Web interface - login page

2. After entering the Web interface, expand the System tree of the Navigation field to the ``System/Port/Multiple port mirroring``.

.. image:: /images/web-interface-2.PNG
   :alt: System/Port/Multiple port mirroring

3. Setting a Multiple port mirroring is done by pressing a button ``Add source port`` and entering the following settings:

* ``Source port = 0/45`` for this case;
* ``Direction = Tx and Rx``

The entered settings will be confirmed by clicking on the button ``Add``.

.. image:: /images/web-interface-3.PNG
   :alt: Setup source port

4. The final step is to enable the port mirroring by setting the ``Mode = Enable``, set the ``Destination port = 0/4`` (according to this example) and confirmt by pressing the ``Submut`` button.

.. image:: /images/web-interface-4.PNG
   :alt: Setup destination port and activate the port mirroring