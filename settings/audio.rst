.. _set-audio:

Audio
=====

SOUND_POWER_SAVE_ON_AC/BAT
--------------------------
::

    SOUND_POWER_SAVE_ON_AC=0
    SOUND_POWER_SAVE_ON_BAT=1

Timeout (in seconds) for the audio power saving mode (supports Intel HDA, AC97).
A value of 0 disables power save.

.. note::

    Power saving mode may cause slight clicks or other disturbances in sound
    output. See the FAQ: :ref:`faq-audio`.

SOUND_POWER_SAVE_CONTROLLER
---------------------------
::

    SOUND_POWER_SAVE_CONTROLLER=Y

* Y – power off the controller together with the sound chip
* N – controller remains active

Default when unconfigured: Y
