Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

Package Repository
------------------
TLP and ThinkPad-related packages below are available via the official Debian
repositories.

Newer TLP packages may be provided via `Debian backports`_. Add the following line
to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian DIST-backports main

Replace `DIST` with the codename of your installed Debian version.

Update package data: ::

    sudo apt update

Package Installation
--------------------
Install the following packages

* **tlp** *(main)* – Power saving
* **tlp-pd** *(main)* – optional – Select :ref:`profile <intro-profiles>` with a mouse click *(Version 1.9 and newer)*
* **tlp-rdw** *(main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command:

*Version 1.9 and newer* ::

    sudo apt install tlp tlp-pd tlp-rdw

*Version 1.8 and older* ::

    sudo apt install tlp tlp-rdw

For `Debian backports`_ use: ::

   sudo apt -t DIST-backports install tlp tlp-pd tlp-rdw

Replace `DIST` with the codename of your installed Debian version.

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    As of version 5.17, the Linux kernel in combination with TLP 1.5 or later
    offers full battery care support (i.e. charge thresholds and recalibration)
    for ThinkPads from the `Sandy Bridge` generation (2011) onwards.
    Debian Bookworm meets this requirement with its kernel 6.1, the even newer Debian
    Trixie kernel 6.12 anyway.

    **An external kernel module (also referred to as "out-of-tree" module)
    is not required in these cases, and the following steps are not necessary.
    However, if your model is from the `Sandy Bridge` generation (2011) or older,
    read on.**

Only if the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
shows the line

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration

then install the required package

* **tp-smapi-dkms** *(main)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads prior to the `Sandy Bridge`
  generation (2011) and specific :command:`tlp-stat -b` output up to that generation

either with your favorite package manager or the command ::

    apt install tp-smapi-dkms

.. note::

    * Refer to `Debian backports`_ for setup instructions
    * You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian backports`: https://backports.debian.org/Instructions/
