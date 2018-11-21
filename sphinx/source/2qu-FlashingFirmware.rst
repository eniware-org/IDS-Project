.. _flashing-firmware:

2.2. Flashing Routing Firmware
==============================

This is a guide to install new firmware with CLI, confirm image, switch active boot image, turn on all ports, and finally don't forget to "write memory", this will set the configuration file.

Requirements:
 - Running TFTP server;
 - Downloaded firmware for `Quanta LB4m <https://puck.nether.net/~jared/lb4m/>`_ - firmware image ``lb4m.1.1.1.8.bin`` (You can use this `direct download link <https://puck.nether.net/~jared/lb4m/lb4m.1.1.1.8.bin>`_;
 - Client for serial port connection with the switch (for example `PuTTY <https://putty.org/>`_).

Below it will be use an address 192.168.50.2 for access to the TFTP server. The firmware name in this example is ``firmware.bin``. Use appropriate settings for your specific case.

Using PuTTY connect to the serial port (console port) of the switch using the following parameters ``115200,8,N,1``.
Hit enter once and you should get the prompt. Use ``admin`` as a login name, the default password is blank. Hit enter again and you should get the ``(Switching)>`` prompt. Then type the command ``enable`` and the switsh will go into command mode (the password is blank again).


Use the below command to copy from your tftp server:

.. code-block:: guess

 (Switching) #copy tftp://192.168.50.2/firmware.bin image2

 Mode........................................... TFTP
 Set Server IP.................................. 192.168.50.2
 Path........................................... ./
 Filename....................................... firmware.bin
 Data Type...................................... Code
 Destination Filename........................... image2

 Management access will be blocked for the duration of the transfer
 Are you sure you want to start? (y/n) y
 TFTP code transfer starting
 4208280 bytes transferred...
 File contents are valid. Copying file to flash...


 File transfer operation completed successfully.

 (Switching) #

.. note:: To be sure that the unit will accept the file, you should check the names of the firmware slots. Use the ``bootvar`` command to find out what they are named. In our case they are  ``image1`` in slot 1 and ``image2`` in slot 2. .

Now image2 slot contains 1.1.0.8 Router version:

.. code::

    (Switching) #show bootvar

.. code-block:: guess

    Image Descriptions

     image1 : default image
     image2 :


    Images currently available on Flash

    --------------------------------------------------------------------
     unit      image1      image2     current-active        next-active
    --------------------------------------------------------------------

        1     5.13.12.14    1.1.0.8        image1            image1

    (Switching) #


Switch system to boot from “image2” firmware:

.. code::

    (Switching) #boot system image2

.. code-block:: guess

    Activating image image2 ..

    (Switching) #show bootvar

    Image Descriptions

     active : default image
     backup :

     --------------------------------------------------------------------
       unit      image1      image2     current-active        next-active
     --------------------------------------------------------------------

        1     5.13.12.14    1.1.0.8        image1            image1

     (Switching) #


Now you should redoot the switch:

.. code::

    (Switching) #reload


.. code-block:: guess

    Are you sure you would like to reset the system? (y/n) y


    System will now restart!


    Boot Menu Version: 28 Apr 2008

    Calculating CRC of active image...done...
    Select an option. If no selection in 2 seconds then
    operational code will start.

    1 - Start operational code.
    2 - Start Boot Menu.
    Select (1, 2):


    Operational Code Date: Wed May 13 12:16:52 2009
    Uncompressing.....

                           50%                     100%
    |||||||||||||||||||||||||||||||||||||||||||||||||||
    Attaching interface lo0...done

    Adding 48962 symbols for standalone.


                    VxWorks

    Copyright 1984-2002  Wind River Systems, Inc.

                CPU: Motorola E500 : Unknown system version
       Runtime Name: VxWorks
    Runtime Version: 5.5.1
        BSP version: 1.2/0
            Created: May 13 2009, 11:57:16
      WDB Comm Type: WDB_COMM_NETWORK
                WDB: Ready.



    PCI unit 0: Dev 0xb514, Rev 0x01, Chip BCM56514_A0, Driver BCM56514_A0
    PCI unit 1: Dev 0xb514, Rev 0x01, Chip BCM56514_A0, Driver BCM56514_A0
    SOC unit 0 attached to PCI device BCM56514_A0
    SOC unit 1 attached to PCI device BCM56514_A0

    (Unit 1)>

    NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE

    Unauthorized access and/or use prohibited.  All access and/or use subject to
    monitoring.

    NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE   NOTICE

    Applying configuration, please wait ...

    Applying Global configuration, please wait ...

    Applying Interface configuration, please wait ...


Login into the switch and check if the new firmware is running:

.. code::

    User:admin
    Password:

    (Routing) >enable
    Password:

    (Routing) #show bootvar

    Image Descriptions

    image1 : default image
    image2 :


    Images currently available on Flash

    --------------------------------------------------------------------
     unit      image1      image2     current-active        next-active
    --------------------------------------------------------------------

        1     1.1.0.8    5.13.12.14        image2             image2

    (Routing) #


The system is now running the new firmware.

.. warning:: It is highly recommended to change the default admin password for the switch!