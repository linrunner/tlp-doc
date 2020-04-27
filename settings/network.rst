Networking
==========

WIFI_PWR_ON_AC/BAT
------------------
::

    WIFI_PWR_ON_AC=off
    WIFI_PWR_ON_BAT=on

Sets Wi-Fi power saving mode. Adapter support depends on kernel and driver.
Possible values:

* off – disabled
* on – enabled

Hint: deprecated config values 1=off/5=on are supported for backwards
compatibility.

.. note::

    Power saving mode can cause an unstable Wi-Fi link.

.. set-wol-disable:

WOL_DISABLE
-----------
::

    WOL_DISABLE=Y

* Y – Wake on LAN disabled
* N – Wake on LAN enabled

Default when unconfigured: N

Hint: after enabling a reboot is required to ensure that the new setting takes.
