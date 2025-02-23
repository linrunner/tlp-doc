Fedora
======

.. important::

   Good news! The issues with SELinux have been fixed in Fedora 40,
   so you can enjoy full TLP functionality again.

.. rubric:: Scope:

* Officially supported Fedora releases

Package Installation
--------------------
TLP packages are available from the official Fedora repositories:

* **tlp** *(Updates)* – Power saving
* **tlp-rdw** *(Updates)* – optional – :doc:`/settings/rdw`

Hint: packages for RHEL/CentOS are available from the EPEL repositories.

Install the packages either with your favorite package manager or the command: ::

   sudo dnf install tlp tlp-rdw

TLP Repository
^^^^^^^^^^^^^^
If the latest TLP version is not yet available in the official Fedora
repositories, you can get it from the `TLP` repository, which
can be set up with the following command: ::

   sudo dnf install https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm

Hint: the above step is only needed after a clean Fedora installation,
not after release upgrades.

After the repository is set up, install TLP with the command: ::

   sudo dnf install tlp tlp-rdw

Remove conflicting packages
^^^^^^^^^^^^^^^^^^^^^^^^^^^
After installing TLP, conflicting packages must be uninstalled:

* Fedora version 40 and below ::

   sudo dnf remove power-profiles-daemon

* Fedora version 41 and newer ::

   sudo dnf remove tuned tuned-ppd

.. seealso::

    FAQ: :doc:`/faq/ppd`

Service Units
-------------
To complete the installation you must enable TLP's service: ::

   sudo systemctl enable tlp.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

   sudo systemctl mask systemd-rfkill.service systemd-rfkill.socket

.. seealso::

    FAQ: :ref:`Service units <faq-service-units>`

Legacy ThinkPads only: External Kernel Module for Battery Care
--------------------------------------------------------------
.. important::

    Fedora 40 was released with Linux kernel 6.8. In combination with TLP 1.5
    or newer it offers full battery care support (i.e. charge thresholds and
    recalibration) for ThinkPads from model year 2011 onwards. The same applies
    to later Fedora versions.

    **An external kernel module (also referred to as "out-of-tree" module)
    is not required in this case, and the following steps are not necessary.
    However, if your model is from 2011 or older, read on.**

Only if the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
shows the line

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration

then install the required kernel modules. They are not available from the official Fedora repositories.
Instead you need to add the `TLP` (see above) and `RPM Fusion` repositories: ::

   sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

Hint: the above step is only needed after a clean Fedora installation,
not after release upgrades.

Required packages:

* **kernel-devel** *(Fedora repo)* – Needed to build the kernel module from
  the `akmod` package
* **akmod-tp_smapi** *(TLP repo)* – optional – External kernel
  module source providing battery charge thresholds and recalibration

Install either with your favorite package manager
or the command ::

   sudo dnf install kernel-devel akmod-tp_smapi

New packages are available in the `tlp-updates-testing` repository first: ::

   sudo dnf --enablerepo=tlp-updates-testing install kernel-devel akmod-tp_smapi

.. important::

    * The `akmod-*` package is provided "as is" by a volunteer, it is
      not part of the TLP project
    * Please *do not file issues* if it is not yet available for the
      latest Fedora version, better watch the `tlp-updates-testing` repository
    * In case of difficulties installing, please ask for help in your
      preferred Fedora forum

.. note::

    * The RPM Fusion repository delivers build dependencies for the `akmod-*` packages
    * You must disable Secure Boot to use the ThinkPad specific packages

How to validate the Repository Keys
-----------------------------------
Kernel module packages provided by the ThinkPad Extras repository for Fedora are
signed with a release specific key. Yo may check the fingerprint with the
following procedure.

1. Download the key:

.. code-block:: none

    wget https://repo.linrunner.de/fedora/tlp/repos/RPM-GPG-KEY-tlp-fedora-41-primary

2. Get the fingerprint:

.. code-block:: none

    gpg -n -q --import --import-options import-show RPM-GPG-KEY-tlp-fedora-41-primary

3. Check that the resulting fingerprint matches the fingerprint from the list below.

4. If they match, import the key:

.. code-block:: none

    sudo rpm --import RPM-GPG-KEY-tlp-fedora-41-primary

Fingerprints
------------

RPM-GPG-KEY-tlp-fedora-42-primary: ::

    12D4 0BFD 3011 21FE 3FB5 C015 7586 7412 AC4D D754

RPM-GPG-KEY-tlp-fedora-41-primary: ::

    BFC3 0267 A648 4B13 0A8B D63A 5A95 D830 9811 B297

RPM-GPG-KEY-tlp-fedora-40-primary: ::

    C279 E61F 6B48 9D22 A672 F8B1 B478 BF61 B8E3 FA4C

RPM-GPG-KEY-tlp-fedora-39-primary: ::

    61A3 F536 A295 C543 C90B 6583 F211 4CD7 DD65 A6C4
