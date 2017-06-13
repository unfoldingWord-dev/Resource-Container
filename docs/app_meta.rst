.. _app_meta:

App Meta
========

Applications can store meta data in a Resource Container (RC) within the ``.apps`` directory.

.. note:: The contents of this directory are outside the scope of the RC specification.
    The examples below are only a suggestion.

translationStudio
-----------------

translationStudio keeps track of stages chunks go through during translation.

.. code-block:: none

    my_rc/.apps/ts/
        |-status.json

translationCore
---------------

translationCore keeps track of checking information.

.. code-block:: none

    my_rc/.apps/tc/
        |-checkdata/
        |   |-LexicalChecker.tc
        |   |-ProposedChanges.tc
        |   |-common.tc
        |
        |-tc-manifest.json

unfoldingWord
-------------

unfoldingWord uses dynamically provided localization files for use in the app interface.

.. code-block:: none

    my_rc/.apps/uw/
        |-app_words.json