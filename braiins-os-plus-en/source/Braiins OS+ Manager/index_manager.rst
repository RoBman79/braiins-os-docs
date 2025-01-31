
.. _manager:

###################
Braiins OS+ Manager
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+ Manager is a cloud-based platform that allows you to remotely configure your mining devices running the Braiins OS+ firmware as well as continuously receive data about their performance.

The data are being sent over the Stratum V2 protocol and using the same channel that is used for collecting the dev fee, thus not burdening your network with another connection.

The main object in the Braiins OS+ Manager is a group of devices called Farm. Every Farm has its Farm ID. It is a string you have to set on your Braiins OS+ devices if you wish to connect them to the Farm. Once connected, the devices send their performance data to the Braiins OS+ Manager every 120 seconds.

Every Farm has its configuration that is being pushed to the devices immediately after each save. Since the same config is applied to all devices in a Farm, **we strongly recommend that you create a separate farm for each device type** or simply a group of devices (even of the same type) you wish to configure differently.

*******
Sign Up
*******

To use Braiins OS+ Manager, simply `signup here <https://manager.braiins.com/#/register>`_.

After you enter your email address, we will send you confirmation email. After following the link in the email, you will be prompted to choose your password and setup two-factor authentication.

*************
Create a Farm
*************

Once you are logged, start with creating a Farm:

  - Open the Farm creation dialogue by clicking on the '+' symbol.
  - Choose a name you wish to use for your farm. You can change the name later.
  - Enter mining credentials. You will be able to change the credentials later as well as add other pools.
  - The Farm ID for your farm has been created.

The Farm ID is a string you have to set on your Braiins OS+ devices you wish to connect to the Farm. 

*******************
Connect to the Farm
*******************

In order to connect a device to your Braiins OS+ Manager Farm, you need to:

  - run Braiins OS+ 21.04 or later running on the selected devices, 
  - set the Farm ID (bos_mgmt_id) on the selected devices.

Those steps can be done using BOS Toolbox with the following steps.
**Important note:** Download the latest version of BOS Toolbox `from here <https://manager.braiins.com/#/register>`_, before using the commands bellow.

.. raw:: html

   <details>
   <summary><a>Setting Farm ID during Braiins OS+ installation</a></summary>
   <p></p>

If your devices don’t run Braiins OS+ yet, you can install the Braiins OS+ and set the Farm ID in one simple step by using BOS Toolbox’s install command with `--bos-mgmt-id` argument.
Replace the “HOSTS” placeholder with an IP address or with a text-file containing multiple IPs (one per line, for batch installation). Replace “FARM_ID” with your Farm ID.
   
::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

.. raw:: html

   <p></p>
   </details>

.. raw:: html

   <details>
   <summary><a>Update existing Braiins OS+ installation and set Farm ID</a></summary>
   <p></p>

If your devices are already running Braiins OS+, proceed with the following steps:

::

    #Windows
    bos-toolbox.bat update --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

.. raw:: html

   <p></p>
   </details>
   <p></p>

******************
Configure the Farm
******************

**Workername Setup**

There are three different options on how the devices included in a Farm can identify themselves in the Manager device list and on the pool side:

  - Per Device (FARMNAME_IP4) - default - workername consists of the name of the Farm and fourth segment of IP address of a device
  - Per Device (FARMNAME_IP3_IP4) - in addition, the third segment of the IP address is also included
  - Single (FARMNAME) - All devices use the same workername (name of the Farm). This means that the hash rate is aggregated to one worker on the pool side.

The workername mode may be changed anytime.

**Mining Configuration**

The mining configuration available in the “Configuration” tab includes a sub-set of `general Braiins OS\+ configuration <https://docs.braiins.com/os/plus-en/Configuration/index_configuration.html>`_ available on individual devices. For example, options for individual hash chains are not available here since it only makes sense from an individual device perspective. Other than that, all the important options to configure tuning, target temperatures or dynamic power scaling are present.

The configuration requires you to input credentials for at least one pool (which is done during the farm creation process). Other configuration fields are optional. If you don't provide any value, each Device in a Farm will simply use its default. It is behavior equivalent to leaving the config of a single Braiins OS+ device empty.

Once you click on the Save button, the new configuration is propagated to the devices included in the Farm almost immediately - typically within one second.

**Local changes**

Local changes (on the miner) are always overwritten by the the Manager. If you wish to take control of the device, disconnect it from the Farm first.

************************
Disconnect from the Farm
************************

If you wish to disconnect the devices from the Farm and configure them individually, you can do it by simply removing the bos_mgmt_id file from selected devices. For multiple devices, this can be done using BOS Toolbox as follows:

::

    #Windows
    bos-toolbox.bat command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"
    
    #Linux
    ./bos-toolbox command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"
