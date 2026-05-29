Graphics
========

.. seealso::

    Power saving for hybrid graphics is covered in
    :ref:`faq-powercon-hybrid-graphics`.

Screen flicker
---------------
.. rubric:: Nvidia dGPU

Affected hardware: a Dell Latitude 5480 user reported *"screen flickers at an
inconsistent rate, though not that sporadic"*.

Solution: disable ASPM on battery in the configuration ::

    PCIE_ASPM_ON_BAT=default
