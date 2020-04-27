Dev's Changelog
===============
This section lists changes that are relevant for packaging TLP.
For feature changes see the
`user oriented changelog <https://github.com/linrunner/TLP/blob/master/changelog>`_.

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
