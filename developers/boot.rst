System Start
============

Systemd
-------
The sources contain the unit :command:`tlp.service`, which calls
:command:`tlp init start/stop` upon system startup/shutdown. Use the
`TLP_WITH_SYSTEMD` :doc:`makefile` switch to install it.

Sysvinit
--------
The package install process must link the sysvinit script **tlp.init** i.e.
**/etc/init.d/tlp** to the necessary runlevels by means of the target
distribution.

Other Init System
-----------------
In case the target distribution does support neither `systemd` nor `sysvinit`,
you have to program a script that calls :command:`tlp` accordingly: ::

    tlp init start|stop|restart|force-reload|status

The parameter after `init` is checked by :command:`tlp`.
