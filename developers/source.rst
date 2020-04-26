Source
======

.. _dev-get-source:

Code
----
TLP's source code is hosted on `GitHub <https://github.com/linrunner/TLP>`_.
The repository holds the following branches:

* **master** – current development – releases are tagged with VERSION
* **debian/current** – Debian/Ubuntu packing for current releases
* debian/legacy – Debian/Ubuntu packing for old releases (deprecated, unmaintained)

Obtain the current development version with ::

    git clone git://github.com/linrunner/TLP.git tlp.git
    cd tlp.git

Checkout the `master` branch ::

    git checkout master

or a release version ::

    git checkout VERSION

.. note::

    Please base your pull requests on the `master` branch.

The current changelog for the development version is
`here <https://github.com/linrunner/TLP/blob/master/changelog>`_.

Download release tarballs at the
`Release page <https://github.com/linrunner/TLP/releases>`_.

Building Packages
-----------------
Debian / Ubuntu
^^^^^^^^^^^^^^^

1. Install the package

* **packaging-dev** – Metapackage providing common packaging tools

2. Checkout debian/current branch ::

    git checkout -b debian/current origin/debian/current

3. Checkout master branch ::

    git checkout master

4. Populate **debian/** with ::

    rm -rf debian/*
    git checkout debian/current -- debian
    git rm -r --cached debian

5. Edit **debian/changelog** to your needs (optional).

6. Build binary packages with ::

    dpkg-buildpackage -b -tc -i.git

Arch Linux
^^^^^^^^^^
Packages tracking the master branch are available in the AUR:

* `tlp-git <https://aur.archlinux.org/packages/tlp-git/>`_
* `tlp-rdw-git <https://aur.archlinux.org/packages/tlp-rdw-git/>`_

.. dev-install-source:

Installing from Source
----------------------
1. Checkout the master branch (see :ref:`dev-get-source`).

2. In the **tlp.git/** directory ::

    make install
    make install-man
    systemctl enable tlp.service

.. include:: ../include/warn-source-install.rst
