Battery Care
============

What is "Battery Life"?
-----------------------
* `Battery life` is the amount of time your device runs before it needs to be
  recharged, it's also called `battery runtime`. It's the task of
  TLP's power saving :ref:`features <intro-features>` to extend it.
* In contrast `battery lifespan` is the amount of time your battery lasts until
  it needs to be replaced. This is where `battery care` comes in.


.. _faq-battery-care:

What is "Battery Care"?
-----------------------
.. include:: ../include/battery-care-explain.rst

.. rubric:: How does it work?

The process of battery charging is not directly controlled by TLP, but by the
embedded controller (EC) of your laptop. This makes the process work even when
the laptop is switched off or no operating system is running. The contribution
of TLP is to write the threshold values into the corresponding control registers
of the EC using a hardware specific kernel driver.

The charging process is determined by the charge thresholds:

.. include:: ../include/charge-threshold-effect.rst

You cannot change the basic behavior described above, because it is hard-coded
into the EC firmware by the vendor. However, you can control it by setting
thresholds using TLP.

Charge thresholds written into the EC registers are persistently stored
in the hardware of many vendors. Which means they are retained
when another operating system is started - provided they are not changed by
software installed there and the hardware actually supports persistence.

.. rubric::  What charge thresholds do `not` do

* They do not exercise any control over the discharging of the battery,
  this depends solely on whether AC is connected and if the laptop is switched on
* In particular, with AC connected, a battery with a charge level higher than
  the stop charge threshold will not be discharged to the stop charge threshold,
  nor will there be a (cyclic) discharge down to the start charge threshold

.. seealso::

    * `How to prolong lithium-based batteries <https://batteryuniversity.com/article/bu-808-how-to-prolong-lithium-based-batteries>`_
      - Basics on the ageing of batteries and the effect of charge thresholds
    * The level of battery care support depends on laptop vendor or brand, Linux
      kernel version and TLP version - consult :doc:`/settings/bc-vendors` for details
    * Settings: :doc:`/settings/battery`
    * Commands: :ref:`cmd-tlp-battery-features`
    * :ref:`faq-discharging-misconception`


What is the purpose of battery charge thresholds?
-------------------------------------------------
See :ref:`above <faq-battery-care>`.

How do battery charge thresholds work?
--------------------------------------
See :ref:`above <faq-battery-care>`.


.. _faq-good-battery-thresholds:

How to choose good battery charge thresholds?
---------------------------------------------
.. note::

    Newer ThinkPad models may not need charge thresholds due to dualmode battery
    firmware – a Lenovo staff member states at
    `Lenovo Forums [1] <https://forums.lenovo.com/t5/Windows-10/Power-Manager-for-Windows-10/m-p/2129075#M794>`_:
    *"The battery firmware itself will recognize the scenario where the battery
    is ALWAYS fully charged 100% (over a period of many weeks) and adjust the
    full charge capacity downwards in a way to maintain maximum battery health.
    This is something that happens automatically in the battery firmware.
    There is nothing that a user needs to do manually, to maximize battery health
    on these batteries. For this reason, we don't provide any utility to manually
    manage battery charge thresholds [...]"*


Factory settings for ThinkPad battery thresholds are as follows: when plugged in
the battery starts charging at 96%, and stops at 100%. These settings are optimized
for maximum runtime, but having a battery hold a lot of power will decrease its
capacity over the years. To alleviate this problem, the start/stop charge thresholds
can be adjusted – at the cost of a more or less reduced battery runtime.

It all depends on how you use your laptop, or more precisely, on the minimal
runtime you're ready to accept when you're on the road. In the end, it all comes
down to a runtime vs. lifespan trade-off.

If the laptop is plugged most of the time and rarely unplugged, maximizing battery
lifetime at the cost of a greatly reduced runtime may be acceptable, with values
like starting charge at 40% and stopping at 50%.

On the contrary, if you use it unplugged most of the time, starting charge at
85% and stopping at 90% would allow for a much longer runtime and still give a
lifespan benefit over the factory settings.

Sources:

* `Lenovo Support: Use maximum runtime (hours) or maximum lifespan (years) in battery settings?
  <https://support.lenovo.com/bd/en/solutions/ht078208-how-can-i-increase-battery-life-thinkpad-and-lenovo-vbke-series-notebooks>`_
* `Lenovo Forums [2] <https://forums.lenovo.com/t5/Welcome-FAQs-Knowledge-Base/How-can-I-increase-battery-life-ThinkPad/ta-p/244800>`_

Default TLP settings (only if you uncomment the relevant lines) are slightly more
protective regarding lifespan, with 75/80% charge thresholds.

.. note::

    Please always consider that the start threshold is the critical constraint
    for runtime, because it defines the lowest charge level that can occur while
    plugged. Remember that TLP provides a command (:command:`tlp fullcharge`)
    to fully charge the battery, when you need to temporarily maximize runtime
    (for example in case of a trip).


.. _faq-how-to-set-thresholds:

How to enable charge thresholds?
--------------------------------
It is not enough to set charge thresholds once with the
:command:`tlp setcharge` :ref:`command <cmd-tlp-battery-features>`.
To make charge thresholds permanent, even if the hardware does not have the
ability to keep them persistent, you have to enter them into the
:ref:`configuration file <set-config-files>`, for example **/etc/tlp.conf**: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

Then apply your changed configuration with the command

::

    sudo tlp start

.. seealso::

    * Please check :doc:`/settings/bc-vendors` for permitted threshold values
      of your laptop.
    * :ref:`faq-how-to-disable-thresholds`


How can I check if my configured charge thresholds are working?
---------------------------------------------------------------
The output of :command:`tlp-stat -b` shows two characteristics for the positive
case:

1. An active battery care driver for the charge thresholds
2. The actual charge threshold(s) – read back from the embedded controller

So if there is a line containing `Supported features: charge threshold(s)`
and the displayed threshold(s) match(es) the one(s) you configured, then the
charging logic has properly received them. The article :doc:`/settings/bc-vendors`
shows :command:`tlp-stat -b` sample outputs for all supported hardware to compare.
If your :command:`tlp-stat -b` output does not contain the required
characteristics, check the following sections for solutions.

However, if despite a correctly set up system the charge threshold(s) do(es)
not work as you expect them to, then you should first compare your idea of
the charging process with the description :ref:`above <faq-battery-care>`
and subsequently check the sections further down for possible explanations.

.. note::

    On some models the displayed threshold values do not correspond to
    the configured ones, although they work as they should
    – refer to :ref:`faq-elsy-threshold-values`.


.. _faq-which-kernel-module:

Which external kernel module do I need for my ThinkPad?
-------------------------------------------------------
.. include:: ../include/thinkpad-kernel-modules.rst

.. note::

    `thinkpad_acpi` is not an external kernel module and you do not normally have
    to worry about it. It is contained in the Linux kernel and all distributions
    provide it as part of their kernel packages. ThinkPads load it automatically
    at boot time.

Prerequisite: make sure to install the most recent version of TLP for
accurate recommendations.

Check the bottom of the output of :command:`tlp-stat -b`, section 'Recommendations',
for the following lines

.. code-block:: none

    Install tp-smapi kernel modules for ThinkPad battery thresholds and recalibration
    Install acpi_call kernel module for ThinkPad battery recalibration

and install the required external kernel module package as explained in
:doc:`/installation/index` for your distribution.

Most ThinkPads only need one of the two external kernel modules, if any.
Check the output of :command:`tlp-stat -b` for lines like:

.. code-block:: none

    tp-smapi = inactive (ThinkPad not supported)
    tpacpi-bat (acpi_call)  = inactive (superseded by natacpi)

and remove the unnecessary module package(s).


.. rubric:: natacpi – Ultimate solution

Starting with kernel 5.17 `tpacpi-bat` is completely superseded by the new,
native kernel API called `natacpi` (contained in the ubiquitious kernel module
`thinkpad_acpi`). :command:`tlp-stat -b` indicates this as follows:

*Version 1.5 and later*

.. code-block:: none
    :emphasize-lines: 5

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds, recalibration)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     96 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =    100 [%]
    /sys/class/power_supply/BAT0/charge_behaviour               = [auto] inhibit-charge force-discharge

Kernels 4.19 through 5.16 support only charge thresholds and still require
the help of the external kernel module `acpi_call` for recalibration:

*Version 1.4*

.. code-block:: none
    :emphasize-lines: 6

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds)
    * tpacpi-bat (acpi_call)  = active (recalibration)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     96 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =    100 [%]
    tpacpi-bat.BAT0.forceDischarge                              =      0

*Version 1.3*

.. code-block:: none
    :emphasize-lines: 3

    +++ Battery Features
    natacpi = active (data, thresholds)
    tpacpi-bat = active (recalibrate)
    tp-smapi = inactive (ThinkPad not supported)
    ...
    /sys/class/power_supply/BAT0/charge_start_threshold         =     96 [%]
    /sys/class/power_supply/BAT0/charge_stop_threshold          =    100 [%]
    tpacpi-bat.BAT0.forceDischarge                              =      0

.. seealso::

    * :doc:`/settings/bc-vendors` - Details on hardware support
    * `tp-smapi <https://www.thinkwiki.org/wiki/Tp_smapi>`_
      – Documentation for the external kernel modules required for ThinkPads
      until model year 2011
    * `tpacpi-bat <https://github.com/teleshoes/tpacpi-bat>`_
      – Source code for the tool that is included in TLP to provide battery recalibration
      for ThinkPads since model year 2011 - e.g. T420/X220 and newer
    * `acpi_call <https://github.com/nix-community/acpi_call>`_
      – Source code of the external kernel module required by `tpacpi-bat`


.. _faq-bc-coreboot-thinkpad:

Are ThinkPads with coreboot supported?
--------------------------------------
*Version 1.5 and older*

Some models are not recognized as ThinkPads because of an incorrect model
string provided by coreboot. As a result battery care is not activated
(see Issue `#657 <https://github.com/linrunner/TLP/issues/657>`_). Sample output
of :command:`tlp-stat`:

.. code-block:: none
    :emphasize-lines: 2, 3

    +++ Battery Care
    Plugin: generic
    Supported features: none available
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =      0 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =    100 [%]

When the recognition works, the capabilities are limited:

* Charge thresholds: work.
* Recalibration: not supported
  (see `Issues #547 <https://github.com/linrunner/TLP/issues/547>`_,
  `#626 <https://github.com/linrunner/TLP/issues/626>`_). Sample output
  of :command:`tlp-stat`: ::

    /sys/class/power_supply/BAT1/charge_behaviour               = [auto]

* Battery status: battery readings `energy_full_design`, `energy_full` and
  `energy_now` are missing from the :command:`tlp-stat -b` output because
  coreboot supplies incorrect data to the Linux kernel
  (see `Issue #657 <https://github.com/linrunner/TLP/issues/657>`_);
  sample output:

.. code-block:: none
    :emphasize-lines: 15

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds, recalibration)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1:  0(off)..96(default)..99
    * STOP_CHARGE_THRESH_BAT0/1:   1..100(default)

    +++ ThinkPad Battery Status: BAT0 (Ultrabay / Slice / Replaceable)
    /sys/class/power_supply/BAT1/manufacturer                   = SONY
    /sys/class/power_supply/BAT1/model_name                     = 42T4967
    /sys/class/power_supply/BAT1/cycle_count                    =      0 (or not supported)
    /sys/class/power_supply/BAT1/status                         = Discharging
    < battery readings missing here >

    /sys/class/power_supply/BAT1/charge_control_start_threshold =     50 [%]
    /sys/class/power_supply/BAT1/charge_control_end_threshold   =     60 [%]
    /sys/class/power_supply/BAT1/charge_behaviour               = [auto]

.. note::

    * Version 1.6 correctly recognizes ThinkPads with coreboot
    * Version 1.7 (unreleased) will display complete battery readings

.. _faq-thresholds-ignored:

Why is my battery charged up to 100% – ignoring the charge thresholds?
----------------------------------------------------------------------
Possible causes are:

Laptop is not supported
^^^^^^^^^^^^^^^^^^^^^^^
:doc:`/settings/bc-vendors` lists supported hardware and explains the prerequisites.

Kernel module `thinkpad_acpi` is not loaded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptoms:

.. code-block:: none
    :emphasize-lines: 2, 3

    +++ Battery Care
    Plugin: generic
    Supported features: none available

Trying to load it with :command:`sudo modprobe thinkpad_acpi` will reveal ::

    modprobe: ERROR: could not insert 'thinkpad_acpi': Invalid argument

Cause: your system configuration contains broken boot options for `thinkpad_acpi`.

Solution: check and correct your GRUB config (distribution dependent)
or module config files (**/etc/modprobe.d/*.conf**).

External kernel module is not installed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptom: :command:`tlp-stat -b` shows one of the following messages:

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' not installed)

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' not installed)

Solution: read :ref:`faq-which-kernel-module` and install the necessary packages.
If :command:`tlp-stat -b` still claims "not installed" after installing
the appropriate package, reinstall the package via shell command and check
the output for errors. See below for possible causes.


.. _faq-dkms-package:

Installation of package `tp-smapi-dkms` or `acpi-call-dkms` failed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

.. important::

    `tp-smapi`, `acpi_call` and derived packages are not provided by the TLP project.
    Please don't file bug reports for them in the TLP issue tracker.

Symptom 1 (Ubuntu): :command:`tlp-stat -b` shows one of the following messages

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' not installed)

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' not installed)

while the package installation reports

.. code-block:: none

    Setting up tp-smapi-dkms ...
    Error! Your kernel headers for kernel X.Y.0-NN-generic cannot be found.
    Please install the linux-headers-X.Y.0-NN-generic package

Solution: install package **linux-generic-headers**.

Symptom 2 (Debian, Ubuntu): :command:`tlp-stat -b` displays

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' not installed)

while package installation reports

.. code-block:: none

    Setting up acpi-call-dkms ...
    Error! Bad return status for module build on kernel: ...

Solutions:

Upgrade the kernel to 5.17 or later together with TLP version 1.5
for full natacpi support, rendering acpi_call obsolete.
Alternatively upgrade the `acpi-call-dkms` package:

* Kernel ≥ 5.13 needs at least acpi-call-dkms 1.2.2-1
  (Debian Sid/Bookworm or Ubuntu 22.04 or TLP PPA)
* Kernel ≥ 5.6 needs at least acpi-call-dkms 1.1.0-5ubuntu0.1 (Ubuntu 20.04)
  or 1.1.0-6 (Debian Bullseye/Buster Backports)

Kernel module `tp-smapi` or `acpi_call` is not loaded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptom: :command:`tlp-stat -b` displays one of the following messages

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' load error)

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' load error)

Solution: try to load manually with

.. code-block:: sh

    sudo modprobe -v tp_smapi

Go through `tp-smapi Troubleshooting <http://www.thinkwiki.org/wiki/Tp_smapi#Troubleshooting>`_
for a solution that matches the error message displayed. If this is not successful,
ask in relevant forums. The latter also applies to issues with `acpi-call`.

.. note::

    * You may need to disable Secure Boot if `tp-smapi` or `acpi_call` refuses to load,
      check your distribution's :doc:`/installation/index` instructions
    * `Coreboot` and `Libreboot` do not support `tp-smapi`
    * `tp-smapi` does not support newer models, check :ref:`faq-which-kernel-module`

Fedora release upgrade
^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

It may be necessary to rebuild the kernel modules (as root): ::

    akmods --force

ThinkPad E495
^^^^^^^^^^^^^
Symptom: thresholds have been written to the Embedded Controller (EC),
:command:`tlp-stat -b` reads them back as configured
(see `Issue #454 <https://github.com/linrunner/TLP/issues/454>`_):

.. code-block:: none

    /sys/class/power_supply/BAT0/charge_start_threshold = 45 [%]
    /sys/class/power_supply/BAT0/charge_stop_threshold = 60 [%]

Yet they do not work.

Cause: bug in Lenovo's EC firmware.

Workaround:

* Update BIOS (contains EC firmware) to 1.16 or higher
* Remove thresholds once from EC with :command:`tlp fullcharge`
* Leave the thresholds enabled in the config file
* Reboot, which will restore the configured thresholds

ThinkPad T430(s)/T530/W530/X230 (and all later models)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Solution: install version 1.5 or later and a kernel ≥ 5.17 for
full `natacpi` support.

ThinkPad T420(s)/T520/W520/X220
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`tp-smapi` doesn't support start threshold and recalibration on Sandy Bridge
generation ThinkPads. Symptoms are:

:command:`tlp-stat -b` shows

.. code-block:: none

    /sys/devices/platform/smapi/BAT0/start_charge_thresh = (not available)

:command:`tlp setcharge/tlp fullcharge` show the message

.. code-block:: none

    start => Warning: cannot set threshold.

:command:`tlp discharge/recalibrate` show the message

.. code-block:: none

    Error: battery discharge/recalibrate not available.

Solution: install version 1.5 or later and a kernel ≥ 5.17 for
full `natacpi` support.


.. _faq-unsupported-thinkpads:

ThinkPad L420/520, L512, SL300/400/500, X121e
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
These models are neither supported by `tp-smapi` nor by `tpacpi-bat` or `natacpi`.
Please refrain from opening issues.


.. _faq-asus-threshold-not-set-on-boot:

ASUS laptops: stop charge threshold isn't set at boot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Symptom: Battery stop charge threshold isn't set at boot. But manually running
:command:`tlp start` or :command:`tlp setcharge` sets the threshold.

Cause: kernel module `asus_wmi` is loaded too late in the boot sequence.

Solution: add the module to the Initramfs. The procedure depends on your distribution,
for Arch Linux/Manjaro see `Issue #602 <https://github.com/linrunner/TLP/issues/602>`_.

Battery has been removed
^^^^^^^^^^^^^^^^^^^^^^^^
By removing (and re-inserting) the battery the charge thresholds may be reset to
vendor specific defaults. To restore TLP's settings use

.. code-block:: sh

    sudo tlp setcharge

(see :ref:`cmd-tlp-battery-features`) or

* Restart system
* Shutdown and power off system


.. _faq-wrong-threshold-values:

Charge thresholds shown by `tlp-stat -b` do not correspond to the configured ones
---------------------------------------------------------------------------------
Possible causes are:

.. rubric:: Configuration was not activated

After changes to the configuration it is necessary to reboot. Alternatively use

.. code-block:: sh

    sudo tlp start

or

.. code-block:: sh

    sudo tlp setcharge

to activate the thresholds.


.. _faq-elsy-threshold-values:

.. rubric:: ThinkPad E / L / S / Yoga series

Also affected: ThinkPad Edge series, 11, SL410/510

On these models the threshold values shown by :command:`tlp-stat -b` do not
correspond to the written values. For example the settings ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

may show up as ::

    /sys/class/power_supply/BAT0/charge_start_threshold         =     75 [%]
    /sys/class/power_supply/BAT0/charge_stop_threshold          =     74 [%]

or similar. Common feature is that one of the two threshold values is always
displayed incorrectly.

The described behavior is caused by Lenovo's EC firmware, not by TLP.
Nonetheless the charge thresholds work as configured.


.. _faq-start-thresholds-does-not-apply:

Start threshold does not apply after change
-------------------------------------------
Affected hardware: ThinkPads X240, Yoga 12 (based on user feedback)

Workaround: activating a new start threshold may require to discharge the battery
below the old start threshold after writing the new threshold, i.e. via
:command:`tlp setcharge` or reboot
(see `Issue #173 <https://github.com/linrunner/TLP/issues/183#issuecomment-175228097>`_).

Do charge thresholds work even when TLP is not running or the laptop is powered off?
------------------------------------------------------------------------------------
Yes. The charging process is not controlled by software running on behalf of the
operating system but by the embedded controller (EC). TLP only passes the
threshold values to the EC firmware using the appropriate driver.
Once stored in the EC, the thresholds also take effect when the laptop is switched
off. See below for removal.

A general explanation of charge thresholds is given :ref:`above <faq-battery-care>`.


.. _faq-start-threshold:

What exactly does the start charge threshold `START_CHARGE_THRESH_BATx` do?
---------------------------------------------------------------------------
The start charge threshold ensures that the battery is not recharged immediately
after every short discharge process. The charging process starts only when the
previous discharge was below the value of `START_CHARGE_THRESH_BATx`.

A general explanation of charge thresholds is given :ref:`above <faq-battery-care>`.


.. _faq-how-to-disable-thresholds:

How to disable the charge thresholds?
-------------------------------------
As explained :ref:`above <faq-battery-care>`, in many cases the charge
thresholds are persistently stored in the hardware.
Therefore, it is not sufficient to simply remove the thresholds from
the configuration (by the way, disabling or uninstalling TLP is not
enough either). In fact, two steps are required to disable the thresholds:

1. Removing the charge thresholds from the configuration by inserting a leading `#`

.. code-block:: none

    #START_CHARGE_THRESH_BAT0=75
    #STOP_CHARGE_THRESH_BAT0=80

2. Reverting to vendor specific defaults with the command

.. code-block:: sh

    sudo tlp fullcharge


.. _faq-disabling-thresholds-does-not-work:

Disabling the charge thresholds does not work
---------------------------------------------
Affected hardware: ThinkPad E580, T480s, X1 Carbon 6th Gen (and possibly others)

Symptom: after resetting the thresholds as described above, :command:`tlp-stat -b`
shows the stop threshold unchanged.

Cause: after applying a stop threshold value < 100, Lenovo's EC firmware does not
accept values higher than the previously set value.

Solution: update EC firmware (contained in BIOS update)

* T480s: ECP 1.13 (BIOS 1.31) or higher
* X1 Carbon 6th Gen: ECP 1.12 (BIOS 1.37) or higher

Workaround (without BIOS update):

* Disable the threshold configuration as decribed above
* Power off the laptop via shutdown
* Unplug AC power
* Power on the laptop
* At the Lenovo logo: press :kbd:`Enter`, :kbd:`F1` to enter the BIOS setup
* Go to: `Config → Power → Disable Built-in Battery`, :kbd:`Enter`, :kbd:`Y`
  laptop will power off
* Connect AC
* Power on laptop
* Check with :command:`tlp-stat -b` – thresholds should be at factory settings
  96/100% now
* When unsuccessful, repeat the whole procedure


.. _faq-thinkpad-battery-malfunc:

Various ThinkPad battery malfunctions
-------------------------------------
Affected hardware: ThinkPad T480, X280, X1 Carbon 4th Gen (and possibly others)

.. note::

    TLP does not cause the symptoms described in this section.

At times, ThinkPads may exhibit symptoms that suggest a battery or hardware defect.
Users have reported the following:

* ThinkPad switches off as soon as the internal battery BAT0 is empty,
  even if the external battery BAT1 is still charged.
  Reference: `Arch Linux forum <https://bbs.archlinux.org/viewtopic.php?id=285210>`_.
* ThinkPad unexpectedly switches off when the external battery BAT1 is removed,
  even if the internal battery BAT0 is still charged.
  Reference: `Issue #690 <https://github.com/linrunner/TLP/issues/690>`_.
* Battery is not charging even though the charger is connected and no
  charge threshold is active.
* :command:`tlp recalibrate` fails with "Error: discharge BATx malfunction --
  check your hardware (battery, charger)".

However, in the mentioned cases, there was no battery or hardware defect,
but rather a malfunction of the EC firmware, which controls all battery
functions independently of the operating system.

Solutions:

* Reset the EC: shut down the system, then press the emergency reset hole (button)
  on the bottom of the ThinkPad with a paper clip
* Update to the latest EC firmware (most elegantly with `fwupdmgr`)

.. seealso::

    :ref:`faq-disabling-thresholds-does-not-work`


.. _faq-erratic-battery-behavior:

Erratic battery behavior on ThinkPad T420(s)/T520/W520/X220 (and possibly later models)
---------------------------------------------------------------------------------------
Symptom: some users report severely reduced battery capacity or sudden drops of
the charge level from around 30% to zero when employing charge thresholds.

Probable cause: conflict with dualmode battery firmware (refer to
:ref:`faq-good-battery-thresholds`).

Solution: remove battery thresholds completely or use only the start threshold
by setting the stop threshold to 100%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=100

Then recalibrate the battery once.

.. note::

    This is a software only issue, no harm is done to the battery.

How to designate the battery to discharge when battery powered?
---------------------------------------------------------------
On dual battery ThinkPads, the embedded controller (EC) alone determines
the order in which the batteries are used (discharged) and charged, as well
as the switching point. The Lenovo EC firmware does not provide any way
to change this behavior (except indirectly through charge thresholds and
forced discharge, of course). Also, the behavior may vary from model to model.

The above means that selecting the active battery for ThinkPads - also
called "battery balancing" - is impossible because the EC firmware provides
no way to do this, either manually or automatically.


.. _faq-charging-stops-at-80:

`tlp fullcharge BAT1` stops at ~80%
------------------------------------
Affected hardware: ThinkPad T440s (based on user feedback)

Symptom: although the stop threshold is set to 100% either by configuration
or by :command:`tlp fullcharge/setcharge`, the charge of BAT1 stops at about
80%. When BAT1 reaches 80%, BAT0 starts charging to 100%, then BAT1 continues
to 100%. If a stop threshold is set for BAT0, the last step may never happen.

No solution: Lenovo's EC firmware offers no possibility to change the behaviour
(see above).


.. _faq-discharging-misconception:

Why does the battery not begin to discharge when the stop threshold is reached?
-------------------------------------------------------------------------------
*Author's note: sometimes users get into this misunderstanding without me
understanding how it happens. This is an attempt to get them out.*

The purpose of the stop threshold is to reduce battery wear by limiting the
charge level below 100%. So charging stops at the threshold and the battery
is not discharged as long as the charger remains connected.
This is the manufacturer's intended behavior. It cannot (and should not) be
changed, because repeatedly discharging the battery while operating on AC power
would lead to absurdly high wear (i.e. charging cycles) without any benefit.

A general explanation of charge thresholds is given :ref:`above <faq-battery-care>`.

`tlp recalibrate` terminates with an error message
--------------------------------------------------
*ThinkPads only*

Impact: Recalibration will not work without a full discharge. This is to tell
the battery controller (the one *in* the battery) where the actual 0% is.

Symptom 1: ::

    Warning: battery BAT0 was not discharged completely -- AC/charger removed.

Solution: first make sure AC power is connected during the whole process then
try a different charger.

Symptom 2: ::

    Error: battery BAT0 was not discharged completely i.e. terminated by the firmware -- check your hardware (battery, charger).

Cause: this is a hardware issue either with your battery (likely), charger or laptop.

Solution: first try another battery pack then a different charger. If this does not
remedy the situation, a system board defect could be the reason.

.. note::

    * If the discharge process regularly stops at 1%, for example, you may prefer
      to ignore the problem because it is minor (could be a rounding error).
      With higher values, however, it could be a battery or hardware defect.
    * Values ≤ 1% will not trigger an error message in version 1.7 (yet unreleased).

.. _faq-panel-applet-soc:

Why does the panel applet show the battery state "charging" despite charge thresholds are effective?
----------------------------------------------------------------------------------------------------
*ThinkPads only*

Existing panel applets query `upowerd` or the standard kernel interface which do
not reflect the charging condition correctly as soon as charge thresholds intervene.

In this situation :command:`tlp-stat -b` shows

* "Idle" - *Version 1.4 and later*
* "Unknown (threshold effective)" - *Version 1.3*

for **/sys/class/power_supply/BATx/status**.

For ThinkPad models supporting `tp-smapi`, the correct state "Idle" is shown
for **/sys/devices/platform/smapi/BATx/state**.


.. _faq-cycle-count:

Why does `tlp-stat -b` display "cycle_count = (not supported)"?
---------------------------------------------------------------
Cycle count is not available for all laptops. Positive exceptions are older
ThinkPads supporting `tp-smapi` and certain newer hardware.

.. note::

    For new batteries with zero cycles, :command:`tlp-stat -b` displays
    `(not supported)` too.  This is because the kernel output does not distinguish
    zero cycles from unsupported, it is `0` in both cases. With supported hardware
    the display works again as soon as the battery has at least one cycle.
