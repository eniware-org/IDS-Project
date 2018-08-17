Flashing Routing Firmware
=========================

.. code-block:: txt

   --------------------------------------------------------------------
   unit      active      backup     current-active        next-active
   --------------------------------------------------------------------

    1     1.1.0.8  5.13.12.14            1.1.0.8         5.13.12.14

   (Switching) #

   (Switching) #boot ?


!Guide to install new firmware with CLI, confirm image, switch active boot image, turn on all
!ports, and finally don't forget to "write memory", this will set the configuration file.

!Use CLI with serial cable and settings on com port of 9600,8,N,1 (without any data checking)
!Hit enter a couple times, should get prompt, then
!use "admin" as login name
!default password is blank, so just enter, should get the (Switching)> prompt
!then type command "enable", will put you into command mode, password blank again
!>>>>>if ever any questions on a command, or for a list of commands use the ? symbol then enter
!>>>>>FASTEST way to transfer image file (if no web page) is to use TFTP server, download a free one
!!setup on either the machine you are using, or another on the same network, make sure to 
!!clear the TFTP program through the windows firewall, otherwise the switch won't see it.
!Once TFTP is ready to go, copy over the image file, and rename to something simple, I used xcode.bin

!use the below command to copy from your tftp server (this one is at 10.0.5.250, 
!with root directory containing flash file "xcode.bin")
!>>>>>>one trick to making sure the unit will accept the file, is to make sure you know the names of the firmware
!>>>>>>slots, use the "bootvar" command to find out what they are called.  Mine started out as
!>>>>>>"active" in slot 1 and "backup" in slot 2, but after image flash and reboot to the new router firmware
!>>>>>>they changed to "image1" and "image2"

(Switching) #copy tftp://10.0.5.250/xcode.bin backup

Mode........................................... TFTP
Set Server IP.................................. 10.0.5.250
Path........................................... ./
Filename....................................... xcode.bin
Data Type...................................... Code
Destination Filename........................... backup

Management access will be blocked for the duration of the transfer
Are you sure you want to start? (y/n) y
TFTP code transfer starting
4208280 bytes transferred...
File contents are valid. Copying file to flash...


File transfer operation completed successfully.

(Switching) #

! Now backup image contains 5.13.12.14 Router version

(Switching) #show bootvar

Image Descriptions

 active : default image
 backup :


 Images currently available on Flash

--------------------------------------------------------------------
 unit      active      backup     current-active        next-active
--------------------------------------------------------------------

    1     1.1.0.8  5.13.12.14            1.1.0.8            1.1.0.8

(Switching) #

! Switch system to boot from "Backup" image firmware

(Switching) #boot system backup
Activating image backup ..

(Switching) #show bootvar

Image Descriptions

 active : default image
 backup :


 Images currently available on Flash

--------------------------------------------------------------------
 unit      active      backup     current-active        next-active
--------------------------------------------------------------------

    1     1.1.0.8  5.13.12.14            1.1.0.8         5.13.12.14

(Switching) #

(Switching) #boot ?

autoinstall              Used to enable/disable autoinstall operational mode
                         on the switch.
host                     Used to enable/disable autoinstall persistent mode on
                         the switch.
system                   Marks the given image as active for subsequent
                         re-boots.

(Switching) #boot system backup
Activating image backup ..

(Switching) #show bootvar

Image Descriptions

 active : default image
 backup :


 Images currently available on Flash

--------------------------------------------------------------------
 unit      active      backup     current-active        next-active
--------------------------------------------------------------------

    1     1.1.0.8  5.13.12.14            1.1.0.8         5.13.12.14

(Switching) #reload ?

<cr>                     Press enter to execute the command.

(Switching) #reload

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

    1     1.1.0.8  5.13.12.14             image2             image2

(Routing) #

!system now running on new image
!Check DHCP assignment


(Routing) #show network

IP Address..................................... 0.0.0.0
Subnet Mask.................................... 0.0.0.0
Default Gateway................................ 0.0.0.0
Burned In MAC Address.......................... 60:EB:69:A9:22:05
Locally Administered MAC Address............... 00:00:00:00:00:00
MAC Address Type............................... Burned In
Network Configuration Protocol Current......... None
Management VLAN ID............................. 1

(Routing) #

>>all ports turned off, so no DHCP assignment received yet
>>To turn on all ports

(Routing) #configure

(Routing) (Config)#no shutdown all

(Routing) (Config)#exit

(Routing) #show port all

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


(Routing) #write memory

This operation may take a few minutes.
Management interfaces will not be available during this time.

Are you sure you want to save? (y/n) y

Config file 'startup-config' created successfully .

Configuration Saved!

(Routing) #

