Source
======

.. _dev-source-code:

Code
----
TLP's source code is hosted on `GitHub <https://github.com/linrunner/TLP>`_.
The repository holds the following branches:

* **main** – current development – releases are tagged with VERSION
* **debian/current** – Debian/Ubuntu packaging for current releases
  (not identical with the official distribution packages)

Obtain the current development version with ::

    git clone https://github.com/linrunner/TLP.git
    cd TLP

Checkout the `main` branch ::

    git checkout main

or a release version ::

    git checkout VERSION

.. note::

    Please base your pull requests on the `main` branch.

The current changelog for the development version is
`here <https://github.com/linrunner/TLP/blob/main/changelog>`_.

Download release tarballs at the
`Release page <https://github.com/linrunner/TLP/releases>`_.

Building Packages
-----------------
Debian / Ubuntu
^^^^^^^^^^^^^^^

1. Install the required package

* **packaging-dev** – Metapackage providing common packaging tools

2. Add a Git worktree for the `debian/current` branch ::

    git worktree add worktree/debian-current debian/current

3. Symlink the worktree to **debian/**

    ln -s worktree/debian-current/debian debian

4. Edit **debian/changelog** to your needs (optional).

6. Build binary packages with ::

    dpkg-buildpackage -b -tc -i.git

Arch Linux
^^^^^^^^^^
Packages tracking the `main` branch are available in the AUR:

* `tlp-git <https://aur.archlinux.org/packages/tlp-git/>`_
* `tlp-rdw-git <https://aur.archlinux.org/packages/tlp-rdw-git/>`_
* `tlp-pd-git <https://aur.archlinux.org/packages/tlp-pd-git/>`_

.. dev-install-source:

Installing from Source
----------------------
1. If applicable, uninstall any existing TLP packages using your package manager.

2. Install all dependencies as listed in :doc:`dependencies`.

3. Stop and remove conflicting tools such as `power-profiles-daemon`
   (refer to :doc:`dependencies`).

4. Checkout the `main` branch and change to the **TLP/** directory as described in the
   :ref:`dev-source-code` section above.

5. Install: ::

    # Please copy and save the complete output for further reference
    sudo make install install-man

6. Enable the services: ::

    sudo systemctl enable --now tlp-pd.service
    sudo systemctl enable --now tlp.service

.. include:: ../include/warn-source-install.rst

Removing a Source Install
-------------------------
 In the **TLP/** directory: ::

    sudo make uninstall uninstall-man

.. warning::

    Please do not use to remove a package-based installation!
