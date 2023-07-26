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

ThinkPads only: External Kernel Modules
---------------------------------------
.. important::

    Arch Linux (at the time of writing) offers kernel 6.4 (linux) or 6.1 (linux-lts).
    In combination with TLP 1.5 or newer this enables full battery care support
    (i.e. charge thresholds and recalibration) for ThinkPads from model year 2011 onwards.

    **Therefore no external kernel modules are required and you do not need to proceed
    any further here.**

    However, continue reading if your ThinkPad model is from 2011 or older.

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` will guide you which package to install:

* **tp_smapi** *(Community)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011
* **tp_smapi-lts** *(Community)* – optional – Use instead of `tp_smapi` when the
  LTS kernel is installed

Install the appropriate package  either with your favorite package manager
or the command ::

    pacman -S tp_smapi

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

