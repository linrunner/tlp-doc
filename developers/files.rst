Files
=====
TLP consists – aside from manpages and docs – solely of POSIX compatible shell
scripts (i.e. without Bash extensions) and Perl scripts.

To be able to install the essential part of TLP without a dependency on
NetworkManager, it is recommended to provide two packages:

* **tlp** – Essential functionality
* **tlp-rdw** – :doc:`/settings/rdw` only (depends on NetworkManager)
* **tlp-pd** – TLP Profiles Daemon, desktop power profile switch

The following table explains the provided files and assigns them to the
packages:

.. list-table::
   :widths: auto
   :align: left

   * - **File**
     - **Package**
     - **Target directory or file**
     - **Purpose**
   * - 00-template.conf
     - tlp
     - /etc/tlp.d/
     - Template for drop-in configuration
   * - bat.d/*
     - tlp
     - /usr/share/tlp/bat.d/
     - TLP battery plugins
   * - completions/bash/tlp.bash_completion
     - tlp
     - | /usr/share/bash-completion/completions/
       |  tlp,
       |  tlp-stat,
       |  bluetooth,
       |  nfc,
       |  wifi,
       |  wwan
     - Bash completion rules
   * - | completions/fish/tlp.fish
       |   tlp-stat.fish
     - tlp
     - /usr/share/fish/vendor_completions.d/
     - Fish completion rules
   * - | completions/zsh/_tlp
       |  _tlp-radio-device
       |  _tlp-run-on
       |  _tlp-stat
     - tlp
     - /usr/share/zsh/site-functions/
     - ZSH completion rules
   * - de.linrunner.tlp.metainfo.xml
     - tlp
     - /usr/share/metainfo
     - AppStream metadata
   * - defaults.conf
     - tlp
     - /usr/share/tlp/
     - Intrinsic configuration defaults for both `tlp` and `tlp-rdw`
   * - deprecated.conf
     - tlp
     - /usr/share/tlp/
     - Deprecated configuration parameters and related messages
       for :command:`tlp-stat -c`
   * - func.d/*
     - tlp
     - /usr/share/tlp/func.d/
     - TLP function libraries
   * - man/*
     - tlp
     - | /usr/share/man/
       |  man1/,
       |  man8/
     - Manpages
   * - README.d
     - tlp
     - /etc/tlp.d/README
     - Explains the drop-in configuration directory
   * - rename.conf
     - tlp
     - /usr/share/tlp/
     - translation table for parameters
   * - tlp.conf
     - tlp
     - /etc/
     - User configuration file for both `tlp` and `tlp-rdw`
   * - tlp
     - tlp
     - /usr/sbin/
     - TLP main program
   * - tlp-func-base
     - tlp
     - /usr/share/tlp/
     - TLP base function library
   * - tlp.init
     - tlp
     - /etc/init.d/tlp
     - SysV init script to be invoked upon system start/shutdown:
       calls :command:`tlp init start/stop` to apply power saving settings,
       switch radio devices and set the battery thresholds
   * - tlp-pcilist
     - tlp
     - /usr/share/tlp/
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
       |   nfc,
       |   wifi,
       |   wwan
     - Script to turn radio devices on and off (symlinks to the same file)
   * - tlp.rules
     - tlp
     - /usr/lib/udev/rules.d/85-tlp.rules
     - Call :command:`tlp-usb-udev` for every plugged USB device
   * - tlp-run-on
     - tlp
     - | /usr/bin/
       |  run-on-bat,
       |  run-on-ac
     - Start commands conditionally depending on the power source
       (symlinks to the same file)
   * - tlp.service
     - tlp
     - /usr/lib/systemd/system
     - Service to be invoked upon system start/shutdown by systemd:
       calls :command:`tlp init start/stop` to apply power saving settings,
       switch radio devices and set the battery thresholds.
   * - tlp-sleep
     - tlp
     - /usr/lib/systemd/system-sleep/tlp
     - Script to be invoked by systemd upon suspend and resume:
       calls :command:`tlp resume/suspend` to apply settings
   * - tlp-sleep.elogind
     - tlp
     - /usr/lib/elogind/system-sleep/49-tlp-sleep
     - Script to be invoked by elogind upon suspend and resume:
       calls :command:`tlp resume/suspend` to apply settings
   * - tlp-stat
     - tlp
     - /usr/bin/
     - Status report with all effective settings
   * - tlp-usb-udev
     - tlp
     - /usr/lib/udev/
     - Enable autosuspend for plugged USB devices
   * - tlp-usblist
     - tlp
     - /usr/share/tlp/
     - Report USB settings; called by :command:`tlp-stat`
   * - tlp.upstart
     - tlp
     - n/a
     - Upstart script (currently not used)
   * - man-rdw/*
     - tlp-rdw
     - /usr/share/man/man8/
     - Manpages
   * - tlp-rdw
     - tlp-rdw
     - /usr/bin
     - RDW command line tool
   * - completions/bash/tlp-rdw.bash_completion
     - tlp-rdw
     - /usr/share/bash-completion/completions/tlp-rdw
     - Bash completion rules
   * - completions/fish/tlp-rdw.fish
     - tlp-rdw
     - /usr/share/fish/vendor_completions.d/tlp-rdw.fish
     - Fish completion rules
   * - completions/zsh/_tlp-rdw
     - tlp-rdw
     - /usr/share/zsh/site-functions/_tlp-rdw
     - ZSH completion rules
   * - tlp-rdw.rules
     - tlp-rdw
     - /usr/lib/udev/rules.d/85-tlp-rdw.rules
     - Call :command:`tlp-rdw-udev` for dock/undock events
   * - tlp-rdw-udev
     - tlp-rdw
     - /usr/lib/udev/
     - Handle dock/undock events
   * - tlp-rdw-nm
     - tlp-rdw
     - /usr/lib/NetworkManager/dispatcher.d
     - NetworkManager hook for ifup/ifdown events
   * - man-pd/*
     - tlp-pd
     - | /usr/share/man/
       |  man1/,
       |  man8/
     - Manpages
   * - tlpctl
     - tlp-pd
     - /usr/bin
     - tlp-pd command line tool
   * - tlp-pd
     - tlp-pd
     - /usr/sbin
     - TLP profiles daemon
   * - tlp-pd.service
     - tlp-pd
     - /usr/lib/systemd/system
     - Service to run tlp-pd on system startup
   * - tlp-pd.dbus.conf**
     - tlp-pd
     - /usr/share/dbus-1/system.d
     - D-Bus configuration;
       :doc:`makefile` performs pattern substitution and installs two copies to the target directory
   * - tlp-pd.dbus.service
     - tlp-pd
     - /usr/share/dbus-1/system-services
     - D-Bus service declaration;
       :doc:`makefile` performs pattern substitution and installs two copies to teh target directory
   * - tlp-pd.policy
     - tlp-pd
     - /usr/share/polkit-1/actions
     - PolKit policy configuration
   * - completions/bash/tlpctl.bash_completion
     - tlp-pd
     - /usr/share/bash-completion/completions/tlpctl
     - Bash completion rules
   * - completions/fish/tlp-rdw.fish
     - tlp-pd
     - /usr/share/fish/vendor_completions.d/tlpctl.fish
     - Fish completion rules
   * - completions/zsh/_tlpctl
     - tlp-pd
     - /usr/share/zsh/site-functions/_tlpctl
     - ZSH completion rules
   * - unit-tests/*
     - n/a
     - n/a
     - functional tests of TLP (incomplete coverage);
       needs specific hardware, not suited for package autotest;
       see unit-tests.rst for requirements
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
