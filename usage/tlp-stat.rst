tlp-stat
--------
.. topic:: Purpose

    View TLP's configuration, system information, kernel power saving tunables
    and battery data.

Invocation without options shows all information categories: ::

    sudo tlp-stat

The following options select individual categories of devices and/or information.
They can also be combined.


System Operation
^^^^^^^^^^^^^^^^
View system information and TLP status: ::

    tlp-stat -s
    tlp-stat --system

Print currently active power profile *(Version 1.7 and newer)*: ::

    tlp-stat -m
    tlp-stat --mode


Configuration
^^^^^^^^^^^^^
View active configuration: ::

    tlp-stat -c
    tlp-stat --config

View the difference between TLP's defaults and user configuration: ::

    tlp-stat --cdiff

Power Saving Tunables
^^^^^^^^^^^^^^^^^^^^
View disk device tunables: ::

    sudo tlp-stat -d
    sudo tlp-stat --disk

View PCIe device tunables: ::

    sudo tlp-stat -e
    sudo tlp-stat --pcie

Add `-v` to see device runtime status.

View graphics card tunables: ::

    sudo tlp-stat -g
    sudo tlp-stat --graphics

*Version 1.9 and newer*: add `-v` to see power state and clocks of AMD GPUs.

View processor tunables: ::

    sudo tlp-stat -p
    sudo tlp-stat --processor

For clarity the standard output shows only `cpu0`, add  `-v` to see all CPUs.
Add `-q` to see CPU driver state only.

View radio device states and tunables: ::

    tlp-stat -r
    tlp-stat --rfkill

*Version 1.9 and newer*: add `-v` to see NetworkManager and rfkill details.

View USB device tunables: ::

    tlp-stat -u
    tlp-stat --usb

Add `-v` to see device runtime status.


Battery Care
^^^^^^^^^^^^
View battery data: ::

    sudo tlp-stat -b
    sudo tlp-stat --battery

Add `-v` to see battery voltages (if available).


Information
^^^^^^^^^^^
View temperatures and fan speed: ::

    tlp-stat -t
    tlp-stat --temp

*Version 1.9 and newer*: add `-v` to see all individual sensors.

Print TLP version *(Version 1.7 and newer)*: ::

    tlp-stat --version


Scope Options
^^^^^^^^^^^^^
Omit version header and show less information in the processor category *(Version 1.7 and newer)*: ::

    -q
    --quiet

Show more information in the battery, graphics, PCIe, processor, temperature, wireless and
USB categories: ::

    -v
    --verbose


Diagnostics and Debugging
^^^^^^^^^^^^^^^^^^^^^^^^^
View tlp-pd diagnostics *(Version 1.9 and newer)*:  ::

    tlp-stat --pd-diag

Monitor power supply udev events: ::

    sudo tlp-stat -P
    sudo tlp-stat --pev

View power supply diagnostics: ::

    tlp-stat --psup

View trace output: ::

    sudo tlp-stat -T
    sudo tlp-stat --trace

View trace output correlated with NetworkManager journal *(Version 1.9 and newer)*: ::

    sudo tlp-stat --trace-nm

Check if udev rules for power source changes and connecting USB devices are active: ::

    tlp-stat --udev

View warnings about SATA disks: ::

    tlp-stat -w
    tlp-stat --warn

Please refer to :doc:`/faq/warnings` for details.
