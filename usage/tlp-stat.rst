tlp-stat
--------
.. topic:: Purpose

    View TLP's configuration, system information, kernel power saving tunables
    and battery data.

.. rubric:: Invocation without options shows all information categories

::

    sudo tlp-stat


.. rubric:: View battery data

::

    sudo tlp-stat -b
    sudo tlp-stat --battery

Add `-v` to see battery voltages (if available).


.. rubric:: View active configuration

::

    tlp-stat -c
    tlp-stat --config


.. rubric:: View the difference between defaults and user configuration

::

    tlp-stat --cdiff


.. rubric:: View disk device information

::

    sudo tlp-stat -d
    sudo tlp-stat --disk


.. rubric:: View PCIe device information

::

    sudo tlp-stat -e
    sudo tlp-stat --pcie

Add `-v` to see device runtime status.


.. rubric:: View graphics card information

::

    sudo tlp-stat -g
    sudo tlp-stat --graphics

.. rubric:: View current power mode

*Version 1.7*

::

    tlp-stat -m
    tlp-stat --mode

.. rubric:: View processor information

::

    sudo tlp-stat -p
    sudo tlp-stat --processor

For clarity the standard output shows only `cpu0`, add  `-v` to see all.


.. rubric:: View less

*Version 1.7*

Omit version header and show less information in the processor category.

::

    tlp-stat -q
    tlp-stat --quiet


.. rubric:: View radio device states

::

    tlp-stat -r
    tlp-stat --rfkill


.. rubric:: View system information and TLP status

::

    tlp-stat -s
    tlp-stat --system


.. rubric:: View temperatures and fan speed

::

    tlp-stat -t
    tlp-stat --temp


.. rubric:: View USB device information

::

    tlp-stat -u
    tlp-stat --usb

Add `-v` to see device runtime status.


.. rubric:: View more

Show more information in battery, PCIe, processor and USB categories.

::

    tlp-stat -v
    tlp-stat --verbose

.. rubric:: View version

*Version 1.7*

::

    tlp-stat --version

.. rubric:: Diagnostics and debugging

Monitor power supply udev events: ::

    sudo tlp-stat -P
    sudo tlp-stat --pev

View power supply diagnostics: ::

    tlp-stat --psup

View trace output: ::

    sudo tlp-stat -T
    sudo tlp-stat --trace

Check if udev rules for power source changes and connecting USB devices are active:

::

    tlp-stat --udev

View warnings about SATA disks: ::

    tlp-stat -w
    tlp-stat --warn

Please refer to :doc:`/faq/warnings` for details.
