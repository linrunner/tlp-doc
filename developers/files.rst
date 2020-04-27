Files
=====
TLP consists – aside from manpages and docs – solely of POSIX compatible shell
scripts (i.e. without Bash extensions) and Perl scripts.

To be able to install the essential part of TLP without a dependency on
NetworkManager, it is recommended to provide two packages:

* **tlp** – Essential functionality
* **tlp-rdw** – :doc:`/settings/rdw` only (depends on NetworkManager)

The following table explains the provided files and assigns them to the
packages:

.. list-table::
   :widths: auto
   :align: center

   * - **File**
     - **Package**
     - **Target directory or file**
     - **Purpose**
   * - tlp.conf
     - tlp
     - /etc/
     - [from 1.3] User configuration file for both `tlp` and `tlp-rdw`
   * - default
     - tlp
     - /etc/default/tlp
     - [before 1.3] User configuration file for both `tlp` and `tlp-rdw`
   * - defaults.conf
     - tlp
     - /usr/share/tlp/
     - [from 1.3] Intrinsic configuration defaults for both `tlp` and `tlp-rdw`
   * - 00-template.conf
     - tlp
     - /etc/tlp.d/
     - [from 1.3] Template for drop-in configuration
   * - de.linrunner.tlp.metainfo.xml
     - tlp
     - /usr/share/metainfo
     - AppStream metadata
   * - func.d/*
     - tlp
     - /usr/share/tlp/func.d/
     - [from 1.2] TLP function libraries
   * - man/*
     - tlp
     - | /usr/share/man/
       |  man1/,
       |  man8/
     - Manpages
   * - README.d
     - tlp
     - /etc/tlp.d/README
     - [from 1.3] Explains the drop-in configuration directory
   * - tlp
     - tlp
     - /usr/sbin/
     - TLP main program
   * - tlp.bash_completion
     - tlp
     - | /usr/share/bash-completion/completions/
       |  tlp,
       |  tlp-stat,
       |  bluetooth,
       |  wifi,
       |  wwan
     -  Bash completion rules
   * - tlp-func-base
     - tlp
     - /usr/share/tlp/
     - [from 1.2] TLP base function library
   * - tlp.init
     - tlp
     - /etc/init.d/tlp
     - SysV init script to be invoked upon system start/shutdown:
       calls :command:`tlp init start/stop` to apply power saving settings,
       switch radio devices and set the battery thresholds
   * - tlp-pcilist
     - tlp
     - | [from 1.3] /usr/share/tlp/
       | [before 1.3] /usr/bin/
     - Report settings for PCIe devices; called by :command:`tlp-stat`
   * - tlp-readconfs
     - tlp
     - /usr/share/tlp/
     - Read and consolidate all of TLP's configuration files;
       called by :command:`tlp-func-base`
   * - tlp-rf
     - tlp
     - | /usr/bin/
       |   bluetooth,
       |   wifi,
       |   wwan
     - Script to turn radio devices on and off (soft links to the same file)
   * - tlp.rules
     - tlp
     - /lib/udev/rules.d/85-tlp.rules
     - Call :command:`tlp-usb-udev` for every plugged USB device
   * - tlp-run-on
     - tlp
     - | /usr/bin/
       |  run-on-bat,
       |  run-on-ac
     - Start commands conditionally depending on the power source
       (soft links to the same file)
   * - tlp.service
     - tlp
     - /lib/systemd/system/
     - Service to be invoked upon system start/shutdown by systemd:
       calls :command:`tlp init start/stop` to apply power saving settings,
       switch radio devices and set the battery thresholds.
   * - tlp-sleep
     - tlp
     - /lib/systemd/system-sleep/tlp
     - [from 1.3] Script to be invoked by systemd upon suspend and resume:
       calls :command:`tlp resume/suspend` to apply settings
   * - tlp-sleep.service
     - tlp
     - /lib/systemd/system/
     - [before 1.3] Service to be invoked by systemd upon suspend and resume:
       calls :command:`tlp resume/suspend` to apply settings
   * - tlp-sleep.elogind
     - tlp
     - /lib/elogind/system-sleep/49-tlp-sleep
     - [from 1.2] Script to be invoked by elogind upon suspend and resume:
       calls :command:`tlp resume/suspend` to apply settings
   * - tlp-stat
     - tlp
     - /usr/bin/
     - Status report with all effective settings
   * - tlp-usb-udev
     - tlp
     - /lib/udev/
     - Enable autosuspend for plugged USB devices
   * - tlp-usblist
     - tlp
     - | [from 1.3] /usr/share/tlp/
       | [before 1.3] /usr/bin/
     - Report USB settings; called by :command:`tlp-stat`
   * - tlp.upstart
     - tlp
     - n/a
     - Upstart script (currently not used)
   * - tpacpi-bat
     - tlp
     - /usr/sbin/
     - ACPI calls for advanced battery functions of Sandy Bridge and newer
       ThinkPad models (X220, T420, et al.). Script written by Elliot Wolk.
   * - man-rdw/*
     - tlp-rdw
     - /usr/share/man/man8/
     - [from 1.2] Manpages
   * - tlp-rdw
     - tlp-rdw
     - /usr/bin
     - [from 1.2] RDW command line tool
   * - tlp-rdw.bash_completion
     - tlp-rdw
     - /usr/share/bash-completion/completions/tlp-rdw
     - Bash completion rules
   * - tlp-rdw.rules
     - tlp-rdw
     - /lib/udev/rules.d/85-tlp-rdw.rules
     - Call :command:`tlp-rdw-udev` for dock/undock events
   * - tlp-rdw-udev
     - tlp-rdw
     - /lib/udev/
     - Handle dock/undock events
   * - tlp-rdw-nm
     - tlp-rdw
     - /etc/NetworkManager/dispatcher.d/
     - Network manager hook for ifup/ifdown events
   * - VERSION
     - n/a
     - n/a
     - Contains TLP's version number, used by the :doc:`makefile`
   * - Makefile
     - n/a
     - n/a
     - Installation of scripts and config file to their respective target dirs;
       see :doc:`makefile`
   * - changelog
     - tlp
     - distribution dependent
     - Changelog for TLP – the target directory is distribution specific and
       therefore it is not installed by the :doc:`makefile`
   * - README.md
     - tlp
     - distribution dependent
     - README file for TLP – the target directory is distribution specific and
       therefore it is not installed by the :doc:`makefile`
   * - AUTHORS
     - n/a
     - distribution dependent
     - List of developers / contributors
   * - COPYING
     - n/a
     - distribution dependent
     - | Copyright information:
       | - The target directory is distribution specific and therefore it is not installed by the Makefile
       | - Installation of this file (or inclusion in a distribution specific template) is mandatory
   * - LICENSE
     - n/a
     - distribution dependent
     - GPL v2 license text
