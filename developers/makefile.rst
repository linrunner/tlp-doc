Makefile
========

Targets
-------
.. list-table::
   :widths: auto
   :align: center

   * - target
     - Purpose
   * - install-tlp
     - Install files for package `tlp`
   * - install-rdw
     - Install files for package `tlp-rdw`
   * - install
     - Install files for `tlp` and `tlp-rdw`
   * - install-man
     - Install manpages for `tlp` and `tlp-rdw`
   * - install-man-tlp
     - Install manpages for package `tlp`
   * - install-man-rdw
     - Install manpages for package `tlp-rdw`
   * - uninstall-tlp
     - Uninstall files for package `tlp`
   * - uninstall-rdw
     - Uninstall files for package `tlp-rdw`
   * - uninstall
     - Uninstall files for `tlp` and `tlp-rdw`
   * - uninstall-man
     - Uninstall manpages for `tlp` and `tlp-rdw`
   * - uninstall-man-tlp
     - Uninstall manpages for package `tlp`
   * - uninstall-man-rdw
     - Uninstall manpages for package `tlp-rdw`
   * - checkall
     - Execute all check targets below
   * - checkbashisms
     - Check all shell scripts for bashisms
   * - checkbatdrv
     - Verify that bat.d plugins implement the must-have batdrv_() functions
   * - checkdupconst
     - Check shell scripts for duplicate constant definitions
   * - checkwip
     - Check shell scripts for comments indicating work in progress
   * - perlcritic
     - Check perl scripts with perlcritic
   * - shellcheck
     - Check shell scripts with ShellCheck

Parameters
----------
.. list-table::
   :widths: auto
   :align: center

   * - Parameter
     - Default value
     - Values / Purpose
   * - DESTDIR
     - (empty)
     - Prefix for all install directories
   * - TLP_CONF
     - /etc/default/tlp
     - | [from 1.3] Deprecated
       | [before 1.3] User configuration
   * - TLP_CONFDEF
     - /usr/share/tlp/defaults.conf
     - [from 1.3] Intrinsic default configuration
   * - TLP_CONFDIR
     - /etc/tlp.d
     - [from 1.3] Directory for drop-in customizations
   * - TLP_CONFREN
     - /usr/share/tlp/rename.conf
     - [from 1.4] Translation table for parameters
   * - TLP_CONFUSR
     - /etc/tlp.conf
     - [from 1.3] User configuration
   * - TLP_BIN
     - /usr/bin
     - Install directory
   * - TLP_BATD
     - /usr/share/tlp/bat.d
     - [from 1.4] Install directory for battery plugins
   * - TLP_ELOD
     - /lib/elogind/system-sleep
     - [from 1.2] Install directory elogind suspend/resume script
   * - TLP_FLIB
     - /usr/share/tlp/func.d
     - [from 1.2] Install directory for function libs
   * - TLP_MAN
     - /usr/share/man
     - Install directory for manpages
   * - TLP_META
     - /usr/share/metainfo
     - Install directory for AppStream metadata
   * - TLP_NMDSP
     - /etc/NetworkManager/dispatcher.d
     - Install directory for NM scripts
   * - TLP_NO_BASHCOMP
     - 0
     - 1=do not install tlp/tlp-rdw.bash_completion
   * - TLP_NO_INIT
     - 0
     - 1=do not install tlp.init
   * - TLP_NO_TPACPI
     - 0
     - 1=do not install tpacpi-bat
   * - TLP_RUN
     - /run/tlp
     - Directory for runtime data (volatile)
   * - TLP_SBIN
     - /usr/sbin
     - Install directory
   * - TLP_SHCPL
     - /usr/share/bash-completion/completions
     - Install directory for bash completion rules
   * - TLP_SYSD
     - /lib/systemd/system
     - Install directory for systemd units
   * - TLP_SDSL
     - /lib/systemd/system-sleep
     - [from 1.3] Install directory for systemd suspend/resume hooks
   * - TLP_SYSV
     - /etc/init.d
     - Install directory for sysvinit script
   * - TLP_TLIB
     - /usr/share/tlp
     - Install directory for function libs
   * - TLP_ULIB
     - /lib/udev
     - Install directory for udev scripts
   * - TLP_VAR
     - /var/lib/tlp
     - [from 1.2.2] persistent storage directory
   * - TLP_WITH_ELOGIND
     - | [from 1.2.2] 1
       | [1.2.1] 0
     - 1=install elogind script
   * - TLP_WITH_SYSTEMD
     - | [from 1.2.2] 1
       | [until 1.2.1] 0
     - 1=install systemd unit files
