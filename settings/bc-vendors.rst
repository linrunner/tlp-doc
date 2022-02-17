Battery Care Vendor Specifics
-----------------------------
The level of Battery Care support depends on laptop vendor or brand, Linux
kernel version and TLP version. The following explains:

    * Which laptops are supported
    * Which kernel drivers are necessary for them
    * What charge control options are offered
    * Which values for the charge thresholds are supported by the hardware (and by TLP)

Prerequisites
^^^^^^^^^^^^^
In order to use charge control thresholds or recalibration with TLP:

    1. The hardware must have the appropriate capability
    2. A suitable kernel driver must make these capabilities usable under Linux

.. note::

    If the hardware does not have the capability or no suitable driver exists,
    then TLP Battery Care cannot use it. Please do not submit issues for hardware
    lacking kernel driver support for charge control options. Providing kernel
    drivers is not part of the TLP project.

As for obtaining kernel drivers, there are two options:

    a. Drivers that are maintained in the Linux mainline kernel and shipped with
       each distribution. They are loaded automatically on supported laptops
       and do not require any user action.
    b. So-called "external kernel drivers", which are not part of the Linux
       mainline kernel and require the user to install additional, distribution
       specific packages. This case applies to ThinkPads only and is described
       in :doc:`/installation/index`.

Supported Hardware
^^^^^^^^^^^^^^^^^^

ASUS
""""
.. list-table::
   :widths: 250 1000
   :align: center

   * - **Hardware**
     - ASUS laptops
   * - **Kernel driver**
     - `asus_wmi` - required, included in distribution kernels (5.4 or higher)
   * - **TLP version**
     - 1.4 and higher
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
   * - **Notes**
     - Some ASUS laptops silently ignore stop threshold values other than
       40, 60 or 80. Please check if your configuration works as expected.

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
   :align: center

   * - **Hardware**
     - Huawei MateBooks
   * - **Kernel driver**
     - `huawei_wmi` - required, included in distribution kernels (5.4 or higher)
   * - **TLP version**
     - 1.4 and higher
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


Lenovo ThinkPads
""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: center

   * - **Hardware**
     - Lenovo ThinkPad series as of model year 2011 - e.g. T420(s)/T520/W520/X220 and newer
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
     - 0 (off) .. 96 (default) .. 99
   * - **Stop threshold values**
     - 1 .. 100 (default)
   * - **See also**
     - | - :ref:`faq-which-kernel-module`
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


Lenovo/IBM legacy ThinkPads
"""""""""""""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: center

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
     - 2 .. 96 (default)
   * - **Stop threshold values**
     - 6 .. 100 (default)
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
   :align: center

   * - **Hardware**
     - Lenovo laptops (all non-ThinkPad series)
   * - **Kernel driver**
     - `ideapad_laptop` - required, included in distribution kernels (4.14 or higher)
   * - **TLP version**
     - 1.4 and higher
   * - **TLP plugin**
     - lenovo
   * - **Charge control options**
     - Fixed stop charge threshold at 60% aka *Battery Conservation Mode*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `START/STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 1 - batteries charge to 60%
       | 0 - batteries charges to 100% (battery conservation mode off, )

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
   :align: center

   * - **Hardware**
     - LG Gram laptops
   * - **Kernel driver**
     - `lg_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.4 and higher
   * - **TLP plugin**
     - lg
   * - **Charge control options**
     - Fixed stop charge threshold at 80% aka *Battery Care Limit*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1`, `CMB0`, `CMB1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 80 - batteries charge to 80%
       | 100 - batteries charge to 100% (battery care limit off)

.. rubric:: Sample configuration

Stop charging battery `BAT0`, `BAT1`, `CMB0` and `CMB1` at 80%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=1

Stop charging battery `BAT0`, `BAT1`, `CMB0` and `CMB1` at 100%: ::

    START_CHARGE_THRESH_BAT0=0  # dummy value
    STOP_CHARGE_THRESH_BAT0=0

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: lg
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
   :align: center

   * - **Hardware**
     - Samsung laptops
   * - **Kernel driver**
     - `samsung_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.4 and higher
   * - **TLP plugin**
     - samsung
   * - **Charge control options**
     - Fixed stop charge threshold at 80% aka *battery life extender*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 1 - batteries charge to 80%
       | 0 - batteries charge to 100% (battery life extender off)

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
   :align: center

   * - **Hardware**
     - Sony VAIO laptops
   * - **Kernel driver**
     - `sony_laptop` - required, included in distribution kernels
   * - **TLP version**
     - 1.5 and higher
   * - **TLP plugin**
     - sony
   * - **Charge control options**
     - Stop threshold at 50, 80 or 100% aka *battery care limiter*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 50 - batteries charge to 50%
       | 80 - batteries charge to 80%
       | 100 - batteries charge to 100% (battery care limiter off)

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


Unsupported Hardware
^^^^^^^^^^^^^^^^^^^^
For all laptop vendors/brands without charge control options the output of
:command:`tlp-stat -b` looks like this:

.. code-block:: none

    +++ Battery Care
    Plugin: generic
    Supported features: none available


Final Notes
^^^^^^^^^^^
*Applies to version 1.4 and higher only*

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
