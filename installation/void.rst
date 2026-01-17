Void Linux
==========

Package Installation
--------------------

Packages are available in the offical repositories:

* **tlp** – Power saving
* **tlp-pd** – optional – Select :ref:`profile <intro-profiles>` with a mouse click *(Version 1.9 and newer)*
* **tlp-rdw** – optional – :doc:`/settings/rdw`

Install them with the command:

*Version 1.9 and newer* ::

    sudo xbps-install tlp tlp-pd tlp-rdw

If you have installed tpl-rwd, you also need to install NetWorkManager ::

    sudo xbps-install NetworkManager

Enabling the Services
-------------
To complete the installation you must enable TLP's service(s): ::

    sudo ln -s /etc/sv/tlp /var/service

*For version 1.9 and newer* with tlp-pd, additionally: ::

    sudo ln -s /etc/sv/tlp-pd /var/service

Using the :doc:`/settings/rdw` (tlp-rdw) requires NetworkManager package and its service running.

Before enabling the NetworkManager service using the command below,
please read the `Void Linux installation documentation - NetworkManager <https://docs.voidlinux.org/config/network/networkmanager.html>`_.
There are a few steps required to get NetworkManager working properly. ::

    sudo ln -s /etc/sv/NetworkManager /var/service

.. seealso::
    * FAQ: :ref:`faq-ppd-conflict`

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    Void Linux (at the time of writing) provides kernel 6.18 (linux) or 6.12 (linux-lts).
    In combination with TLP 1.5 or newer this enables full battery care support
    (i.e. charge thresholds and recalibration) for ThinkPads from the `Sandy Bridge`
    generation (2011) onwards.

    **An external kernel module (also referred to as "out-of-tree" module)
    is not required in this case, and the following steps are not necessary.
    However, if your model is from the `Sandy Bridge` generation (2011) or older,
    read on.**

Only if the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
shows the line

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration

then install the approriate package with the command ::

    sudo xbps-install tp_smapi-dkmsk

.. note::

    * You must disable Secure Boot to use the ThinkPad specific packages
