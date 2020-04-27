Ubuntu
======
.. rubric:: Scope:

* Officially supported Ubuntu releases
* Corresponding Linux Mint releases *but not* LMDE – refer to
  :doc:`debian` instead

Package Repository
------------------
Add the TLP-PPA to your package sources with the commands: ::

   sudo add-apt-repository ppa:linrunner/tlp
   sudo apt update

.. note::

   TLP and ThinkPad-related packages below are available via the official Ubuntu
   repository. Nevertheless it is recommended to use the PPA to stay with the
   latest TLP version.

Package Installation
--------------------
Install the following packages

* **tlp** *(PPA or universe)* – Power saving
* **tlp-rdw** *(PPA or universe)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command: ::

    sudo apt install tlp tlp-rdw

ThinkPads only
^^^^^^^^^^^^^^
Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will guide
you which package to install:

* **acpi-call-dkms** *(universe)* – optional – External kernel module providing
  battery charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)
* **tp-smapi-dkms** *(PPA or universe)* – optional – External kernel module providing
  battery charge thresholds, recalibration and specific :command:`tlp-stat -b`
  output for older ThinkPads

Install them either with your favorite package manager or the command ::

    sudo apt -install acpi-call-dkms tp-smapi-dkms

omitting the ones not required by your hardware.

.. seealso::

    * Refer to :ref:`faq-which-kernel-module` for details
    * For systems with Secure Boot enabled, please refer to
      `DKMS Secure Boot <https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS>`_
