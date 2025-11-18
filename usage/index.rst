Usage
*****

Start
=====
After installation TLP will start automatically on boot. To avoid having to
restart the system the first time, you can start it manually by using the
command: ::

   sudo tlp start

.. note:: Also use this command to apply changes after editing the configuration.

Status
======
Use the following command to check that TLP is enabled and active: ::

   tlp-stat -s

Check the output for

.. code-block:: none

   +++ TLP Status
   tlp            = enabled, last run: <Time of system start or last change of power profile>
   tlp-rdw        = enabled
   tlp-pd         = enabled, running
   Power profile  = balanced/BAT
   Power source   = battery


Version
=======
The following commands show TLP's version:

*Version 1.7*
::

   tlp --version


*Version 1.6.1 and older*
::

   tlp-stat -s

See the first output line: ::

   --- TLP 1.6.1 --------------------------------------------

Commands
========

The following sections describe TLP's set of shell commands:

.. toctree::
   :maxdepth: 1

   tlp
   tlpctl
   tlp-rdw
   tlp-stat
   bluetooth, wifi, wwan <radio>
   run-on-ac, run-on-bat <run-on>

.. note::

    * All commands shown with a preceding :command:`sudo` may as well be executed
      without :command:`sudo` in a root shell
    * For even more details refer to the command's manpage: :command:`man <command>`
