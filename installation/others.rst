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

        make install
        make install-man

.. include:: ../include/warn-source-install.rst

3. Enable the services (as root) ::

        systemctl enable tlp.service

.. seealso::

    * FAQ: :ref:`service units <faq-service-units>`
    * FAQ: :ref:`faq-ppd-conflict`


.. _`Downloads`: https://github.com/linrunner/TLP/releases
