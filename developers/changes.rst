Dev's Changelog
===============
This section lists changes that are relevant for packaging TLP.
For feature changes see the
`user oriented changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_.

Version 1.9 (beta)
    Files:

    - New: **bat.d/70-tuxedo**

    New subpackage: tlp-pd
        New files:

        - **tlpctl** - install to **/usr/bin/**
        - **tlp-pd** - install to **/usr/sbin/**
        - **tlp-pd.service**
        - **tlp-pd.dbus.conf** - Makefile performs pattern substitution and
          installs two copies to **/usr/share/dbus-1/system.d/**
        - **tlp-pd.dbus.service** - Makefile performs pattern substitution and
          installs two copies to **/usr/share/dbus-1/system-services/**
        - **tlp-pd.policy** - install to **/usr/share/polkit-1/actions/**
        - New manpages: **tlpctl.1**, **tlp-pd.8**, **tlp-pd.service.8**

        Makefile:

        - New parameters: *TLP_DBCONF*, *TLP_DBSVC*, *TLP_POLKIT*
        - New targets: *install-pd*, *install-man-pd*, *uninstall-pd*, *uninstall-man-pd*

        Dependencies:

        - **python3**
        - **dbus-python**
        - **PyGObjects**

        Conflicts:

        - **power-profiles-daemon**
        - **tuned-ppd**


Version 1.8
    Files:

    - New: **bat.d/55-cros-ec, 56-framework, 65-dell**


Version 1.7
    Makefile:

    - Changed parameter defaults: `TLP_ELOD`, `TLP_SDSL`, `TLP_SYSD`, `TLP_ULIB` -
      install to **/usr/lib** instead of **/lib** (finish UsrMerge)
    - New parameters: `TLP_FISHCPL`, `TLP_NO_FISHCOMP`
    - Removed parameters: `TLP_NO_TPACPI`

    Files:

    - New:

      - **completion/fish/**
      - **bat.d/25-msi, 60-macbook**

    - Removed: **tpacpi-bat**

    Dependencies:

    - Add:

        - **auto-cpufreq** - conflicts
        - **tuned** - conflicts

    - Remove:

        - **acpi-call** - superfluous after tpacpi-bat was removed!
        - **x86_energy_perf_policy** - unnecessary, as backward compatibility
          for EPB with kernels < 5.2 was removed

Version 1.6, 1.6.1
    Makefile:

    - Changed parameter default: `TLP_NMDSP`

      - NetworkManager hook install relocated to
        **/usr/lib/NetworkManager/dispatcher.d/99tlp-rdw-nm**
      - Ensure to remove the old file
        **/etc/NetworkManager/dispatcher.d/99tlp-rdw-nm**
        on package upgrade

    - New parameters: `TLP_ZSHCPL`, `TLP_NO_ZSHCOMP`

    Files:

    - Moved: **tlp*.bash_completion** relocated to **completions/bash/**
    - New:

      - **completion/zsh/_tlp***
      - **deprecated.conf**

    Dependencies:

    - Remove: **acpi-call** - kernel 5.17 satisfies all requirements
      of ThinkPad battery care

Version 1.5
    Files:

    - New command: **nfc** (symlink to **bluetooth**)
    - New manpage: **nfc.1**

    Dependencies:

    - **Conflicts power-profiles-daemon** instead of masking the
      service via package post-install routine; for reasons and further explanation
      see :ref:`Dependencies <dev-depend-ppd>`

    - Removed: `wireless-tools`

Version 1.4
    Source:

    - Branch `master` renamed to `main`
    - New directory: **unit-tests/** - do not install

    Dependencies:

    - Removed: `lsb-release`

    Files:

    - New directory with battery driver plugins: **/usr/share/tlp/bat.d/**
    - New file: **/usr/share/tlp/rename.conf**

    Makefile:

    - New parameters: `TLP_BATD`, `TLP_CONFREN`
    - New target: `chkbatdrv`

    Installation:

    - Add `systemctl mask power-profiles-daemon` to the package post-install
      routine - and vice versa for post-remove
      (see `Issue #564 <https://github.com/linrunner/TLP/issues/564>`_)

Version 1.3
    New configuration scheme:

    - **/usr/share/tlp/defaults.conf** – Intrinsic defaults
    - **/etc/tlp.d/** – Directory for drop-in configuration (by separate packages)

      - **/etc/tlp.d/00-template.conf** – Template
      - **/etc/tlp.d/README** – Explains the directory
      - **/etc/tlp.conf** – User configuration

    Files:

    - New helper script: **tlp-readconfs**
    - Systemd service **tlp-sleep.service** replaced by hook **tlp-sleep**
    - Removed: **default**
    - Removed manpages: **tlp-pcilist.1**, **tlp-usblist.1**, **tlp-sleep.service.8**

    Makefile:

    - Restructured manpage install/uninstall targets:

      - Changed: `[un]install-man` depends on `[un]install-man-tlp` and `[un]install-man-rdw`
      - New: `install-man-tlp`, `uninstall-man-tlp`

    - New targets: `checkwip`, `perlcritic`
    - New parameters: `TLP_CONFDEF`, `TLP_CONFDIR`, `TLP_CONFUSR`, `TLP_SDSL`
    - Install **tlp-pcilist**, **tlp-usblist** to **/usr/share/tlp/**

    This document: removed references for version 1.1 and older

Version 1.2.2
    Files:

    - New directory: **/var/lib/tlp/**

    Makefile:

    - Changed parameter defaults: `TLP_WITH_SYSTEMD=1`, `TLP_WITH_ELOGIND=1`
    - New parameter: `TLP_VAR`

Version 1.2
    Files:

    - New command: **tlp-rdw**
    - New libraries: **tlp-func-base**, **func.d/***
    - New manpage: **tlp-rdw.8**
    - New hook: **tlp-sleep.elogind**
    - Renamed file: **README** → **README.md**
    - Removed libraries: **tlp-functions**, **tlp-rf-func**
    - Removed `pm-utils` hooks: **49tlp**, **tlp-nop**

    Makefile:

    - New targets: `install-man-rdw`, `uninstall-man-rdw`, `checkall`,
      `checkdupconst`, `shellcheck`
    - New parameters: `TLP_ELOD`, `TLP_FLIB`, `TLP_WITH_ELOGIND`
    - Removed parameters: `TLP_NO_PMUTILS`
    - Systemd: ``Wants=bluetooth.service NetworkManager.service`` removed from
      **tlp.service**.

Version 1.1
    Files:

    - New AppStream metadata: **de.linrunner.tlp.metainfo.xml**

    Makefile:

    - `TLP_META`: install AppStream metadata to **/usr/share/metainfo**
    - `TLP_RUN`: store runtime data in **/run/tlp**; previously and deprecated:
      **/var/run/tlp**
