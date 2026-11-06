Networking
==========

.. _set-wifi-pwr:

WIFI_PWR_ON_PRF/BAL/SAV
-----------------------
Sets Wi-Fi power saving mode. Adapter support depends on kernel and driver.
Possible values:

* off – disabled
* on – enabled

*Version 1.11 and newer* ::

    WIFI_PWR_ON_PRF=off
    WIFI_PWR_ON_BAL=on
    WIFI_PWR_ON_SAV=on

*Version 1.10 and older* ::

    WIFI_PWR_ON_AC=off
    WIFI_PWR_ON_BAT=on

Default when unconfigured: off (PRF/AC), on (BAL/BAT, SAV)

.. note::

    Power saving mode can cause an unstable Wi-Fi link.

.. set-wol-disable:

WOL_DISABLE
-----------
Provision for Wake-On-LAN:

* Y – Disable Wake-on-LAN
* N – Do not disable Wake-on-LAN (keep BIOS default)

::

    WOL_DISABLE=Y

Default when unconfigured: Y

.. note::

    * After changing to `WOL_DISABLE=N` a reboot is required to make the
      new setting effective (or use :command:`sudo ethtool -s <interface> wol g`
      in a shell).
    * Your laptop's BIOS may have an option for Wake-on-LAN too. Some vendors
      even allow to configure Wake-on-LAN on `AC only`. If you choose the BIOS
      option, leave `WOL_DISABLE=N`.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/radio`
