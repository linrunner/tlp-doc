Battery Care Vendor Specifics
-----------------------------
.. important::

    Battery care is not supported for any hardware that is not specifically
    listed on this page.

The level of battery care support depends on laptop vendor or brand, Linux
kernel version and TLP version. The following explains:

    * Which laptops are supported
    * Which kernel drivers are necessary for them
    * What charge control options are offered
    * Which values for the charge thresholds are supported by the hardware (and by TLP)

.. note::

   Not all hardware listed here supports arbitrary charge thresholds (0..100),
   and not all hardware has both a start and stop threshold. Please read the
   relevant section before configuring thresholds.


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
    b. So-called "external" or "out-of-tree" kernel drivers, which are not part
       of the Linux mainline kernel and require the user to install additional,
       distribution specific packages.


Supported Hardware
^^^^^^^^^^^^^^^^^^

Apple Macbooks
""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Apple Silicon Macbooks - M* models with macOS firmware 13.0 or newer
   * - **Kernel drivers**
     -  `macsmc_power` - required, provided by the Asahi Linux kernel 6.3 or newer
   * - **TLP version (min)**
     - 1.7
   * - **TLP plugin**
     - macbook
   * - **Charge control options**
     - Start and stop charge threshold
   * - **Threshold configuration**
     - One battery only - `macsmc-battery` uses the `STOP_CHARGE_THRESH_BAT0` parameter
   * - **Start threshold values**
     - | The hardware enforces the start threshold depending on the stop threshold:
       | - stop=80 results in start=75
       | - stop=100 results in start=100
   * - **Stop threshold values**
     - | 80 - battery charges to 80%
       | 100 - battery charges to 100%, hardware default

.. rubric:: Sample configuration

Start charging battery `macsmc-battery` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75 # don't care
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: macbook
    Supported features: charge thresholds
    Driver usage:
    * natacpi (macsmc_power) = active (charge thresholds)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0:  don't care (hardware enforces 75, 100)
    * STOP_CHARGE_THRESH_BAT0:   80, 100(default)
    ...
    /sys/class/power_supply/macsmc-battery/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/macsmc-battery/charge_control_end_threshold   =     80 [%]


ASUS
""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - ASUS laptops
   * - **Kernel driver**
     - `asus_wmi` - required, included in distribution kernels (5.4 or newer)
   * - **TLP version (min)**
     - 1.4
   * - **TLP plugin**
     - asus
   * - **Charge control options**
     - Stop charge threshold
   * - **Threshold configuration**
     - | Batteries `BAT0`, `BATC` and `BATT` share the `STOP_CHARGE_THRESH_BAT0` parameter
       | Battery `BAT1` uses the `STOP_CHARGE_THRESH_BAT1` parameter
   * - **Stop threshold values**
     - | Range: 1 .. 100
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
    ...
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]


.. _bc-vendor-cros-ec:

Chromebooks and Framework
"""""""""""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - | Chromebooks - modded with chrultrabook/coreboot custom UEFI firmware
       | Framework laptops
   * - **Kernel drivers**
     -  `cros_charge-control` - required, included in distribution kernels (6.12 or newer)
   * - **TLP version (min)**
     - 1.8
   * - **TLP plugin**
     - cros-ec
   * - **Charge control options**
     - (Start and) stop charge threshold, recalibrate, discharge, chargeonce
   * - **Threshold configuration**
     - | `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
       | `BAT1` uses the `START/STOP_CHARGE_THRESH_BAT1` parameters
   * - **Start threshold values**
     - | Range: 0 .. 99
       | Special:
       | 0 - hardware default, treshold off
   * - **Stop threshold values**
     - | Range: 1 .. 100
       | Special:
       | 100 - hardware default, threshold off
   * - **Specifics**
     - | EC firmware v2 supports stop threshold only (applies to Framework).
       | EC firmware v3 supports start and stop threshold.
       | Special note for **Framework laptops**: if `cros_charge-control` and `framework_laptop` (:ref:`see below <bc-vendor-framework>`) are to be used simultaneously and `cros_charge-control` is to be used to control the charge threshold, the module option `probe_with_fwk_charge_control=1` must be set for `cros_charge-control`.

.. rubric:: Sample configuration

Start charging battery `BAT0` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75 # Applies to EC firmware v3 only
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b (EC firmware v2)

.. code-block:: none

    +++ Battery Care
    Plugin: cros-ec
    Supported features: charge threshold, recalibration
    Driver usage:
    * natacpi (cros_charge-control) = active (charge threshold, recalibration) - EC cmd v2
    Parameter value ranges:
    * STOP_CHARGE_THRESH_BAT0/1:   1..100(default)
    ...
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]
    /sys/class/power_supply/BAT0/charge_behaviour               = [auto] inhibit-charge force-discharge

.. rubric:: Sample output of tlp-stat -b (EC firmware v3)

.. code-block:: none

    +++ Battery Care
    Plugin: cros-ec
    Supported features: charge thresholds, recalibration
    Driver usage:
    * natacpi (cros_charge-control) = active (charge threshold, recalibration) - EC cmd v3
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1:  0..99(default)
    * STOP_CHARGE_THRESH_BAT0/1:   1..100(default)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]
    /sys/class/power_supply/BAT0/charge_behaviour               = [auto] inhibit-charge force-discharge


.. _bc-vendor-dell:

Dell
""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Dell laptops
   * - **Kernel drivers**
     -  `dell_laptop` - required, included in distribution kernels (6.12 or newer)
   * - **TLP version (min)**
     - 1.8
   * - **TLP plugin**
     - dell
   * - **Charge control options**
     - Start and stop charge threshold
   * - **Threshold configuration**
     - | `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
       | `BAT1` uses the `START/STOP_CHARGE_THRESH_BAT1` parameters
   * - **Start threshold values**
     - | Range: 50 .. 95
       | Special:
       | 95 - hardware default
       | The hardware enforces start = stop - 5
   * - **Stop threshold values**
     - | Range: 55..100
       | Special:
       | 100 - hardware default, threshold off
   * - **Specifics**
     - | TLP changes the `battery charge type
         <https://www.dell.com/support/manuals/en-us/dcpm2.1/userguide_dell-v1/battery-settings?guid=guid-0fbbbeff-4928-4def-89af-3d28d0a231ce&lang=en-us>`_
         to `Custom` so that the thresholds are effective. Other Dell
         charge types are not supported by TLP.
       | To be able to set the thresholds, it is necessary to remove
         the BIOS Admin password that may have been set. It is sufficient
         to do this temporarily in order to write the configured thresholds
         once with :command:`tlp setcharge`.

.. rubric:: Sample configuration

Start charging battery `BAT0` when below 75% and stop at 80%: ::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: dell
    Supported features: charge thresholds
    Driver usage:
    * natacpi (dell_laptop) = active (charge thresholds)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1: 50..95(default)
    * STOP_CHARGE_THRESH_BAT0/1: 55..100(default)
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =      75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =      80 [%]
    /sys/class/power_supply/BAT0/charge_types                   = Trickle Fast Standard Adaptive [Custom]

.. _bc-vendor-framework:

Framework
"""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Framework laptops
   * - **Kernel drivers**
     - `framework_laptop` - required, out-of-tree i.e. not included in distribution kernels
       → install from source
   * - **TLP version (min)**
     - 1.8
   * - **TLP plugin**
     - framework
   * - **Charge control options**
     - Stop charge threshold
   * - **Threshold configuration**
     - | `BAT0` uses the `START/STOP_CHARGE_THRESH_BAT0` parameters
       | `BAT1` uses the `START/STOP_CHARGE_THRESH_BAT1` parameters
   * - **Stop threshold values**
     - | Range: 1 .. 100
       | Special:
       | 100 - hardware default, threshold off
   * - **See also**
     - | The **recommended option** for Framework laptops is to use the
         :ref:`cros-ec plugin shown above <bc-vendor-cros-ec>`.
       | Advantages are:
       | - No need to install an out-of-tree kernel module, as everything is available in the distribution kernel
       | - Extra recalibration feature


.. rubric:: Sample configuration

Stop charging battery `BAT1` at 80%: ::

    START_CHARGE_THRESH_BAT1=0 # don't care
    STOP_CHARGE_THRESH_BAT1=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: framework
    Supported features: charge threshold
    Driver usage:
    * natacpi (cros_charge-control) = active (charge threshold)
    Parameter value ranges:
    * STOP_CHARGE_THRESH_BAT0/1:   1..100(default)
    ...
    /sys/class/power_supply/BAT1/charge_control_end_threshold   =     80 [%]


Huawei
""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Huawei MateBooks
   * - **Kernel driver**
     - `huawei_wmi` - required, included in distribution kernels (5.4 or newer)
   * - **TLP version (min)**
     - 1.4
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
     -  `thinkpad_acpi` - required, included in distribution kernels
   * - **TLP version (min)**
     - all
   * - **TLP plugin**
     - thinkpad
   * - **Charge control options**
     - Start and stop charge threshold, recalibrate, discharge, chargeonce
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
       | - :ref:`faq-thinkpad-battery-malfunc`
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
    ...
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
       | `tp_smapi` - required, out-of-tree → distribution specific package needed
   * - **TLP version (min)**
     - all
   * - **TLP plugin**
     - thinkpad-legacy
   * - **Charge control options**
     - start and stop charge threshold, recalibrate, discharge, chargeonce
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
    ...
    /sys/devices/platform/smapi/BAT0/start_charge_thresh        =     75 [%]
    /sys/devices/platform/smapi/BAT0/stop_charge_thresh         =     80 [%]
    /sys/devices/platform/smapi/BAT0/force_discharge            =      0


Lenovo non-ThinkPad series
""""""""""""""""""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Lenovo laptops (all non-ThinkPad series including ThinkBooks)
   * - **Kernel driver**
     - `ideapad_laptop` - required, included in distribution kernels
   * - **TLP version (min)**
     - 1.4
   * - **TLP plugin**
     - lenovo
   * - **Charge control options**
     - Fixed stop charge threshold aka *battery conservation mode*
   * - **Threshold configuration**
     - All batteries - `BAT0`, `BAT1` - share the `START/STOP_CHARGE_THRESH_BAT0` parameter
   * - **Stop threshold values**
     - | 1 - batteries charge to the fixed threshold
       | 0 - batteries charge to 100%, conservation mode off
   * - **Specifics**
     - | The fixed stop threshold value varies depending on the laptop model,
         60% or 80% are common.
       | There is no way to read out the actual threshold in Linux, therefore it
         cannot be displayed by :command:`tlp-stat -b`. The figure of 60% shown up to
         version 1.6 was based on an assumption, but (according to user feedback)
         does not apply to all models.
       | Some models ignore the setting, conservation mode remains
         off permanently.

.. rubric:: Sample configuration

Stop charging battery `BAT0` and `BAT1` at the fixed threshold: ::

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
   * - **TLP version (min)**
     - 1.4
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
       | **Note**: a regression in kernel 6.9 breaks `lg_laptop` → upgrade to 6.10.7 or later;
         mainline 6.6 LTS and Ubuntu's 6.8 were patched too

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
    ...
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =      80 [%]

    +++ Battery Care
    Plugin: lg-legacy
    Supported features: charge threshold
    Driver usage:
    * vendor (lg_laptop) = active (charge threshold)
    Parameter value range:
    * STOP_CHARGE_THRESH_BAT0: 80(on), 100(off) -- battery care limit

    /sys/devices/platform/lg-laptop/battery_care_limit          = 80 [%]


MSI laptops
"""""""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - MSI laptops
   * - **Kernel driver**
     - `msi_ec` - required, included in distribution kernels (6.3 or newer)
   * - **TLP version (min)**
     - 1.7
   * - **TLP plugin**
     - msi
   * - **Charge control options**
     - Start and stop charge threshold
   * - **Threshold configuration**
     - | Battery `BAT0` uses the `STOP_CHARGE_THRESH_BAT0` parameter
       | Battery `BAT1` uses the `STOP_CHARGE_THRESH_BAT1` parameter
   * - **Start threshold values**
     - | The hardware enforces the start threshold depending on the stop threshold:
       | start = stop - 10
   * - **Stop threshold values**
     - | Range: 10 .. 100
       | Special:
       | 100 - hardware default
   * - **Specifics**
     - | The kernel driver **only accepts very specific models and BIOS versions**, for the unsupported ones :command:`tlp-stat -b` will display `"Plugin: generic / Supported features: none available"`. Please do *not* open a TLP issue for this, instead create an `issue with the msi-ec driver <https://github.com/BeardOverflow/msi-ec/issues?q=is%3Aopen+is%3Aissue>`_.


.. rubric:: Sample configuration

Start charging battery `BAT1` when below 70% and stop at 80%: ::

    START_CHARGE_THRESH_BAT1=0  # don't care
    STOP_CHARGE_THRESH_BAT1=80

.. rubric:: Sample output of tlp-stat -b

.. code-block:: none

    +++ Battery Care
    Plugin: msi
    Supported features: charge thresholds
    Driver usage:
    * natacpi (msi_ec) = active (charge thresholds)
    Parameter value ranges:
    * START_CHARGE_THRESH_BAT0/1:  don't care (hardware enforces stop - 10)
    * STOP_CHARGE_THRESH_BAT0/1:   10..100(default)
    ...
    /sys/class/power_supply/BAT1/charge_control_start_threshold =     70 [%]
    /sys/class/power_supply/BAT1/charge_control_end_threshold   =     80 [%]


Samsung
"""""""
.. list-table::
   :widths: 250 1000
   :align: left

   * - **Hardware**
     - Samsung laptops
   * - **Kernel driver**
     - `samsung_laptop` - required, included in distribution kernels
   * - **TLP version (min)**
     - 1.4
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
   * - **TLP version (min)**
     - 1.5
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
   * - **TLP version (min)**
     - 1.6
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
    ...
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
   * - **TLP version (min)**
     - 1.6
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
    ...
    /sys/class/power_supply/BAT1/charge_control_end_threshold   =     80 [%]


Unsupported Hardware
^^^^^^^^^^^^^^^^^^^^
If the hardware does not have the capability or does not have a suitable kernel driver,
TLP battery care will not be able to control it.

.. note::

    Please do not submit TLP issues for
    hardware that does not have kernel driver support for charge control options. Providing
    kernel drivers is not part of the TLP project.

Incompatible Model
""""""""""""""""""
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

No Kernel Driver
""""""""""""""""
For any laptop vendor/brand/model without

* hardware capabilities or
* corresponding kernel driver

the :command:`tlp-stat -b` output would look like this:

.. code-block:: none

    +++ Battery Care
    Plugin: generic
    Supported features: none available
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold = (not available)
    /sys/class/power_supply/BAT0/charge_control_end_threshold   = (not available)

Please do not submit TLP issues for this case.

No TLP Plugin (yet)
"""""""""""""""""""
For a laptop that has the hardware capability and the necessary kernel driver,
but lacks a proper TLP battery care plugin, the :command:`tlp-stat -b` output would look
similar to this:

.. code-block:: none

    +++ Battery Care
    Plugin: generic
    Supported features: none available
    ...
    /sys/class/power_supply/BAT0/charge_control_start_threshold =     75 [%]
    /sys/class/power_supply/BAT0/charge_control_end_threshold   =     80 [%]

In this case, you might submit an issue to request development of a suitable plugin.

Final Notes
^^^^^^^^^^^

    * Consult the output of :command:`tlp-stat -b` for supported charge control
      options and allowed parameter values of your hardware
    * :command:`tlp setcharge` validates your configuration and reports errors
    * A value of 0 is translated to the vendor specific default (or the `disabled` state)
    * If the hardware supports only a stop charge threshold, use `START_CHARGE_THRESH_BATx=0`
    * In case the hardware supports both thresholds and you want to apply only one,
      then use `START_CHARGE_THRESH_BATx=0` or `STOP_CHARGE_THRESH_BATx=100`
      to skip the other one


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * Settings: :doc:`/settings/battery`
    * Commands: :ref:`cmd-tlp-battery-features`
    * FAQ: :doc:`/faq/battery`
