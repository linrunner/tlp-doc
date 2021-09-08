.. warning::

    .. rubric:: Conflict with power-profiles-daemon

    `power-profiles-daemon <https://gitlab.freedesktop.org/hadess/power-profiles-daemon>`_
    (contained in GNOME 40 and newer) might be contained in the default install
    of your distribution.

    **Symptom:** `power-profiles-daemon.service` prevents TLP from making power
    saving settings at system startup. A check with ::

        systemctl status power-profiles-daemon.service tlp.service

    generates the following output in the conflict case: ::

        ● power-profiles-daemon.service - Power Profiles daemon
             Loaded: loaded (/lib/systemd/system/power-profiles-daemon.service; enabled; vendor preset: enabled)
             Active: active (running) since Wed 2021-09-08 09:55:50 CEST; 2min 13s ago
           Main PID: 413 (power-profiles-)
              Tasks: 3 (limit: 4506)
             Memory: 1.1M
             CGroup: /system.slice/power-profiles-daemon.service
                     └─413 /usr/libexec/power-profiles-daemon

        Sep 08 09:55:50 host systemd[1]: Starting Power Profiles daemon...
        Sep 08 09:55:50 host systemd[1]: Started Power Profiles daemon.

        ● tlp.service - TLP system startup/shutdown
             Loaded: loaded (/lib/systemd/system/tlp.service; enabled; vendor preset: enabled)
             Active: inactive (dead)
               Docs: https://linrunner.de/tlp

    Beginning with version 1.4 :command:`tlp start` and :command:`tlp-stat -s`
    will also detect the conflict: ::

        Error: conflicting power-profiles-daemon.service is enabled, power saving will not apply on boot.
        >>> Invoke 'systemctl mask power-profiles-daemon.service' to correct this!


    **Solution:** disable `power-profiles-daemon` with ::


        sudo systemctl mask power-profiles-daemon.service


    and reboot.
