Audio
=====

.. _set-audio-powersave:

SOUND_POWER_SAVE_ON_AC/BAT
--------------------------
::

    SOUND_POWER_SAVE_ON_AC=1
    SOUND_POWER_SAVE_ON_BAT=1

Timeout (in seconds) for the audio power saving mode (supports Intel HDA, AC97).
A value of 1 is recommended for Linux desktop environments with PulseAudio,
systems without PulseAudio may require 10. The value 0 disables power save.

Default when unconfigured:

    | 1 (AC), 1 (BAT) - *Version 1.4 and newer*
    | 0 (AC), 1 (BAT) - *Version 1.3*

.. note::

    * Power saving mode may cause slight clicks or other disturbances in sound
      output, see the FAQ: :doc:`/faq/audio`
    * Audio power saving mode is a prerequisite for :ref:`automatic power down
      of Nvidia Optimus GPUs <faq-powercon-optimus-audio>`
    * The recommendation of 1 for PulseAudio originates from the
      `pulseaudio-discussion mailing list <https://lists.freedesktop.org/archives/pulseaudio-discuss/2017-December/029154.html>`_

SOUND_POWER_SAVE_CONTROLLER
---------------------------
::

    SOUND_POWER_SAVE_CONTROLLER=Y

* Y – power off the controller together with the sound chip
* N – controller remains active

Default when unconfigured: Y
