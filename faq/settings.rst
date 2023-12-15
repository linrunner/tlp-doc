Settings
========

Does TLP conflict with my desktop environment's power settings?
---------------------------------------------------------------
It depends, TLP:

* Does not touch display or keyboard backlight brightness → no conflict.
* Does not change suspend / hibernate settings → no conflict.
* Applies WiFi power saving by default → conflict with KDE energy saving
  is settings possible.
* Applies charge thresholds → conflict with KDE charge limit setting is
  possible.
* Switches radio devices on and off when configured by the user →
  conflict with GNOME power settings or KDE energy saving settings possible.
* TLP's power savings will not be applied during boot when using GNOME or KDE
  desktop environments that have power-profiles-daemon installed →
  severe conflict - see :doc:`/faq/ppd`.
* Pop!_OS's system76-power works on the same set of
  :ref:`kernel settings <intro-how-it-works>`
  → severe conflict - do not use together with TLP.
* Slimbook Battery uses TLP as backend and overwrites TLP's configuration
  files → conflict - if you wish to configure TLP individually, you must
  first uninstall Slimbook Battery.

.. note::

    "Conflict" refers to the situation where settings configured in
    TLP are overwritten by the desktop environment's settings (and vice
    versa), resulting in unpredictable power savings.

How can I change TLP's settings?
--------------------------------

See :doc:`/settings/index`.

.. _faq-set-upgrade:

Does upgrading TLP overwrite my settings?
-----------------------------------------
No. Package managers take care not to replace a user edited configuration file
without explicit user confirmation and create a backup copy too.

.. _faq-set-mig-from-13:

How do I transfer my settings when upgrading from version 1.2.2 (and older) to 1.3 (and newer)?
-----------------------------------------------------------------------------------------------
Read about the :ref:`config files <set-config-files>` first to
learn about the current configuration scheme (introduced in version 1.3).

There are three migration paths:

.. rubric:: 1. 1:1 Takeover

The format of the configuration file has not changed, only the location.
Save the pristine **/etc/tlp.conf** and copy **/etc/default/tlp** to
**/etc/tlp.conf**.

Debian and Ubuntu only: if **/etc/default/tlp** was ever edited by you,
upgrading to version 1.3 or newer moves it to **/etc/tlp.conf** automatically.
The pristine **tlp.conf** ends up in **/etc/tlp.conf.dpkg-new**.

.. rubric:: 2. Start from Scratch with /etc/tlp.conf

Just enter your specific settings into **/etc/tlp.conf**.
Other than with version 1.2.2 and older all parameters in **/etc/tlp.conf** are
disabled, you have to remove the comment character `#` in the first column for
parameters you want to change.

.. note::

    Debian and Ubuntu only: move **/etc/tlp.conf.dpkg-new** to **/etc/tlp.conf**
    if necessary (see above).

.. rubric:: 3. Start over with a new File below /etc/tlp.d/

Create a new file **01-mytlp.conf** copying **00-template.conf**, then enter your
specific settings only.
The exact file name does not matter as long as it ends in **.conf**. You may as
well split your settings into several files.

.. note::

    Debian and Ubuntu only: remember to move the pristine **/etc/tlp.conf.dpkg-new**
    to **/etc/tlp.conf** if necessary (see above).
