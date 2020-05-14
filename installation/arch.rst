Arch Linux
==========
.. note::

    Execute the commands below in a root shell.

Package Installation
--------------------

Packages are available in the offical repositories:

* **tlp** *(Community)* – Power saving
* **tlp-rdw** *(Community)* – optional, :doc:`/settings/rdw`

Install them either with your favorite package manager or the command: ::

   pacman -S tlp tlp-rdw

ThinkPads only
^^^^^^^^^^^^^^
Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will guide
you which package to install:

* **acpi_call** *(Community)* – optional – External kernel module providing battery
  charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)
* **tp_smapi** *(Community)* – optional – External kernel module providing battery
  charge thresholds, recalibration and specific :command:`tlp-stat -b` output
  for older ThinkPads
* **tp_smapi-lts** *(Community)* – optional – Use instead of `tp_smapi` when the
  LTS kernel is installed

Install them either with your favorite package manager or the command ::

    pacman -S acpi_call tp_smapi

omitting the ones not required by your hardware.

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

Service Units
-------------
To complete the installation you must enable TLP's service: ::

   systemctl enable tlp.service

Version 1.2.2 and lower require an additional service: ::

   systemctl enable tlp-sleep.service

Using the :doc:`/settings/rdw` (tlp-rdw) requires yet another service: ::

   systemctl enable NetworkManager-dispatcher.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

   systemctl mask systemd-rfkill.service
   systemctl mask systemd-rfkill.socket

.. seealso::

    * Refer to :ref:`service units <faq-service-units>`
    * Refer to the `Arch Wiki <https://wiki.archlinux.org/index.php/TLP>`_ as well

