Installation Scripts
====================

.. rubric:: Post Installation Script (package `tlp`)

.. code-block:: sh

    # Mask conflicting systemd services
    for i in systemd-rfkill.service systemd-rfkill.socket; do
        systemctl mask $i 2> /dev/null || true
    done

.. rubric:: Post Removal Script (package `tlp`)

.. code-block:: sh

    # Unmask conflicting systemd services
    for i in systemd-rfkill.service systemd-rfkill.socket; do
        systemctl unmask $i 2> /dev/null || true
    done
    ;;


