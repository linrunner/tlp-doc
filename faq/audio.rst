Audio
=====
When encountering problems with the sound output, for example clicking
noises, try to increase the timeout:

*Version 1.11 and newer* ::

    SOUND_POWER_SAVE_ON_PRF=10
    SOUND_POWER_SAVE_ON_BAL=10
    SOUND_POWER_SAVE_ON_SAV=10

*Version 1.10 and older* ::

    SOUND_POWER_SAVE_ON_AC=10
    SOUND_POWER_SAVE_ON_BAT=10

or disable power saving mode completely:

*Version 1.11 and newer* ::

    SOUND_POWER_SAVE_ON_PRF=0
    SOUND_POWER_SAVE_ON_BAL=0
    SOUND_POWER_SAVE_ON_SAV=0

*Version 1.10 and older* ::

    SOUND_POWER_SAVE_ON_AC=0
    SOUND_POWER_SAVE_ON_BAT0


.. seealso ::

    * :ref:`set-audio-powersave`
    * Audio power saving mode is a prerequisite for :ref:`automatic power down
      of Nvidia Optimus GPUs <faq-powercon-optimus-audio>`
