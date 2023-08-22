Battery Care Vendor Specifics
-----------------------------
The level of battery care support depends on laptop vendor or brand, Linux
kernel version and TLP version. The following explains:

    * Which laptops are supported
    * Which kernel drivers are necessary for them
    * What charge control options are offered
    * Which values for the charge thresholds are supported by the hardware (and by TLP)

Prerequisites
^^^^^^^^^^^^^
In order to use charge thresholds or recalibration with TLP:

    1. The hardware must have the appropriate capability
    2. An existing kernel driver matching the hardware must provide these
       capabilities under Linux

As for obtaining kernel drivers, there are two options:

    a. Drivers that are maintained in the Linux kernel and shipped with each
       distribution. They are loaded automatically on supported laptops and do
       not require any user action.
    b. So-called "external kernel drivers", which are not part of the Linux
       kernel and require the user to install additional, distribution specific
       packages. This case applies to ThinkPads only and is described in
       :doc:`/installation/index`.

.. note::

    If the hardware does not have the capability or no suitable driver exists,
    then TLP battery care cannot control it. Please do not submit issues for hardware
    lacking kernel driver support for charge control options. Providing kernel
    drivers is not part of the TLP project.


Supported Hardware
^^^^^^^^^^^^^^^^^^

ASUS
""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - ASUS laptops
   * - **Kernel driver**
     - `asus_wmi` - required, included in distribution kernels (5.4 or newer)
   * - **TLP version**
     - 1.4 and newer
   * - **TLP plugin**
     - asus
   * - **Charge control options**
     - Stop charge threshold
   * - **Threshold configuration**
     - | Batteries `BAT0`, `BATC` and `BATT` share the `STOP_CHARGE_THRESH_BAT0` parameter
       | Battery `BAT1` uses the `STOP_CHARGE_THRESH_BAT1` parameter
   * - **Stop threshold values**
     - | Range: 0 .. 100
       | Special:
       | 0 - threshold off
       | 100 - hardware default
   * - **Specifics**
     - | Some ASUS laptops silently ignore stop threshold values other than 40, 60 or 80.
         Please check if your configuration works as expected.
       | When resuming from suspend TLP restores the threshold.
       | When powering on, ASUS laptops reset the charge thresholds. TLP
         restores them on boot, but due to the hardware's behaviour, there
         is a short time window where the thresholds do not take effect.
   * - **See also**
     - | :ref:`faq-asus-threshold-not-set-on-boot`


.. rubric:: Sample configuration

Stop charging battery `BAT0`, `BATC` and `BATT` at 80%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: asus
    Supported features: charge threshold
    Driver usage:
    * natacpi (asus_wmi) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0/1: 0(off)..100(default)
    [...]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]


Huawei
""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Huawei MateBooks
   * - **Kernel driver**
     - `huawei_wmi` - required, included in distribution kernels (5.4 or newer)
   * - **TLP version**
     - 1.4 and newer
   * - **TLP plugin**
     - huawei
   * - **Charge control options**
     - Start and stop charge threshold
   * - **Threshold configuration**
     - Batteries `BAT0`, `BAT1` share the `START/STOP_CHARGE_THRESH_BAT0` parameters
   * - **Start threshold values**
     - | Range: 0 .. 99
       | Special:
       | 0 - hardware default, threshold off
   * - **Stop threshold values**
     - | Range: 1 .. 100
       | Special:
       | 100 - hardware default
   * - **Specifics**
     - | When resuming from suspend TLP restores the threshold


.. rubric:: Sample configuration

Start charging battery `BAT0` and `BAT1` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    ++ Battery Care
    Plugin: huawei
    Supported features: charge thresholds
    Driver usage:
    * vendor (huawei_wmi) = active (charge thresholds)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0:  0(default)..99
    * STOP_CHARGE_THRESH_BAT0:   1..100(default)

    /sys/devices/platform/huawei-wmi/charge_control_thresholds  = 75 80

.. _bc-vendor-thinkpad:

Lenovo ThinkPads
""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Lenovo ThinkPad series since model year 2011 - e.g. T420(s)/T520/W520/X220
   * - **Kernel drivers**
     -  | `thinkpad_acpi` - required, included in distribution kernels
        | `acpi_call` - optional, enables recalibration for kernels before 5.17;
          distribution specific package needed
   * - **TLP version**
     - all
   * - **TLP plugin**
     - thinkpad
   * - **Charge control options**
     - Start and stop charge threshold, recalibration
   * - **Threshold configuration**
     - | Main/internal battery `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
       | Auxiliary/UltraBay battery `BAT1` uses the `START/STOP_CHARGE_THRESH_BAT1` parameters
   * - **Start threshold values**
     - | Range: 0 .. 99
       | Special:
       | 0 - threshold off
       | 96 - hardware default
   * - **Stop threshold values**
     - | Range: 1 .. 100
       | Special:
       | 100 - hardware default, threshold off
   * - **See also**
     - | - :ref:`faq-which-kernel-module`
       | - :ref:`faq-thinkad-misc-battery-malfunc`
       | - :ref:`Erratic Battery Behaviour <faq-erratic-battery-behavior>`


.. rubric:: Sample configuration

Start charging battery `BAT0` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: thinkpad
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (thinkpad_acpi) = active (charge thresholds, recalibration)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1:  0(off)..96(default)..99
    * STOP_CHARGE_THRESH_BAT0/1:   1..100(default)
    [...]
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]
    /sys/class/power_supply/BAT0/charge_behaviour               = [auto] inhibit-charge force-discharge

.. _bc-vendor-thinkpad-legacy:

Lenovo/IBM legacy ThinkPads
"""""""""""""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Lenovo or IBM ThinkPad series before model year 2011
   * - **Kernel drivers**
     - | `thinkpad_acpi` - required, included in distribution kernels
       | `tp_smapi` - required, enables charge thresholds and recalibration;
         distribution specific package needed
   * - **TLP version**
     - all
   * - **TLP plugin**
     - thinkpad-legacy
   * - **Charge control options**
     - start and stop charge threshold, recalibration
   * - **Threshold configuration**
     - | Main/internal battery `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
       | Auxiliary/UltraBay battery `BAT1` uses the `START/STOP_CHARGE_THRESH_BAT1` parameters
   * - **Start threshold values**
     - | Range: 2 .. 96
       | Special:
       | 96 - hardware default
   * - **Stop threshold values**
     - | Range: 6 .. 100
       | Special:
       | 100 - hardware default, threshold off
   * - **See also**
     - :ref:`faq-which-kernel-module`


.. rubric:: Sample configuration

Start charging battery `BAT0` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: thinkpad-legacy
    Supported features: charge thresholds, recalibration
    Driver usage:
    * tp-smapi (tp_smapi) = active (status, charge thresholds, recalibration)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1:  2..96(default)
    * STOP_CHARGE_THRESH_BAT0/1:   6..100(default)
    [...]
    /sys/devices/platform/smapi/BAT0/start_charge_thresh        =     75 [%]
    /sys/devices/platform/smapi/BAT0/stop_charge_thresh         =     80 [%]
    /sys/devices/platform/smapi/BAT0/force_discharge            =      0


Lenovo non-ThinkPad series
""""""""""""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Lenovo laptops (all non-ThinkPad series)
   * - **Kernel driver**
     - `ideapad_laptop` - required, included in distribution kernels (4.14 or newer)
   * - **TLP version**
     - 1.4 and newer
   * - **TLP plugin**
     - lenovo
   * - **Charge control options**
     - Fixed stop charge threshold at 60% aka *battery conservation mode*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `START/STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 1 - batteries charge to 60%
       | 0 - batteries charges to 100%, battery conservation mode off

.. rubric:: Sample configuration

Stop charging battery `BAT0` and `BAT1` at 60%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=1

Stop charging battery `BAT0` and `BAT1` at 100%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=0

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: lenovo
    Supported features: charge threshold
    Driver usage:
    * vendor (ideapad_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 0(off), 1(on) -- battery conservation mode

    /sys/bus/platform/drivers/ideapad_acpi/VPC2004:00/conservation_mode = 1 (60%)


LG
""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - LG Gram laptops
   * - **Kernel driver**
     - `lg_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.4 and newer
   * - **TLP plugins**
     - lg, lg-legacy
   * - **Charge control options**
     - Fixed stop charge threshold at 80% aka *battery care limit*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1`, `CMB0`, `CMB1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 80 - batteries charge to 80%
       | 100 - batteries charge to 100%, battery care limit off
   * - **Specifics**
     - | 1.6 and newer:
       | - When resuming from suspend TLP restores the threshold
       | - Plugin lg/kernel 5.18 (and newer): standard sysfs attribute `charge_control_end_threshold` is used
       | - Plugin lg-legacy/older kernels: `battery_care_limit` is used

.. rubric:: Sample configuration

Stop charging battery `BAT0`, `BAT1`, `CMB0` and `CMB1` at 80%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=1

Stop charging battery `BAT0`, `BAT1`, `CMB0` and `CMB1` at 100%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=0

.. rubric:: Sample outputs of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: lg
    Supported features: charge threshold
    Driver usage:
    * natacpi (lg_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 80(on), 100(off)
    [...]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =      80 [%]

    +++ Battery Care
    Plugin: lg-legacy
    Supported features: charge threshold
    Driver usage:
    * vendor (lg_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 80(on), 100(off) -- battery care limit

    /sys/devices/platform/lg-laptop/battery_care_limit          = 80 [%]


Samsung
"""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Samsung laptops
   * - **Kernel driver**
     - `samsung_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.4 and newer
   * - **TLP plugin**
     - samsung
   * - **Charge control options**
     - Fixed stop charge threshold at 80% aka *battery life extender*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 1 - batteries charge to 80%
       | 0 - batteries charge to 100%, battery life extender off

.. rubric:: Sample configuration

Stop charging battery `BAT0` and `BAT1` at 80%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=1

Stop charging battery `BAT0` and `BAT1` at 100%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=0

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: samsung
    Supported features: charge threshold
    Driver usage:
    * vendor (samsung_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 0(off), 1(on) -- -- battery life extender

    /sys/devices/platform/samsung/battery_life_extender         = 1 (80%)


Sony
""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Sony VAIO laptops
   * - **Kernel driver**
     - `sony_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.5 and newer
   * - **TLP plugin**
     - sony
   * - **Charge control options**
     - Stop threshold at 50, 80 or 100% aka *battery care limiter*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 50 - batteries charge to 50%
       | 80 - batteries charge to 80%
       | 100 - batteries charge to 100%, battery care limiter off

.. rubric:: Sample configuration

Stop charging battery `BAT0` and `BAT1` at 80%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=80

Stop charging battery `BAT0` and `BAT1` at 100%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=100

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: sony
    Supported features: charge threshold
    Driver usage:
    * vendor (sony_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 50, 80, 100(off) -- battery care limiter

    /sys/devices/platform/sony-laptop/battery_care_limiter      =     80 [%]


System76
""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - System76 laptops - models with with **open source EC firmware** only
   * - **Kernel drivers**
     -  `system76_acpi` - required, included in distribution kernels (5.16 or newer)
   * - **TLP version**
     - 1.6 and newer
   * - **TLP plugin**
     - system76
   * - **Charge control options**
     - Start and stop charge threshold
   * - **Threshold configuration**
     - One battery only - `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
   * - **Start threshold values**
     - | Range: 0 .. 99
       | Special:
       | 0 - threshold off
       | 90 - hardware default
   * - **Stop threshold values**
     - | Range: 1 .. 100
       | Special:
       | 100 - hardware default, threshold off
   * - **Specifics**
     - | A stop threshold of 100 disables the feature.
       | A start value of 0 will always enable the charger, and charge up to the stop threshold.
       | The thresholds will be set to their defaults on EC reset (i.e. AC is unplugged when powered off).

.. rubric:: Sample configuration

Start charging battery `BAT0` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: system76
    Supported features: charge thresholds
    Driver usage:
    * natacpi (system76_acpi) = active (charge thresholds)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0:  0(off)..99
    * STOP_CHARGE_THRESH_BAT0:   1..100(default)
    [...]
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]

Toshiba
"""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Toshiba and Dynabook laptops
   * - **Kernel driver**
     - `toshiba_laptop` - required, included in distribution kernels (6.0 and newer)
   * - **TLP version**
     - 1.6 and newer
   * - **TLP plugins**
     - toshiba
   * - **Charge control options**
     - Fixed stop charge threshold at 80%
   * - **Threshold configuration**
     - | `BAT0` uses the `STOP_CHARGE_THRESH_BAT0` parameter
       | `BAT1` uses the `STOP_CHARGE_THRESH_BAT1` parameter
   * - **Stop threshold values**
     - | 80 - battery charges to 80%
       | 100 - battery charges to 100%, hardware default
   * - **Specifics**
     - The threshold is persistent (stored in NVRAM), even if the battery is removed and reinserted.

.. rubric:: Sample configuration

Stop charging battery `BAT1` at 80%: ::

    START_CHARGE_THRESH_BAT1=0  # dummy value
    STOP_CHARGE_THRESH_BAT1=80

Stop charging battery `BAT1` at 100%: ::

    START_CHARGE_THRESH_BAT1=0  # dummy value
    STOP_CHARGE_THRESH_BAT1=100

.. rubric:: Sample outputs of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: toshiba
    Supported features: charge threshold
    Driver usage:
    * natacpi (toshiba_acpi) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0/1: 80(on), 100(off)
    [...]
    /sys/class/power_supply/BAT1/charge_control_end_threshold   =     80 [%]


Unsupported Hardware
^^^^^^^^^^^^^^^^^^^^
You may encounter the case that although one of the plugins listed above
is active because the kernel driver matching the vendor/brand/model has been
detected, and yet no charge control options are available:

.. code-block:: none

    +++ Battery Care
    Plugin: <any of the above>
    Supported features: none available

Here the obstacle can be on any level - hardware capabilities or firmware
of the vendor's model in question as well as the corresponding kernel driver
- without TLP being able to determine exactly where.

For any laptop vendor/brand/model without

* hardware capabilities or
* corresponding kernel driver or
* suitable battery care plugin

the output of
:command:`tlp-stat -b` looks like this:

.. code-block:: none

    +++ Battery Care
    Plugin: generic
    Supported features: none available


Final Notes
^^^^^^^^^^^
*Applies to version 1.4 and newer only*

    * Consult the output of :command:`tlp-stat -b` for supported charge control
      options and allowed parameter values of your hardware
    * :command:`tlp setcharge` validates your configuration and reports errors
    * A value of 0 is translated to the vendor specific default (or the `disabled` state)
    * If the hardware supports only a stop charge threshold, use `START_CHARGE_THRESH_BATx=0`
    * In case the hardware supports both thresholds and you want to apply only one,
      then use `START_CHARGE_THRESH_BATx=0` or `STOP_CHARGE_THRESH_BATx=100`
      to skip the other one

.. seealso ::

    * Settings: :doc:`/settings/battery`
    * Commands: :ref:`cmd-tlp-battery-features`
    * FAQ: :doc:`/faq/battery`
