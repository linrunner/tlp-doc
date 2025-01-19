Introduction
============
This section explains how TLP's settings are `organized <#profiles>`_,
the `Config Files`_, the `Parameter Syntax`_ and `Making Changes`_.

Quick Instructions
------------------

If you just want to make a quick change to the configuration, read this
section and then read the rest later.

Open TLP’s main config file with a text editor: ::

    sudo nano /etc/tlp.conf

Change the desired line(s). Before: ::

    #CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    #CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

After – remember to remove the leading #:

.. code-block::
    :emphasize-lines: 2

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=power

.. note::

    In contrast to below, the two preceding code blocks are not shell
    commands but excerpts from the configuration file.

Save and quit the editor. Then activate your changes using the shell
command ::

    sudo tlp start


Profiles
--------
TLP uses two settings profiles that are automatically applied depending on the
power source:

* Parameters ending in `_AC` are effective when AC is connected
* Parameters ending in `_BAT` are effective when running on battery

Parameters ending neither in `_AC` nor in `_BAT` apply to both profiles.

.. important::

    Parameters without intrinsic default (see :ref:`below <set-param-defaults>`)
    must *always* be specified for *both _AC and _BAT*. Omitting one of the two
    makes the set value effective for both power sources, since a change only
    occurs when different values are defined.


.. _set-config-files:

Config Files
------------

Settings are read from the following files in the specified order:

* Intrinsic defaults
* **/etc/tlp.d/*.conf**: Drop-in customization snippets, read in lexical (alphabetical) order
* **/etc/tlp.conf**: User configuration

Hints:

* In case of identical parameters in several but also within the same file, the
  last occurence has precedence
* This also means, parameters in **/etc/tlp.conf** will override anything else
  because it is read last
* All parameters in **/etc/tlp.conf** are disabled, remove the leading `#` to
  activate your change
* Config files in the **/etc/tlp.d/** directory are created by the user:

   * Filenames must end in **.conf**, otherwise the file will be ignored
   * **00-template.conf** is provided as an example

* If in doubt, put your configuration changes in **/etc/tlp.d/01-mytlp.conf**
* :ref:`Transfer settings when upgrading <faq-set-mig-from-13>` describes how to
  migrate your configuration from versions before 1.3

.. note::

    TLP versions 1.2.2 and older stored all settings in a single
    config file named **/etc/default/tlp**.


.. _set-param-defaults:

Parameter Defaults
------------------
Two kinds of parameters exist:

* Parameters with intrinsic default:

    * Marked with `"Default when unconfigured:"` in this documentation
    * Preceded by `"Default:"` in **/etc/tlp.conf**

* Parameters without intrinsic default

.. note::

    Parameter values given in this documentation and in the config files may
    be suggestions rather than intrinsic defaults.


Parameter Syntax
----------------
:ref:`set-config-files` consist of parameter and comment lines.

.. rubric:: Parameter lines

::

    PARAMETER=value

Parameter values containing blanks must be enclosed in double quotes: ::

    USB_DENYLIST="1111:2222 3333:4444"

.. rubric:: Comment lines

The content of lines beginning with a `#` in the first column
is ignored completely: ::

    #What is written here does not matter.

Empty lines are ignored as well.

Until version 1.5 comments after parameters are not allowed, the whole line
will be silently ignored: ::

   EXAMPLE="do not use like this" # Parameter in front is ignored - until version 1.5!

As of version 1.6 the above line is valid and taken into account.

.. rubric:: Disabling features

Parameters without intrinsic default may be disabled by commenting them out with
a leading `#`: ::

    #STOP_CHARGE_THRESH_BAT1=80

Parameters with intrinsic default may be disabled by entering an empty string: ::

    RUNTIME_PM_DRIVER_DENYLIST=""

.. rubric:: Concatenation with +=

A nifty feature to add something to an intrinsic default (Example 1):

    Intrinsic default `DISK_DEVICES="nvme0n1 sda"`

    plus **/etc/tlp.d/01-mytlp.conf**: ::

        DISK_DEVICES+="sdb"

    Results in: `DISK_DEVICES="nvme0n1 sda sdb"`

Or add values in a subsequent config file (Example 2):

    **/etc/tlp.d/01-mytlp.conf**: ::

        USB_DENYLIST="1111:2222 3333:4444"

    plus **/etc/tlp.d/02-hw-specific.conf**: ::

        USB_DENYLIST+="5555:6666"

    Results in: `USB_DENYLIST="1111:2222 3333:4444 5555:6666"`


.. _set-making-changes:

Making Changes
--------------
A config file can be changed with any text editor (root privilege is needed).
For example: ::

   sudo nano /etc/tlp.conf
   sudo nano /etc/tlp.d/01-mytlp.conf

All changes must be activated by removing the leading `#` and, after saving the
file, will take effect only

* after a reboot,
* plugging or unplugging AC
* or by the shell command ::

   sudo tlp start

.. note::

    When installing upgrades of TLP, the package manager asks for confirmation
    before overwriting a changed config file with an updated version. Please
    refer to :ref:`faq-set-upgrade`


.. seealso::

    Use :doc:`/usage/tlp-stat` to:

    * Show active configuration files and enabled parameters: :command:`tlp-stat -c`
    * Show the difference between default and user configuration: :command:`tlp-stat --cdiff`
    * Get the TLP version installed: :command:`tlp-stat -s`
