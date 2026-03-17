Installation
============
.. seealso::

    :doc:`Conflicts (with other power management tools and desktop environments) </faq/conflicts>`


.. _faq-service-units:

Must I enable TLP's systemd service units?
------------------------------------------
Symptoms: :command:`tlp-stat -s` displays one of the following messages:

.. code-block:: none

    Error: TLP's power saving will not apply on boot because tlp.service is not enabled
    --> Run 'systemctl enable tlp.service' to ensure the full functionality of TLP.

    Error: You can't switch TLP profiles by mouse click because tlp-pd.service is not enabled
    --> Run 'systemctl enable --now tlp-pd.service' to ensure the full functionality of TLP.

    Error: After the next restart, you won't be able to switch TLP profiles by mouse click because
    tlp-pd.service is not enabled --> Run 'systemctl enable tlp-pd.service' to ensure the
    full functionality of TLP.

Answer: YES, the service units are *indispensable* for correct operation -
**tlp.service** applies power saving settings and charge thresholds
as well as switching radio devices on system boot and shutdown.
**tlp-pd.service** is required to switch TLP profiles by mouse click.

.. note::

    Debian, Ubuntu and derivatives enable the service by default as part of the package
    :doc:`/installation/index`, others such as Arch Linux, Fedora and openSUSE
    don't. If unsure check the output of :command:`tlp-stat -s` for corresponding
    notes.

Does TLP run on my laptop (not a ThinkPad)?
-------------------------------------------
TLP runs on every laptop brand. A few features are available on IBM/Lenovo
ThinkPads only.

Does TLP make sense on newer laptops / with newer Linux versions?
-----------------------------------------------------------------
Yes, of course.

The Linux kernel has accumulated many power saving features over the years,
but not all are enabled by default. It seems to be really hard for the kernel
developers to fully debug power saving on all possible hardware, so power
saving stays disabled for many drivers and it's up to the user to enable it.

Conclusion: a userspace tool like TLP is still needed to enable power saving globally.

Should I install TLP inside a virtual machine?
----------------------------------------------
No. It is not effective to run a power management tool inside a virtual machine
guest. Install TLP in the host operating system instead.

Ubuntu/Debian: I do not use Network Manager, how do I install tlp without tlp-rdw?
----------------------------------------------------------------------------------
::

    sudo apt install --no-install-recommends tlp

Ubuntu: How do I prevent the installation of postfix as a dependency?
---------------------------------------------------------------------
The package `tlp` recommends `smartmontools` which pulls `postfix`
(via recommends too). Use: ::

    sudo apt install --no-install-recommends tlp tlp-rdw ethtool smartmontools


My Linux distribution does not provide a TLP package, how do I install it?
--------------------------------------------------------------------------
See :doc:`/installation/others`.

How do I install TLP on a development release of my distribution?
-----------------------------------------------------------------
TLP packages for new distribution versions appear in due time for the release.
If you want to use TLP with alpha or beta releases, download the packages for
the predecessor and install them manually with your favorite package manager.


What if I want a GUI?
---------------------
Get `TLPUI <https://github.com/d4nj1/TLPUI>`_.
