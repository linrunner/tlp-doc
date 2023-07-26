openSUSE
===============================
.. rubric:: Scope:

* Officially supported openSUSE Leap releases and Tumbleweed

.. note::

    Execute the commands below in a root shell or with with a preceding :command:`sudo`.

Package Installation
--------------------
Packages are available from the official repositories:

* **tlp** – Power saving
* **tlp-rdw** – optional - :doc:`/settings/rdw`

Install them either with your favorite package manager or the command: ::

    zypper install tlp tlp-rdw

*openSUSE Leap 15.4 and newer as well as Tumbleweed*

Uninstall the conflicting `power-profiles-daemon` package: ::

   zypper remove power-profiles-daemon

Service Units
-------------
To complete the installation you must enable TLP's service: ::

   systemctl enable tlp.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

   systemctl mask systemd-rfkill.service systemd-rfkill.socket

.. seealso::

    * FAQ: :ref:`Service units <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`

ThinkPads only: External Kernel Modules
---------------------------------------
.. important::

    OpenSUSE Tumbleweed (at the time of writing) provides Linux kernel 6.4.
    In combination with TLP 1.5 or newer it offers full battery care support
    (i.e. charge thresholds and recalibration) for ThinkPads from model
    year 2011 onwards.

    **Therefore OpenSUSE Tumbleweed requires no external kernel modules and you
    do not need to proceed any further here.**

    Linux kernel 5.15 distributed with OpenSUSE Leap 15.4 and 15.5 provides
    only charge threshold functionality but no recalibration. If this is
    sufficient for you, stop reading here. However, if you need the recalibration
    feature or your model and/or kernel is older, read on. You may find out your
    current kernel version with the command uname -a or when TLP is already
    installed with tlp-stat -s.

For openSUSE Leap (with kernel < 5.17) and/or ThinkPads of model year 2011 and older
your mileage may vary as you have to browse `openSUSE Software <https://software.opensuse.org/>`_
for community packages or build the required module from source. The output of
:command:`tlp-stat -b` will guide you which external kernel module is required:

* **acpi_call** – optional – External kernel module providing battery recalibration
  for ThinkPads as of model year 2011 - e.g. T420/X220 and newer
* **tp_smapi** – optional – External kernel module providing battery charge
  thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011

.. seealso::

    * Refer to :ref:`faq-which-kernel-module` for details
