Support
*******

There is no dedicated TLP support forum yet. For general questions on how to
install, configure or use TLP, please visit your favorite Linux forum.
For ThinkPad battery specific issues please ask for advice in a ThinkPad or
hardware oriented forum.

.. important::

    Before asking for support, please check :doc:`/faq/index` and
    :doc:`/support/troubleshooting` first.

Necessary Information
=====================
When asking for support, *always* provide the full output of ::

    sudo tlp-stat

completed by the information with which power source – AC and/or battery –
the problem occurs.

Bug Reports
===========
Bug reports may be filed via the
`issue tracker <https://github.com/linrunner/TLP/issues>`_.

.. important::

    Please read the
    `Bug Reporting Howto <https://github.com/linrunner/TLP/blob/main/.github/Bug_Reporting_Howto.md>`_
    first.

.. note::

    Submit corrections to this website via the
    `tlp-doc <https://github.com/linrunner/tlp-doc>`_ GitHub repo.

.. _support-trace-mode:

Trace Mode
==========
To examine suspected problems in TLP more closely, activate trace mode in
:ref:`set-config-files`: ::

    TLP_DEBUG="arg bat disk lock nm path pm ps rf run sysfs udev usb"

The accumulated trace data may be read at any time with ::

    sudo tlp-stat -T

.. rubric:: (r)syslog

In case the trace output is missing, you have to modify your syslog configuration.
For rsyslogd create the file **/etc/rsyslog.d/90-debug.conf*** containing ::

    *.=debug;\
    auth,authpriv.none;\
    news.none;mail.none -/var/log/debug

and restart the daemon ::

    sudo /etc/init.d/rsyslog restart

.. rubric:: systemd

No prerequisites, TLP uses journald for trace data.
