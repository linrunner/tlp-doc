Makefile
========

Targets
-------
.. list-table::
   :widths: auto
   :align: left

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
   :align: left

   * - Parameter
     - Default value
     - Values / Purpose
   * - DESTDIR
     - (empty)
     - Prefix for all install directories
   * - TLP_BATD
     - /usr/share/tlp/bat.d
     - Install directory for battery plugins
   * - TLP_BIN
     - /usr/bin
     - Install directory
   * - TLP_CONFDEF
     - /usr/share/tlp/defaults.conf
     - Intrinsic default configuration
   * - TLP_CONFDIR
     - /etc/tlp.d
     - Directory for drop-in customizations
   * - TLP_CONFREN
     - /usr/share/tlp/rename.conf
     - Translation table for parameters
   * - TLP_CONFUSR
     - /etc/tlp.conf
     - User configuration
   * - TLP_ELOD
     - | [from 1.7] /usr/lib/elogind/system-sleep
       | [until 1.6.1] /lib/elogind/system-sleep
     - Install directory elogind suspend/resume script
   * - TLP_FISHCPL
     - /usr/share/fish/vendor_completions.d
     - Install directory for Fish completion rules
   * - TLP_FLIB
     - /usr/share/tlp/func.d
     - Install directory for function libs
   * - TLP_MAN
     - /usr/share/man
     - Install directory for manpages
   * - TLP_META
     - /usr/share/metainfo
     - Install directory for AppStream metadata
   * - TLP_NMDSP
     - | [from 1.6] /usr/lib/NetworkManager/dispatcher.d
       | [until 1.5] /etc/NetworkManager/dispatcher.d
     - Install directory for NetworkManager hooks
   * - TLP_NO_BASHCOMP
     - 0
     - 1=do not install Bash completion rules
   * - TLP_NO_FISHCOMP
     - 0
     - 1=do not install Fish completion rules
   * - TLP_NO_INIT
     - 0
     - 1=do not install tlp.init
   * - TLP_NO_TPACPI
     - [until 1.6.1] 0
     - 1=do not install tpacpi-bat
   * - TLP_NO_ZSHCOMP
     - 0
     - 1=do not install ZSH completion rules
   * - TLP_RUN
     - /run/tlp
     - Directory for runtime data (volatile)
   * - TLP_SBIN
     - /usr/sbin
     - Install directory
   * - TLP_SDSL
     - | [from 1.7] /usr/lib/systemd/system-sleep
       | [until 1.6.1] /lib/systemd/system-sleep
     - Install directory for systemd suspend/resume hooks
   * - TLP_SHCPL
     - /usr/share/bash-completion/completions
     - Install directory for Bash completion rules
   * - TLP_SYSD
     - | [from 1.7] /usr/lib/systemd
       | [until 1.6.1] /lib/systemd
     - Install directory for systemd units
   * - TLP_SYSV
     - /etc/init.d
     - Install directory for sysvinit script
   * - TLP_TLIB
     - /usr/share/tlp
     - Install directory for function libs
   * - TLP_ULIB
     - | [from 1.7] /usr/lib/udev
       | [until 1.6.1] /lib/udev
     - Install directory for udev scripts
   * - TLP_VAR
     - /var/lib/tlp
     - persistent storage directory
   * - TLP_WITH_ELOGIND
     - 1
     - 1=install elogind script
   * - TLP_WITH_SYSTEMD
     - 1
     - 1=install systemd unit files
   * - TLP_ZSHCPL
     - /usr/share/zsh/site-functions
     - Install directory for ZSH completion rules
