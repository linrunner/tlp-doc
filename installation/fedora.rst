Fedora
======
.. rubric:: Scope:

* Officially supported Fedora releases

.. note::

    Execute the commands below in a root shell or with with a preceding :command:`sudo`.

Package Installation
--------------------
TLP packages are available from the official Fedora repositories:

* **tlp** *(Updates)* – Power saving
* **tlp-rdw** *(Updates)* – optional – :doc:`/settings/rdw`

Hint: packages for RHEL/CentOS are available from the EPEL repositories.

Install the packages either with your favorite package manager or the command: ::

   dnf install tlp tlp-rdw

Uninstall the conflicting `power-profiles-daemon` package: ::

   dnf remove power-profiles-daemon

Service Units
-------------
To complete the installation you must enable TLP's service: ::

   systemctl enable tlp.service

You should also mask the following services to avoid conflicts and assure proper
operation of TLP's :doc:`/settings/radio` options: ::

   systemctl mask systemd-rfkill.service systemd-rfkill.socket

.. seealso::

    * FAQ: :ref:`Service units <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`

ThinkPads only: External Kernel Modules
---------------------------------------
.. important::

    Fedora 36 was released with Linux kernel 5.17. In combination with TLP 1.5
    or newer it offers full battery care support (i.e. charge thresholds and
    recalibration) for ThinkPads from model year 2011 onwards.

    **Therefore no external kernel modules are required and you do not need to proceed
    any further here.**

    However, continue reading if your ThinkPad model is from 2011 or older.

ThinkPad models before 2011 (i.e. T410/X201 and older) require the
external kernel module `tp_smapi` to provide battery charge thresholds
and recalibration.
For T420/X220 (model year 2011) only `tp_smapi` is not a hard requirement
but they benefit by having :command:`tlp-stat -b` display the battery cycle
count and other additional information.
The output of :command:`tlp-stat -b` will recommend to install `tp_smapi`
accordingly.

The necessary packages are not available from the official Fedora repositories.
Instead you need to add the `RPM Fusion` and `ThinkPad Extras` repositories: ::

   dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
   dnf install https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm

.. note::

    Above steps are only needed on a new installation of Fedora *but not* after release
    upgrades.

Required packages:

* **kernel-devel** *(Fedora repo)* – Needed to build the kernel module from
  the `akmod` package
* **akmod-tp_smapi** *(ThinkPad Extras repo)* – optional – External kernel
  module source providing battery charge thresholds and recalibration

Install either with your favorite package manager
or the command ::

   dnf install kernel-devel akmod-tp_smapi

New packages are available first in the testing repository: ::

   dnf --enablerepo=tlp-updates-testing install kernel-devel akmod-tp_smapi

.. important::

    * The `akmod-*` package is provided "as is" by a volunteer, it is
      not part of the TLP project
    * Please *do not file issues* if it is not yet available for the
      latest Fedora version, better watch the `tlp-updates-testing` repository
    * In case of difficulties installing, please ask for help in your
      preferred Fedora forum

.. note::

    * The RPM Fusion repo delivers build dependencies for the `akmod-*` packages
    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

How to validate the Repository Keys
-----------------------------------
Kernel module packages provided by the ThinkPad Extras repository for Fedora are
signed with a release specific key. Yo may check the fingerprint with the
following procedure.

1. Download the key:

.. code-block:: none

    wget https://repo.linrunner.de/fedora/tlp/repos/RPM-GPG-KEY-tlp-fedora-38-primary

2. Get the fingerprint:

.. code-block:: none

    gpg -n -q --import --import-options import-show RPM-GPG-KEY-tlp-fedora-38-primary

3. Check that the resulting fingerprint matches the fingerprint from the list below.

4. If they match, import the key:

.. code-block:: none

    rpm --import RPM-GPG-KEY-tlp-fedora-38-primary

Fingerprints
------------

RPM-GPG-KEY-tlp-fedora-39-primary: ::

    61A3 F536 A295 C543 C90B 6583 F211 4CD7 DD65 A6C4

RPM-GPG-KEY-tlp-fedora-38-primary: ::

    18E9 1496 E81A 2040 F94E C306 B3BE 4F28 7F13 C3C8

RPM-GPG-KEY-tlp-fedora-37-primary: ::

    666F 0F62 9C09 5486 7FA9 7C55 4E41 F248 779F E8EE

RPM-GPG-KEY-tlp-fedora-36-primary: ::

    B1F7 4D6D 9F56 93BB 1A9C 9D64 85F1 A909 051D B38A

RPM-GPG-KEY-tlp-fedora-35-primary: ::

    65C4 7531 819C 6D74 33BE 25D5 5052 26CB 40D9 3801

RPM-GPG-KEY-tlp-fedora-34-primary: ::

    1E4F 2F53 A348 6025 FC4E FD86 7704 0BAF FA30 D1C8
