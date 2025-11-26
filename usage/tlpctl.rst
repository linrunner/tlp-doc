tlpctl
------
*Version 1.9 and newer with tlp-pd*

.. topic:: Purpose

    Control TLP power profiles using the TLP Profiles Daemon *tlp-pd*.


.. _usage-tlpctl-profiles:

Profile Shortcuts
^^^^^^^^^^^^^^^^^
In addition to clicking on the Linux desktop panel, the power profile can also be
selected via the command line. Unlike the traditional command :doc:`tlp <tlp>`,
root privileges are not required.

Switch to the `performance` profile (`_AC` parameters): ::

    tlpctl performance

Switch to the `balanced` profile (`_BAT` parameters): ::

    tlpctl balanced

Switch to the `power-saver` profile (`_SAV` parameters): ::

    tlpctl power-saver


Profile Management
^^^^^^^^^^^^^^^^^^
List available power profiles: ::

     tlpctl list

The active profile is marked with an asterisk (*).

Print the currently active power profile: ::

    tlpctl get

Set the active power profile: ::

     tlpctl set <profile>

Valid profiles are: `performance`, `balanced` and `power-saver`.
This will release all active profile holds.

Profile Holds
^^^^^^^^^^^^^^
Run a command and request a specific power profile for it (*"profile hold"*): ::

    tlpctl launch <command> [options]

.. rubric:: Launch Options

Profile to hold while running the command (default: `performance`): ::

    -p, --profile <profile>

Reason for the profile hold (defaults to the second and remaining words of the command): ::

    -r, --reason <reason>

Application identifier for the hold (defaults to the first word of the command): ::

    -i, --appid <appid>

Holds are automatically released, returning to the user's selected profile, when:

* the holding command exits,
* the user manually changes the profile (with tlpctl set),
* the application explicitly releases the hold.

Multiple holds can be active simultaneously.

List current power profile holds (from the `launch` command), showing the profile name,
application ID, and reason for each hold:

.. code-block:: bash

     tlpctl list-holds


.. rubric:: Launch Examples

Launch a game with performance profile:

.. code-block:: bash

    tlpctl launch --profile performance --reason "Gaming" steam

Launch a command with default performance hold:

.. code-block:: bash

    tlpctl launch make -j8


Diagnostics and Debugging
^^^^^^^^^^^^^^^^^^^^^^^^^
Set the loglevel of `tlp-pd`: ::

    sudo tlpctl loglevel <level>

Valid levels are: `info` and `debug`. Requires root privilege.

Version
^^^^^^^
Display version information for both the `tlpctl` client and `tlp-pd`: ::

    tlpctl --version
