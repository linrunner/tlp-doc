Settings
========

How can I change TLP's settings?
--------------------------------

See :doc:`/settings/index`.


.. _faq-set-uninstall-persist:

I uninstalled TLP — could it be that its configuration still works?
-------------------------------------------------------------------
No. After uninstalling and rebooting, it's as if it never happened.
TLP only applies tuning while it is running. It does not make permanent
changes to your system or hardware.

However, there may be one exception, refer to :ref:`faq-how-to-disable-thresholds`


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
