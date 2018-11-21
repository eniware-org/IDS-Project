.. _nids-install:

3.1. Install the Core Components of the NIDS Server
===================================================

**Requirements:**

* Freshly installed Ubuntu 16.04 LTS Server Edition
* Server with two network interfaces



.. _ubuntu-install:

3.1.2. Ubuntu 16.04 LTS Server Installation
---------------------------------------------

The first step is to install `Ubuntu 16.04 LTS Server Edition <http://releases.ubuntu.com/16.04/>`_.
The complete installation guide can be found here: `Ubuntu Server Guide <https://help.ubuntu.com/16.04/serverguide/index.html>`_.



.. _core-install:

3.1.3. Install the Core Components
----------------------------------

Once a server system is already installed, it can proceed to install the core components.
To do this, log on to the server and do the following:

Perform an update after logging in for the first time:

.. code::

    sudo apt-get update && sudo apt-get upgrade


Changing the Network Interfaces Names Back to ethX
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::
  This step is purely for convenience. You can skip this step if you want, but in this case don't forget to change the interfaces names accordingly to your system.

Open the GRUB config file and find the line which starts with ``GRUB_CMDLINE_LINUX`` and add ``"net.ifnames=0 biosdevname=0"`` to it:

.. code::

    sudo nano /etc/default/grub

The result should be something like:

.. code::

    GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"

After that update GRUB and reboot Ubuntu:

.. code::
    
    sudo update-grub
    sudo reboot


.. _net-int-snort:

Setting up the Network Interfaces for Snort
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One of the network interfaces is going to be used for *managment and remote access*, so it will be configured with a static IP address. The other network interface is going to be used by Snort for *inspection of the mirrored traffic*. The interface needs to operate in promiscuous mode for Snort to be able to use it.
In this example ``eth1`` is used for managment and ``eth9`` is used by Snort.

Your ``/etc/network/interface`` file should look something like:

.. code::

    sudo nano /etc/network/interfaces

.. code::

    auto eth1
    iface eth1 inet static
    address 192.168.50.21
    netmask 255.255.255.0
    gateway 192.168.50.1
    dns-nameservers 8.8.8.8 4.4.4.4

    auto eth9
    iface eth9 inet manual



Install SSH Server
^^^^^^^^^^^^^^^^^^

Install SSH server for remote access to the Snort machine:

.. code::

    sudo apt-get install -y openssh-server


.. note::
    It is required to setup the SSH client not to forward your locale to the remote host. This is done in order to avoid side effects on scripts and programs that rely on locale being the default for the machine. This is achieved with the following command:
    .. code::

        sudo sed -e '/SendEnv/ s/^#*/#/' -i /etc/ssh/ssh_config

    You must log out and login again for the changes to take place.




Install Logstash
^^^^^^^^^^^^^^^^^^^

.. note::
    Logstash needs Java version 8 at least.

.. code::

    sudo apt-get install openjdk-8-jre

To install the script that is going to convert the Snort log files to JSON enter the following:

.. code::

    sudo apt-get install python-pip
    pip install idstools

.. code::

    sudo apt-get install snort


To install Logstash 6.3 first we need to add  it's repository to the repositories list:

.. code::

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.3.list
    sudo apt-get update

After that Logstash can be isntalled:

.. code::

    sudo apt-get install -y logstash


Install Snort and TShark
^^^^^^^^^^^^^^^^^^^^^^^^^

To install the latest stable version of Snort and TShark enter the following commands:


.. code::

    sudo apt-get install -y snort
    sudo apt-get install --y tshark

The next step is to create a folder to store the decoded traffic, which will be analyzed by TShark:

.. code::

   sudo mkdir /var/log/tshark

