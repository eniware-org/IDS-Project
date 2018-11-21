.. _snort-install:

3.2. NIDS Server Configuration
===============================


3.2.1. LogStash Configuration
---------------------------------

LogStash has a separate configuration file for each datasource it will feed into the ElasticSearch database.
In our case, the following configuration files are used:

* snoort.conf
* modbus.conf

To edit the configuration file, use a text editor, such as **nano** for example:

.. code:: 

  sudo nano /etc/logstash/conf.d/snoort.conf

Enter the following for the ``snoort.conf`` configuration file content:

.. code::

  input {
    file {
      path => "/var/log/snort/alerts.json"
      codec => "json"
    }
  }
  
  output {
    elasticsearch {
      hosts =>  "192.168.40.122:9200"
      index => "snort_alerts"
    }
  }

  
The second configuration file to be edited  is ``modbus.conf``: 

.. code:: 

  sudo nano /etc/logstash/conf.d/modbus.conf

Enter the following into the ``modbus.conf`` content:

.. code::

  input {
    file {
      path => "/var/log/tshark/modbus.json"
      codec => "json"
    }
  }
  
  output {
    elasticsearch {
      hosts =>  "192.168.40.122:9200"
      index => "modbus_traffic"
    }
  }

  
3.2.2. Starting LogStash, TShark & Snort 
----------------------------------------------
  
Now start the LogStash service:

.. code::

  sudo service logstart logstart

  
  
To start the Tshark use the following command options:

.. code::

  sudo tshark -i eth9 -O modbus -Y modbus -T ek -J modbus -j modbus > /var/log/modbus.json

In this setup the network interface ``eth9`` is connected to a **SPAN port** of a switch and only the Modbus protocol of the application layer needs to be decoded. 

For the Modbus/TCP Layer to be decoded the command to run TShark is:

.. code:: 

  sudo tshark -i eth9 -O modbus -Y modbus -T ek -J "mbtcp modbus" > /var/log/modbus.json

  
To start the Snort use the following command options:

.. code:: 

  sudo snort -i eth9 -c /etc/snort/snort.conf -l /var/log/snort/ -h 192.68.50.0/24 -A Console


.. Hint:: **Additional Snort rules for some sanity checks on the MODBUS protocol** could  be downloaded from https://github.com/digitalbond/Quickdraw-Snort/blob/master/modbus.rules
  The  ``modbus.rules`` file should be placed in the ``/etc/snort/rules/``.


To start the tool which converts Snort logs from **Unified2** format to **JSON** use the following command:

.. code::

  sudo idstools-u2json --verbose --snort-conf /etc/snort/snort.conf --directory /var/log/snort \
      --prefix snort.log --follow --output /var/log/snort/alerts.json


