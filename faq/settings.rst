Settings
========

Does TLP conflict with my desktop's power settings?
---------------------------------------------------
It depends on the desktop environment and the particular setting. TLP:

* does not touch display or keyboard backlight brightness → no conflict,
* does not change suspend / hibernate settings → no conflict,
* applies Wi-Fi power saving by default → conflict with KDE energy saving
  settings possible,
* switches radio devices on and off when configured by the user → conflict with
  GNOME power settings or KDE energy saving settings possible.

"Conflict possible" means that the setting configured in TLP may get overwritten
by the desktop's setting (and vice versa). System stability issues are not to be
expected from those conflicts.

How can I change TLP's settings?
--------------------------------

See :doc:`/settings/index`.

.. _faq-set-upgrade:

Does upgrading TLP overwrite my settings?
-----------------------------------------
No. Package managers take care not to replace a user edited configuration file
without explicit user confirmation and create a backup copy too.

.. _faq-set-mig-from-13:

How do I transfer my settings when upgrading from version 1.2.2 (or lower) to 1.3?
----------------------------------------------------------------------------------
Read about the :ref:`config files of version 1.3 <set-config-files-13>` first to
learn about the new configuration scheme in version 1.3.

There are three migration paths:

.. rubric:: 1. 1:1 Takeover

The format of the configuration file has not changed, only the location.
Save the pristine **/etc/tlp.conf** and copy **/etc/default/tlp** to
**/etc/tlp.conf**.

Debian and Ubuntu only: if **/etc/default/tlp** was ever edited by you,
upgrading to version 1.3 moves it to **/etc/tlp.conf** automatically. The pristine
**tlp.conf** ends up in **/etc/tlp.conf.dpkg-new**.

.. rubric:: 2. Start from Scratch with /etc/tlp.conf

Just enter your specific settings into **/etc/tlp.conf**.
Other than with version 1.2.2 (and lower) all parameters in **/etc/tlp.conf** are
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
