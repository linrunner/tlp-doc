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

The process is as follows:

.. include:: ../include/charge-threshold-effect.rst

You cannot change the basic behavior described above, because it is hard-coded
into the EC firmware by the vendor. However, you can control it by setting
thresholds using TLP.

Once charge thresholds have been loaded into the EC registers, they are retained
when another operating system is started - provided they are not changed by
software installed there and the hardware supports this.

.. rubric::  What charge thresholds do `not` do

* They do not exercise any control over the discharging of the battery,
  this depends solely on whether AC is connected and if the laptop is switched on
* In particular, with AC connected, a battery with a charge level higher than
  the stop charge threshold will not be discharged to the stop charge threshold,
  nor will there be a (cyclic) discharge down to the start charge threshold

.. seealso::

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

Source: `Lenovo Forums [2] <https://forums.lenovo.com/t5/Welcome-FAQs-Knowledge-Base/How-can-I-increase-battery-life-ThinkPad/ta-p/244800>`_

Default TLP settings (only if you uncomment the relevant lines) are slightly more
protective regarding lifespan, with 75/80% charge thresholds.

.. note::

    Please always consider that the start threshold is the critical constraint
    for runtime, because it defines the lowest charge level that can occur while
    plugged. Remember that TLP provides a command (:command:`tlp fullcharge`)
    to fully charge the battery, when you need to temporarily maximize runtime
    (for example in case of a trip).

How can I check if my configured charge thresholds are working?
---------------------------------------------------------------
The output of :command:`tlp-stat -b` shows two characteristics for the positive
case:

1. An active battery care driver for the charge thresholds
2. The actual charge thresholds – read back from the embedded controller

So if there is a line containing `active (charge thresholds)` and the displayed
thresholds match the ones you configured, then the charging logic has properly
received them.

Two examples for different ThinkPad generations:

.. code-block:: none

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds)
    * tpacpi-bat (acpi_call)  = active (recalibration)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]
    tpacpi-bat.BAT0.forceDischarge                              =      0

.. code-block:: none

    +++ Battery Care
    Plugin: thinkpad-legacy
    Supported features: charge thresholds, recalibration
    Driver usage:
    * tp-smapi (tp_smapi) = active (status, charge thresholds, recalibration)
    ...
    /sys/devices/platform/smapi/BAT0/start_charge_thresh        =     75 [%]
    /sys/devices/platform/smapi/BAT0/stop_charge_thresh         =     80 [%]

If the output does not contain the required characteristics, check the following
sections for solutions.

However, if despite a correctly set up system the charging thresholds do not
work as you expect them to, then you should first compare your idea of the
charging process with the description in :doc:`/settings/battery` and subsequently
check the sections further down for possible explanations.

.. note::

    On some models the displayed threshold values do not correspond to
    the configured ones, although they work as they should
    – refer to :ref:`faq-elsy-threshold-values`.


.. _faq-which-kernel-module:

Which external kernel module do I need for my ThinkPad?
-------------------------------------------------------
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
    Install acpi_call kernel module for ThinkPad battery thresholds and recalibration
    Install acpi_call kernel module for ThinkPad battery recalibration

and install the required external kernel module package as explained in
:doc:`/installation/index` for your distribution.

Almost all ThinkPad models need only one of the above kernel modules. You may
check the output of :command:`tlp-stat -b` for lines like:

.. code-block:: none

    tpacpi-bat = inactive (ThinkPad not supported)
    tp-smapi = inactive (ThinkPad not supported)

and remove the unnecessary module package (`tpacpi-bat` means `acpi_call`).
However it doesn't really hurt to keep both.

.. rubric:: natacpi – Ultimate solution at the horizon

Starting with kernel 4.17 `tpacpi-bat` gets superseded by a new, native kernel
API called `natacpi` (contained in the ubiquitious kernel module `thinkpad_acpi`)
which supports charge thresholds so far. :command:`tlp-stat -b` indicates this
as follows:

*Versions 1.2.2 through 1.3.1*

.. code-block:: none

    +++ Battery Features
    natacpi = active (data, thresholds)
    tpacpi-bat = active (recalibrate)
    tp-smapi = inactive (ThinkPad not supported)
    ...
    /sys/class/power_supply/BAT0/charge_start_threshold         =     96 [%]
    /sys/class/power_supply/BAT0/charge_stop_threshold          =    100 [%]
    tpacpi-bat.BAT0.forceDischarge                              =      0

*Version 1.4*

.. code-block:: none

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

*Version 1.5 or higher with kernel 5.17 or higher*

Kernel 5.17 adds recalibration and completes `natacpi`. `tpacpi-bat` and the
external kernel module `acpi_call` together with the distribution-specific
packages become superfluous. :command:`tlp-stat -b` now looks like this:

.. code-block:: none

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds, recalibration)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     96 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =    100 [%]
    /sys/class/power_supply/BAT0/charge_behaviour               = [auto] inhibit-charge force-discharge



.. seealso::

    * :doc:`/settings/bc-vendors` - Details on hardware support
    * `tpacpi-bat <https://github.com/teleshoes/tpacpi-bat>`_
      – Source code for the tool that is included in TLP to provide battery recalibration
      for ThinkPads since model year 2011 - e.g. T420/X220 and newer
    * `acpi_call <https://github.com/nix-community/acpi_call>`_
      – Source code of the external kernel module required by `tpacpi-bat`
    * `tp-smapi <https://www.thinkwiki.org/wiki/Tp_smapi>`_
      – Documentation for the external kernel modules required for ThinkPads
      until model year 2011

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

Symptoms: ::

    --- TLP 1.5.0 --------------------------------------------

    +++ Battery Care
    Plugin: generic
    Supported features: none available

or ::

    --- TLP 1.3.1 --------------------------------------------

    +++ Battery Features: Charge Thresholds and Recalibrate
    natacpi    = inactive (laptop not supported)
    tpacpi-bat = inactive (laptop not supported)
    tp-smapi   = inactive (laptop not supported)

Load attempt with :command:`sudo modprobe thinkpad_acpi` reveals ::

    modprobe: ERROR: could not insert 'thinkpad_acpi': Invalid argument

Cause: your system configuration contains broken boot options for `thinkpad_acpi`.

Solution: check and correct your GRUB config (distribution dependent)
or module config files (**/etc/modprobe.d/*.conf**).

External kernel module is not installed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptom: :command:`tlp-stat -b` shows

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' not installed)

or

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' not installed)

Solution: read :ref:`faq-which-kernel-module` and install the necessary packages.
If :command:`tlp-stat -b` still claims 'not installed' after installing
the appropriate package, reinstall the package via shell command and check
the output for errors. See below for possible causes.

Fedora release upgrade
^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

It may be necessary to rebuild the kernel modules (as root): ::

    akmods --force

.. _faq-acpi-call-dkms-package:

Installation of package `acpi-call-dkms` failed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

.. important::

    `acpi_call` and derived packages are not provided by the TLP project.
    Please don't file bug reports for them in the TLP issue tracker.

Symptoms: :command:`tlp-stat -b` shows

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' not installed)

Package install shows

.. code-block:: none

    Setting up acpi-call-dkms ...
    Error! Bad return status for module build on kernel: ...

Solution: upgrade the package:

.. rubric:: Debian and Ubuntu derivatives

* Kernel ≥ 5.13 needs at least package version 1.2.2-1
  (Debian Sid/Bookworm or Ubuntu 22.04 or TLP PPA)
* Kernel ≥ 5.6 needs at least package version 1.1.0-5ubuntu0.1 (Ubuntu 20.04)
  or 1.1.0-6 (Debian Bullseye/Buster Backports, Ubuntu 21.10)

Kernel module `acpi-call` is not loaded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptom: :command:`tlp-stat -b` shows

.. code-block:: none

    tpacpi-bat = inactive (kernel module 'acpi_call' load error)

Solution: try to load manually with

.. code-block:: sh

    sudo modprobe -v acpi_call

and use adequate forums to resolve your issue with `acpi-call`.

.. note::

    You may need to disable Secure Boot when `acpi-call` refuses to load.
    Check your distribution's :doc:`/installation/index` instructions.

Installation of package `tp-smapi-dkms` failed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

.. important::

    `tp-smapi` and derived packages are not provided by the TLP project.
    Please don't file bug reports for them in the TLP issue tracker.

Symptoms (Ubuntu): :command:`tlp-stat -b` shows

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' not installed)

Package install shows

.. code-block:: none

    Setting up tp-smapi-dkms ...
    Error! Your kernel headers for kernel X.Y.0-NN-generic cannot be found.
    Please install the linux-headers-X.Y.0-NN-generic package

Solution: install package **linux-generic-headers**.

Symptoms: :command:`tlp-stat -b` shows

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' not installed)

Package install shows

.. code-block:: none

    Setting up tp-smapi-dkms ...
    Error! Bad return status for module build on kernel: ...

Solution: upgrade the package:

Kernel module `tp-smapi` is not loaded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*ThinkPads only*

Symptom: :command:`tlp-stat -b` shows

.. code-block:: none

    tp-smapi = inactive (kernel module 'tp_smapi' load error)

Solution: try to load manually with

.. code-block:: sh

    sudo modprobe -v tp_smapi

and check `tp-smapi Troubleshooting <http://www.thinkwiki.org/wiki/Tp_smapi#Troubleshooting>`_
for a solution matching the error message or use adequate forums to resolve your
issue with `tp-smapi`.

.. note::

    * You may need to disable Secure Boot when `tp-smapi` refuses to load,
      check your distribution's :doc:`/installation/index` instructions
    * `Libreboot` does not support `tp-smapi`
    * `tp-smapi` does not support newer models, check :ref:`faq-which-kernel-module`

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

    Error: discharge function not available for this laptop.

Solutions:

* Install a kernel ≥ 4.19 to make natacpi available
* TLP automatically uses `tpacpi-bat` when the kernel module `acpi_call` is
  available, see :ref:`faq-which-kernel-module`

ThinkPad T430(s)/T530/W530/X230 (and all later models)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Solutions:

* Install a kernel ≥ 4.19 to make natacpi available
* TLP automatically uses `tpacpi-bat` when the kernel module `acpi_call` is
  available, see :ref:`faq-which-kernel-module`

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

.. _faq-unsupported-thinkpads:

ThinkPad L420/520, L512, SL300/400/500, X121e
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
These models are neither supported by `tp-smapi` nor by `tpacpi-bat` or `natacpi`.
Please refrain from opening issues.

Battery has been removed
^^^^^^^^^^^^^^^^^^^^^^^^
By removing (and re-inserting) the battery the charge thresholds may be reset to
vendor specific defauls. To restore TLP's settings use

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

.. rubric:: ThinkPad 11, Edge, E / L / S series, SL410/510, Yoga series

On these models the threshold values shown by :command:`tlp-stat -b` do not
correspond to the written values. For example the settings ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

may show up as ::

    /sys/class/power_supply/BAT0/charge_start_threshold         =     75 [%]
    /sys/class/power_supply/BAT0/charge_stop_threshold          =     74 [%]

The described behavior is caused by the EC firmware, not by TLP.
Nonetheless the charge thresholds work as configured.

.. _faq-start-thresholds-does-not-apply:

Start threshold does not apply after change
-------------------------------------------
Affected hardware: ThinkPads X240, Yoga 12 (based on user feedback)

Workaround: activating a new start threshold may require to discharge the battery
below the old start threshold after writing the new threshold, i.e. via
:command:`tlp setcharge` or reboot
(see `Issue #173 <https://github.com/linrunner/TLP/issues/183#issuecomment-175228097>`_).

.. _faq-charging-stops-at-80:

`tlp fullcharge BAT1` stops at ~80%
------------------------------------
Affected hardware: ThinkPad T440s (based on user feedback)

Symptom: despite the stop threshold is set to 100% either by configuration or by
:command:`tlp fullcharge/setcharge`, charging of BAT1 stops at around 80%.

Cause: this is hard-coded into Lenovo's EC firmware.
After BAT1 reaches 80% charging commences with BAT0 until 100%, afterwards BAT1
continues until 100%. If a stop threshold is set for BAT0, the last step may never
happen.

No solution: Lenovo's EC firmware offers no possibility to change the behaviour.

.. _faq-erratic-battery-behavior:

Erratic battery behavior on ThinkPad T420(s)/T520/W520/X220 (and all later models)
----------------------------------------------------------------------------------
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

.. _faq-panel-applet-soc:

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

How to designate the battery to discharge when battery powered?
---------------------------------------------------------------
ThinkPad battery balancing i.e. selecting the active battery for
models with more than one battery is not possible because the Lenovo
EC firmware offers no way to do this, neither manual nor automatic.

.. _faq-discharging-misconception:

Why does the battery not begin to discharge when the stop threshold is reached?
-------------------------------------------------------------------------------
*Author's remark: sometimes users trap into this misunderstanding without me having
understood how it happens. This is the attempt to lead them out again.*

The task of the stop threshold is to reduce battery wear by limiting the charge
level below 100%. So charging stops at the threshold and the battery will not be
discharged as long as the charger remains connected.

This is the behaviour designated by the vendor. It cannot (and should not)
be changed, because repeated discharge of the battery during operation on AC power
would lead to absurdly high wear (i.e. charging cycles) without any benefit being
derived from it.

A general explanation of charge thresholds is given :ref:`above <faq-battery-care>`.


.. _faq-how-to-disable-thresholds:

How do I disable the charge thresholds?
---------------------------------------
Remove the charge thresholds from the configuration by inserting a leading `#`

.. code-block:: none

    #START_CHARGE_THRESH_BAT0=75
    #STOP_CHARGE_THRESH_BAT0=80

and use

.. code-block:: sh

    sudo tlp fullcharge

to immediately activate vendor specific defaults.

.. _faq-disabling-thresholds-does-not-work:

Disabling the charge thresholds does not work
----------------------------------------------
Affected hardware: ThinkPads E580, T480s, X1 Carbon 6th (based on user feedback)

Symptom: after resetting the thresholds as described above, :command:`tlp-stat -b`
shows the stop threshold unchanged.

Cause: after applying a stop threshold value < 100, Lenovo's EC firmware does not
accept values higher than the previously set value.

Solution: update EC firmware (contained in BIOS update)

* T480s: ECP 1.13 (BIOS 1.31) or higher
* X1 Carbon 6th: ECP 1.12 (BIOS 1.37) or higher

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

My battery does not charge anymore after recalibration showing X% remaining capacity constantly
-----------------------------------------------------------------------------------------------
*ThinkPads only*

Most probable cause: battery is defect – and was it even before the recalibration
attempt.

`tlp recalibrate` terminates with an error message
--------------------------------------------------
*ThinkPads only*

Impact: recalibration does not work at all without a full discharge to tell the
battery controller (the one *in* the battery) where the actual 0% is.

Symptom 1: ::

    Warning: battery BAT0 was not discharged completely -- AC/charger removed.

Solution: first make sure AC power is connected during the whole process then
try a different charger.

Symptom 2: ::

    Error: battery BAT0 was not discharged completely i.e. terminated by the firmware -- check your hardware.

Cause: this is a hardware issue either with your battery (likely), charger or laptop.

Solution: first try another battery pack then a different charger. If this does not
remedy the situation, a system board defect could be the reason.

.. note::

    If the discharge process regularly stops at 1%, for example, you may prefer
    to ignore the problem because it is minor. Nevertheless, a battery or
    hardware defect could be present in this case.


.. _faq-cycle-count:

Why does the panel applet show the battery state "charging" despite charge thresholds are effective?
----------------------------------------------------------------------------------------------------
*ThinkPads only*

Existing panel applets query `upowerd` or the standard kernel interface which do
not reflect the charging condition correctly as soon as charge thresholds intervene.

In this situation :command:`tlp-stat -b` shows

* "Idle" - *Version 1.4 and higher*
* "Unknown (threshold effective)" - *Version 1.3.1 and lower*

for **/sys/class/power_supply/BATx/status**.

For ThinkPad models supporting `tp-smapi`, the correct state "Idle" is shown
for **/sys/devices/platform/smapi/BATx/state**.

Why does `tlp-stat -b` display "cycle_count = (not supported)"?
---------------------------------------------------------------
Cycle count is not available for all laptops. Positive exceptions are older
ThinkPads supporting `tp-smapi` and certain newer hardware.

.. note::

    For new batteries with zero cycles, :command:`tlp-stat -b` displays
    `(not supported)` too.  This is because the kernel output does not distinguish
    zero cycles from unsupported, it is `0` in both cases. With supported hardware
    the display works again as soon as the battery has at least one cycle.
