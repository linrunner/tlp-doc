Architecture
============
.. note::

    There is no TLP daemon. TLP is event based and depends on other system daemons.

The call architecture is as follows:

.. rubric:: Change of Power Source

When the power supply is plugged or unplugged, `udevd` detects this by a rule in
**tlp.rules** and calls :command:`tlp auto`.

.. rubric:: Suspend (Hibernate) / Resume Cycle

Depends on the target distribution's init system:

* Systemd: sources contain **tlp-sleep** which calls :command:`tlp suspend` or
  :command:`tlp resume.`
* Elogind: sources contain **tlp-sleep.elogind** which calls
  :command:`tlp suspend` or :command:`tlp resume.`
* Sysvinit or other init system: you have to provide a script that calls
  .command:`tlp suspend` or :command:`tlp resume`.
