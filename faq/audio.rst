Audio
=====
When encountering problems with the sound output on battery, for example clicking
noises, try to increase the timeout ::

    SOUND_POWER_SAVE_ON_BAT=10

or disable power saving mode completely ::

    SOUND_POWER_SAVE_ON_BAT=0

.. seealso ::

    * :ref:`set-audio-powersave`
    * Audio power saving mode is a prerequisite for :ref:`automatic power down
      of Nvidia Optimus GPUs <faq-powercon-optimus-audio>`

