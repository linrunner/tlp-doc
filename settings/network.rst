Networking
==========

.. _set-wifi-pwr:

WIFI_PWR_ON_AC/BAT
------------------
::

    WIFI_PWR_ON_AC=off
    WIFI_PWR_ON_BAT=on

Sets Wi-Fi power saving mode. Adapter support depends on kernel and driver.
Possible values:

* off – disabled
* on – enabled

Default when unconfigured: off (AC), on (BAT)

Hint: deprecated config values 1=off/5=on are supported for backwards
compatibility.

.. note::

    Power saving mode can cause an unstable Wi-Fi link.

.. set-wol-disable:

WOL_DISABLE
-----------
::

    WOL_DISABLE=Y

* Y – Disable Wake-on-LAN
* N – Do not disable Wake-on-LAN (keep BIOS default)

Default when unconfigured: Y

.. note::

    * After changing to `WOL_DISABLE=N` a reboot is required to make the
      new setting effective (or use :command:`sudo ethtool -s <interface> wol g`
      in a shell).
    * Your laptop's BIOS may have an option for Wake-on-LAN too. Some vendors
      even allow to configure Wake-on-LAN on `AC only`. If you choose the BIOS
      option, leave `WOL_DISABLE=N`.
