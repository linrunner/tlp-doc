Other Linux Distributions
=========================
.. note::

    Before installing from source please check your distribution's repositories
    whether packages already exist there.

You may install the source tarball from `Downloads`_:

1. Unpack ::

        tar xfvz TLP-V.v.v.tar.gz
        cd TLP-V.v.v

2. Install (as root) ::

        make install install-man

.. include:: ../include/warn-source-install.rst

3. Enable the services (as root) ::

        systemctl enable tlp.service

4. *For version 1.9 and newer* with tlp-pd, additionally: ::

        systemctl enable --now tlp-pd.service

.. seealso::

    * FAQ: :ref:`Service unit <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`


.. _`Downloads`: https://github.com/linrunner/TLP/releases
