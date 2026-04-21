Ubuntu
======
.. rubric:: Scope:

* Officially supported Ubuntu releases
* Corresponding Linux Mint releases *but not* LMDE – refer to
  :doc:`debian` instead

Package Repository
------------------
Add the `TLP PPA`_ to your package sources with the commands: ::

   sudo add-apt-repository ppa:linrunner/tlp
   sudo apt update

.. note::

   TLP and ThinkPad-related packages below are available via the official Ubuntu
   repository. Nevertheless it is recommended to use the `TLP PPA`_ to stay with the
   latest TLP version.

Package Installation
--------------------
Install the packages

* **tlp** *(PPA or main)* – Power saving
* **tlp-pd** *(PPA or main)* – optional – Select :ref:`TLP profile <intro-profiles>` with a mouse click *(Version 1.9 and newer)*
* **tlp-rdw** *(PPA or main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command:

*Version 1.9 and newer* ::

    sudo apt install tlp tlp-pd tlp-rdw

*Version 1.8 and older* ::

    sudo apt install tlp tlp-rdw

.. note::

    Installing TLP from the PPA removes the default power management package **power-profiles-daemon**.
    Remember to reinstall it if you decide to uninstall TLP.


.. _ubuntu-dkms-fail:

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    Currently supported Ubuntu kernel versions (22.04 upgraded to the 6.8 HWE kernel)
    offer full battery care support – i.e. charge thresholds and recalibration – for
    ThinkPads from the `Sandy Bridge` generation (2011) onwards.
    An external kernel module (also referred to as "out-of-tree" module)
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

.. warning::

    In *Ubuntu 24.04* and newer the `tp-smapi-dkms` package from the official
    repositories is broken and does neither build nor install the kernel module.

    Workaround: download the working package from
    `Debian Sid <https://packages.debian.org/sid/amd64/tp-smapi-dkms/download>`_
    and install manually ::

        wget -P /tmp https://ftp.debian.org/debian/pool/main/t/tp-smapi/tp-smapi-dkms_0.45-1_amd64.deb
        sudo apt install /tmp/tp-smapi-dkms_0.45-1_amd64.deb

    and prevent Ubuntu from updating to its broken package ::

        sudo apt-mark hold tp-smapi-dkms


.. seealso::

    * `TLP PPA`_ - Contains latest TLP packages for Ubuntu
    * `Debian Bug #1034233 <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1034233>`_ -
      Cause of the not enabled service with the 1.5 packages
    * For systems with Secure Boot enabled, please refer to
      `DKMS Secure Boot <https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS>`_
    * :ref:`faq-ppd-conflict` - FAQ

.. _`TLP PPA`: https://launchpad.net/~linrunner/+archive/ubuntu/tlp
