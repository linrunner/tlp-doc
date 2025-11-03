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

    git clone https://github.com/linrunner/TLP.git tlp.git
    cd tlp.git

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

3. Symlink worktree to **debian/**

    ln -s worktree/debianc-urrent/debian debian

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
1. Checkout the `main` branch (see :ref:`dev-source-code`).

2. In the **tlp.git/** directory ::

    make install
    make install-man
    systemctl enable tlp.service
    systemctl enable tlp-pd.service

.. include:: ../include/warn-source-install.rst
