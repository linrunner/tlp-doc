Radio Device Wizard
===================
The `Radio Device Wizard` provides the capability to enable or disable builtin
bluetooth, Wi-Fi and WWAN devices triggered by certain events.

.. important:: The `Radio Device Wizard` settings shown below require the
    installation of the optional **tlp-rdw** package, which in turn requires
    `NetworkManager`.


DEVICES_TO_DISABLE_ON_*_CONNECT
-------------------------------------------
.. rubric:: Disable on Network Connect

When a LAN, Wi-Fi or WWAN connection has been established, the radio
devices entered here will be disabled:

* bluetooth
* wifi – Wireless LAN
* wwan – Wireless Wide Area Network (3G/UMTS, 4G/LTE, 5G)

Multiple devices are separated with blanks. ::

    DEVICES_TO_DISABLE_ON_LAN_CONNECT="wifi wwan"
    DEVICES_TO_DISABLE_ON_WIFI_CONNECT="wwan"
    DEVICES_TO_DISABLE_ON_WWAN_CONNECT="wifi"

Default when unconfigured: <none>


DEVICES_TO_ENABLE_ON_*_DISCONNECT
---------------------------------------------
.. rubric:: Enable on Network Disconnect

When a LAN, Wi-Fi, WWAN connection has been disconnected, the radio
devices entered here will be enabled. ::

    DEVICES_TO_ENABLE_ON_LAN_DISCONNECT="wifi wwan"
    DEVICES_TO_ENABLE_ON_WIFI_DISCONNECT=""
    DEVICES_TO_ENABLE_ON_WWAN_DISCONNECT=""

Default when unconfigured: <none>


DEVICES_TO_ENABLE/DISABLE_ON_DOCK
---------------------------------
.. rubric:: Enable/Disable on Dock

After docking the radio devices entered here will be enabled/disabled. ::

    DEVICES_TO_ENABLE_ON_DOCK=""
    DEVICES_TO_DISABLE_ON_DOCK=""

Default when unconfigured: <none>


DEVICES_TO_ENABLE/DISABLE_ON_UNDOCK
-----------------------------------
.. rubric:: Enable/Disable on Undock

After undocking the radio devices entered here will be enabled/disabled. ::

    DEVICES_TO_ENABLE_ON_UNDOCK="wifi"
    DEVICES_TO_DISABLE_ON_UNDOCK=""

Default when unconfigured: <none>

.. note::

    * Not all docks can be recognized, especially USB-C docks.
      However, a generic detection exists for Thunderbolt docks.
    * If your dock is not recognized but a LAN cable is connected,
      use `DEVICES_TO_DISABLE_ON_LAN_CONNECT`
      and `DEVICES_TO_ENABLE_ON_LAN_DISCONNECT` instead.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/radio`
