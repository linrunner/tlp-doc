Arch Linux
==========
.. note::

    Execute the commands below in a root shell or with with a preceding :command:`sudo`.

Package Installation
--------------------

Packages are available in the offical repositories:

* **tlp** *(Community)* – Power saving
* **tlp-rdw** *(Community)* – optional, :doc:`/settings/rdw`

Install them either with your favorite package manager or the command: ::

   pacman -S tlp tlp-rdw


Service Units
-------------
To complete the installation you must enable TLP's service: ::

   systemctl enable tlp.service

Using the :doc:`/settings/rdw` (tlp-rdw) requires one more service: ::

   systemctl enable NetworkManager-dispatcher.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

   systemctl mask systemd-rfkill.service systemd-rfkill.socket

.. seealso::

    * FAQ: :ref:`Service units <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`
    * Refer to the `Arch Wiki <https://wiki.archlinux.org/index.php/TLP>`_ as well

ThinkPads only
--------------
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` will guide you which package to install:

* **acpi_call** *(Community)* – optional – External kernel module providing battery
  recalibration for ThinkPads since model year 2011 - e.g. T420/X220 and newer
* **acpi_call-lts** *(Community)* – optional – Use instead of `acpi_call` when the
  LTS kernel is installed
* **tp_smapi** *(Community)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011
* **tp_smapi-lts** *(Community)* – optional – Use instead of `tp_smapi` when the
  LTS kernel is installed

Install the appropriate package  either with your favorite package manager
or the command ::

    pacman -S acpi_call

Replace `acpi_call` with `acpi_call-lts`, `tp_smapi` or `tp_smapi-lts` where suitable.

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

