Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

Package Repository
------------------
TLP and ThinkPad-related packages below are available via the official Debian
repositories.

Newer TLP packages are provided via `Debian backports`_. Add a new file
**/etc/apt/sources.list.d/debian-backports.sources**:  ::

    Types: deb deb-src
    URIs: http://deb.debian.org/debian
    Suites: DIST-backports
    Components: main
    Enabled: yes
    Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Replace `DIST` with the codename of your installed Debian version.

Then run: ::

    sudo apt update

Package Installation
--------------------
Install the following packages

* **tlp** *(main)* – Power saving
* **tlp-pd** *(main)* – optional – Select :ref:`TLP profile <intro-profiles>` with a mouse click *(Version 1.9 and newer)*
* **tlp-rdw** *(main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command:

*Version 1.9 and newer* ::

    sudo apt install tlp tlp-pd tlp-rdw

*Version 1.8 and older* ::

    sudo apt install tlp tlp-rdw

For `Debian backports`_ use: ::

   sudo apt -t DIST-backports install tlp tlp-pd tlp-rdw

Replace `DIST` with the codename of your installed Debian version.

.. note::

    Installing TLP removes the default power management package **power-profiles-daemon**.
    Remember to reinstall it if you decide to uninstall TLP.

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    Currently supported Debian kernel versions offer full battery care support
    – i.e. charge thresholds and recalibration – for ThinkPads from the `Sandy Bridge`
    generation (2011) onwards. An external kernel module (also referred to as "out-of-tree" module)
    is *not required*, and the following steps are not necessary.
    However, if your model is from the `Sandy Bridge` generation (2011) or older,
    read on.

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

    You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian backports`: https://backports.debian.org/Instructions/
