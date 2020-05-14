Fedora
======
.. rubric:: Scope:

* Officially supported Fedora releases

.. note::

    Execute the commands below in a root shell.

Package Installation
--------------------
TLP packages are available from the official Fedora repositories:

* **tlp** *(Updates)* – Power saving
* **tlp-rdw** *(Updates)* – optional – :doc:`/settings/rdw`

Hint: packages for RHEL/CentOS are available from the EPEL repositories.

Install the packages either with your favorite package manager or the command: ::

   dnf install tlp tlp-rdw

ThinkPads only
^^^^^^^^^^^^^^
Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The necessary packages are not available from the official Fedora repositories.
Instead you need to add the `RPM Fusion` and `ThinkPad Extras` repositories: ::

   dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
   dnf install https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will guide
you which package to install:

* **kernel-devel** *(Fedora repo)* – Needed for the `akmod` packages below
* **akmod-acpi_call** *(ThinkPad Extras repo)* – optional – External kernel module providing
  battery charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)
* **akmod-tp_smapi** *(ThinkPad Extras repo)* – optional – External kernel module providing
  battery charge thresholds, recalibration and specific :command:`tlp-stat -b`
  output for older ThinkPads

.. note::

    The RPM Fusion repo delivers build dependencies for the `akmod-*` packages.

.. important::

    * The `akmod-*` packages are provided "as is" by a volunteer, they are
      not part of the TLP project
    * Please *do not file issues* if they are not yet available for the
      latest Fedora version
    * In case of difficulties installing them, please ask for help in your
      preferred Fedora forum

Install them either with your favorite package manager or the command ::

   dnf install kernel-devel akmod-acpi_call akmod-tp_smapi

omitting the one not required by your hardware.

New packages are available first in the testing repository: ::

   dnf --enablerepo=tlp-updates-testing install kernel-devel akmod-acpi_call akmod-tp_smapi

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

How to validate the Repository Keys
-----------------------------------
Kernel module packages provided by the ThinkPad Extras repository for Fedora are
signed with a release specific key. Yo may check the fingerprint with the
following procedure.

1. Download the key:

.. code-block:: none

    wget https://repo.linrunner.de/fedora/tlp/repos/RPM-GPG-KEY-tlp-fedora-32-primary

2. Get the fingerprint:

.. code-block:: none

    gpg -n -q --import --import-options import-show RPM-GPG-KEY-tlp-fedora-32-primary

3. Check that the resulting fingerprint matches the fingerprint from the list below.

4. If they match, import the key:

.. code-block:: none

    rpm --import RPM-GPG-KEY-tlp-fedora-32-primary

Fingerprints
^^^^^^^^^^^^
RPM-GPG-KEY-tlp-fedora-32-primary: ::

    6BED 8C16 80E0 E9DC D310 94FB 274D 8DB1 A690 281B

RPM-GPG-KEY-tlp-fedora-31-primary: ::

    685D B6BB 26B9 A03B 2924 71CF 3CA1 F6C1 B629 712A

RPM-GPG-KEY-tlp-fedora-30-primary: ::

    8130 3994 EEAF 1CC5 2AC1 DED7 2DDA 0C47 9F42 55D8

RPM-GPG-KEY-tlp-fedora-29-primary: ::

    45CE 5574 CA74 65D1 90A9 9EB2 F59A C581 180C 9484

RPM-GPG-KEY-tlp-fedora-28-primary: ::

    C807 AEB6 3DD0 4587 E695 9DD2 455A 80BA 1A85 3C73

RPM-GPG-KEY-tlp-fedora-27-primary: ::

    9EEE ADC8 9282 2138 F017 7E41 9D87 D611 5CE7 AC42

RPM-GPG-KEY-tlp-fedora-26-primary: ::

    A6AA 476D 471E 05A5 5CA2 8EDE 097F 6445 1482 D93F

RPM-GPG-KEY-tlp-fedora-25-primary: ::

    F4BC 65CB 2E7E 83F4 7C87 914A 5096 4F53 2058 F5CF


