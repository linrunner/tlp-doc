Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

.. note::

    Execute the commands below in a root shell or with a preceding :command:`sudo`.

Package Repository
------------------
TLP and ThinkPad-related packages below are available via the official Debian
repositories.

Newer TLP packages may be provided via `Debian backports`_. Add the following line
to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian DIST-backports main

Replace `DIST` with the codename of your installed Debian version.

Update package data: ::

    apt update

Package Installation
--------------------
Install the following packages

* **tlp** *(main)* – Power saving
* **tlp-rdw** *(main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command: ::

    apt install tlp tlp-rdw

For `Debian backports`_ use: ::

    apt -t DIST-backports install tlp tlp-rdw

Replace `DIST` with the codename of your installed Debian version.

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    As of version 5.17, the Linux kernel in combination with TLP 1.5 or later
    offers full battery care support (i.e. charge thresholds and recalibration)
    for ThinkPads from model year 2011 onwards. Debian Bookworm meets this
    requirement with its kernel 6.1, the even newer Debian Sid kernel as well.

    **An external kernel module (also referred to as "out-of-tree" module)
    is not required in these cases, and the following steps are not necessary.
    However, if your model is from 2011 or older, read on.**

Only if the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
shows the line

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration

then install the required package

* **tp-smapi-dkms** *(main)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011

either with your favorite package manager or the command ::

    apt install tp-smapi-dkms

.. note::

    * Refer to `Debian backports`_ for setup instructions
    * You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian backports`: https://backports.debian.org/Instructions/

