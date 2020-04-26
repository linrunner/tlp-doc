Installation Scripts
====================

.. rubric:: Post Installation Script (package `tlp`)

.. code-block:: sh

    # Mask conflicting systemd services
    for i in $(ls /lib/systemd/system 2> /dev/null | egrep "systemd-rfkill"); do
        systemctl mask $i 2> /dev/null || true
    done

.. rubric:: Post Removal Script (package `tlp`)

.. code-block:: sh

    # Unmask conflicting systemd services
    for i in $(ls /lib/systemd/system 2> /dev/null | egrep "systemd-rfkill"); do
        systemctl unmask $i 2> /dev/null || true
    done
    ;;


