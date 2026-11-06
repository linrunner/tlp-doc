Audio
=====

.. _set-audio-powersave:

SOUND_POWER_SAVE_ON_PRF/BAL/SAV
-------------------------------
Timeout (in seconds) for the audio power saving mode (supports Intel HDA, AC97 sound chips).
A value of 1 is recommended for Linux desktop environments with PipeWire or PulseAudio sound
servers; systems without may require 10. The value 0 disables power save.

*Version 1.11 and newer* ::

    SOUND_POWER_SAVE_ON_PRF=1
    SOUND_POWER_SAVE_ON_BAL=1
    SOUND_POWER_SAVE_ON_SAV=1

*Version 1.10 and older* ::

    SOUND_POWER_SAVE_ON_AC=1
    SOUND_POWER_SAVE_ON_BAT=1

Default when unconfigured: 1

.. note::

    * Power saving mode may cause slight clicks or other disturbances in sound
      output, see the FAQ: :doc:`/faq/audio`
    * Audio power saving mode is a prerequisite for :ref:`automatic power down
      of Nvidia Optimus GPUs <faq-powercon-optimus-audio>`
    * The recommendation of 1 for PulseAudio originates from the
      `pulseaudio-discussion mailing list <https://lists.freedesktop.org/archives/pulseaudio-discuss/2017-December/029154.html>`_


SOUND_POWER_SAVE_CONTROLLER
---------------------------
Power saving of the sound controller:

* Y – power off the controller together with the sound chip
* N – controller remains active

::

    SOUND_POWER_SAVE_CONTROLLER=Y

Default when unconfigured: Y


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/audio`
