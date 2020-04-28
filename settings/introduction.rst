Introduction
============
This section explains how TLP's settings are organized, the syntax of the
parameters used and how to change them.

Profiles
--------
TLP uses two settings profiles that are applied depending on the power source:

* Parameters ending on `_AC` are effective when AC is connected
* Parameters ending on `_BAT` are effective when running on battery

Parameters ending neither on `_AC` nor `_BAT` apply to both profiles.

Defaults
--------
Two classes of parameters exist:

* Parameters with an intrinsic default – preceded by a `Default:` line in the config file
* Parameters without default

Syntax notes
------------
* Parameter values containing blanks must be enclosed in double quotes (`""`)
* Parameters without default may be disabled completely by a leading `#`
* Parameters with intrinsic default are documented as `Default when unconfigured:`
  - use `PARAM=""` to disable them completely
* Parameter values given here and in the config files may be suggestions rather
  than defaults

.. _set-config-files:

Config Files
------------
.. note::

    Refer to the output of :command:`tlp-stat -s` to determine the TLP version
    installed.

.. _set-config-files-13:

TLP 1.3 and higher
^^^^^^^^^^^^^^^^^^
TLP 1.3 introduces a new configuration scheme. Settings are read from the following
files in the specified order:

* Intrinsic defaults
* **/etc/tlp.d/*.conf**: Drop-in customization snippets, read in lexical (alphabetical) order
* **/etc/tlp.conf**: User configuration

Hints:

* In case of identical parameters in several but also within the same file, the
  last occurence has precedence
* This also means, parameters in **/etc/tlp.conf** will override anything else
  because it is read last
* All parameters in **/etc/tlp.conf** are disabled, remove the leading `#` to
  activate a change
* Config files in the **/etc/tlp.d/** directory are created by the user:

   * Files consist of `PARAMETER="value"` entries, comments characterized by `#`
     in the 1st column and empty lines
   * Filenames must end in **.conf**, otherwise the file will be ignored
   * **00-template.conf** is provided as an example

* If you are unsure, put your configuration changes in **/etc/tlp.conf**
* :ref:`Transfer settings when upgrading <faq-set-mig-from-13>` describes how to
  migrate your configuration from versions before 1.3

TLP 1.2.2 and lower
^^^^^^^^^^^^^^^^^^^
All settings are stored in the single config file **/etc/default/tlp**.

Making Changes
^^^^^^^^^^^^^^
A config file can be changed with any text editor (root privilege is needed).
For example: ::

   sudo nano /etc/tlp.conf

All changes must be activated by removing the leading `#` and, after saving the
file, will take effect only

* after a reboot,
* plugging or unplugging AC
* or by the command ::

   sudo tlp start

.. note::

    When installing upgrades of TLP, the package manager asks for confirmation
    before overwriting a changed config file with an updated version. Please
    refer to :ref:`faq-set-upgrade`
