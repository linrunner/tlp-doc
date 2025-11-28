Arch Linux
==========

Package Installation
--------------------

Packages are available in the offical repositories:

* **tlp** *(Community)* – Power saving
* **tlp-pd** *(Community)* – optional, select :ref:`profile <intro-profiles>` with a mouse click *(Version 1.9 and newer)*
* **tlp-rdw** *(Community)* – optional, :doc:`/settings/rdw`

Install them either with your favorite package manager or the command:

*Version 1.9 and newer* ::

    sudo pacman -S tlp tlp-pd tlp-rdw

*Version 1.8 and older* ::

    sudo pacman -S tlp tlp-rdw


Service Units
-------------
To complete the installation you must enable TLP's service(s): ::

    sudo systemctl enable tlp.service

*For version 1.9 and newer* with tlp-pd, additionally: ::

    sudo systemctl enable --now tlp-pd.service

Using the :doc:`/settings/rdw` (tlp-rdw) requires one more service: ::

    sudo systemctl enable NetworkManager-dispatcher.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

    sudo systemctl mask systemd-rfkill.service systemd-rfkill.socket

.. seealso::

    * FAQ: :ref:`Service units <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`
    * Refer to the `Arch Wiki <https://wiki.archlinux.org/index.php/TLP>`_ as well

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    Arch Linux (at the time of writing) provides kernel 6.10 (linux) or 6.6 (linux-lts).
    In combination with TLP 1.5 or newer this enables full battery care support
    (i.e. charge thresholds and recalibration) for ThinkPads from model year 2011 onwards.

    **An external kernel module (also referred to as "out-of-tree" module)
    is not required in this case, and the following steps are not necessary.
    However, if your model is from 2011 or older, read on.**


Only if the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
shows the line

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration

then install the approriate package

* **tp_smapi** *(Community)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011
* **tp_smapi-lts** *(Community)* – optional – Use instead of `tp_smapi` when the
  LTS kernel is installed

either with your favorite package manager or the command ::

    pacman -S tp_smapi

.. note::

    * You must disable Secure Boot to use the ThinkPad specific packages
