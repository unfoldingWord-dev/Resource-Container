.. _ubxf:

Unlocked Bible Interchange Format (UBXF)
========================================

Overview
--------

The Unlocked Bible Interchange Format (UBXF) is a special type of Resource Container
designed to provide a consistent format for separate apps to exchange Bible
translations and metadata with one another.

.. _ubxf-characteristics:

Characteristics
---------------

The UBXF is a specific type of RC :ref:`types-bundle` that utilizes the USFM 3
specification to allow extra metadata to be encoded directly in the translation.
All the characteristics of a RC :ref:`types-bundle` apply, with these added
requirements:

- UBXF must provide its data in `USFM 3 <https://ubsicap.github.io/usfm/index.html>`_.
- The ``format`` field of the ``manifest.yaml`` file must be set to ``text/usfm3``.
- Word metadata should be encoded in the translation using `USFM 3 word attributes <https://ubsicap.github.io/usfm/attributes/index.html>`_.
- Alignment metadata should be encoded using `USFM 3 milestones <https://ubsicap.github.io/usfm/milestones/index.html>`_.  See :ref:`ubxf-alignmentexample` below.

.. _ubxf-licensing:

Licensing
---------

A UBXF is a derivitive work of the source and target translations in the project. It is strongly recommended that you only use texts with one of these licenses:

-  Public Domain
- `CC0 <https://creativecommons.org/publicdomain/zero/1.0/>`_
- `CC BY <https://creativecommons.org/licenses/by/4.0/>`_
- `CC BY-SA <https://creativecommons.org/licenses/by-sa/4.0/>`_

Use the ``source`` array in the ``manifest.yaml`` file to indicate the upstream resources.

.. _ubxf-sourcetexts:

Recommended Source Texts
------------------------

For New Testament alignment, the `Unlocked Greek New Testament (UGNT) <https://unfoldingword.org/ugnt/>`_ or the `Bunning Heuristic Prototype (BHP) <https://git.door43.org/Door43/BHP>`_ are the recommended sources.

For Old Testament alignment, the `Unlocked Hebrew Bible (UHB) <https://unfoldingword.org/uhb/>`_ or the `Open Scriptures Hebrew Bible (OSHB) <https://github.com/openscriptures/morphhb/releases/latest>`_ are the recommended sources.

.. _ubxf-alignmentexample:

Alignment Data Example
----------------------

Here is an example of alignment data encoded in UFSM 3 milestones. Note the following features:

- Punctuation occurs outside the ``\w`` markers.
- Alignment data must use the ``x-aln`` milestone markers.  (Simple word for word alignment data could use word level attributes, but that makes it more difficult for software to process).
- Use a short code name for the actual Greek or Hebrew text that has been aligned.  The example below uses the ``x-ugnt`` attribute to indicate that the `UGNT text <https://unfoldingword.org/ugnt/>`_ is the source. Note that the code should match what is listed in the ``source`` array in the ``manifest.yaml`` file.
- The ``occurrence`` and ``occurrences`` attributes can help software identify individual occurrences of identical words within a verse.

.. code-block:: none

    \v 1 \x-aln-s | strong="G39720" x-occurrence="1" x-occurrences="1" x-ugnt="Παῦλος"
    \w Paul|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*,
    \x-aln-s | strong="G14010" x-occurrence="1" x-occurrences="1" x-ugnt="δοῦλος"
    \w a|x-occurrence="1" x-occurrences="1"\w*
    \w servant|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G23160" x-occurrence="1" x-occurrences="2" x-ugnt="Θεοῦ"
    \w of|x-occurrence="1" x-occurrences="4"\w*
    \w God|x-occurrence="1" x-occurrences="2"\w*
    \x-aln-e\*
    \x-aln-s | strong="G06520" x-occurrence="1" x-occurrences="1" x-ugnt="ἀπόστολος"
    \w and|x-occurrence="1" x-occurrences="2"\w*
    \w an|x-occurrence="1" x-occurrences="4"\w*
    \w apostle|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G11610" x-occurrence="1" x-occurrences="1" x-ugnt="δὲ"
    \w of|x-occurrence="2" x-occurrences="4"\w*
    \x-aln-e\*
    \x-aln-s | strong="G24240" x-occurrence="1" x-occurrences="1" x-ugnt="Ἰησοῦ"
    \w Jesus|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G55470" x-occurrence="1" x-occurrences="1" x-ugnt="Χριστοῦ"
    \w Christ|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*,
    \x-aln-s | strong="G25960" x-occurrence="1" x-occurrences="1" x-ugnt="κατὰ"
    \w for|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G41020" x-occurrence="1" x-occurrences="1" x-ugnt="πίστιν"
    \w the|x-occurrence="1" x-occurrences="3"\w*
    \w faith|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G23160" x-occurrence="2" x-occurrences="2" x-ugnt="Θεοῦ"
    \w of|x-occurrence="3" x-occurrences="4"\w*
    \w God's|x-occurrence="2" x-occurrences="2"\w*
    \x-aln-e\*
    \x-aln-s | strong="G15880" x-occurrence="1" x-occurrences="1" x-ugnt="ἐκλεκτῶν"
    \w chosen|x-occurrence="1" x-occurrences="1"\w*
    \w people|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G25320" x-occurrence="1" x-occurrences="1" x-ugnt="καὶ"
    \w and|x-occurrence="2" x-occurrences="2"\w*
    \x-aln-e\*
    \x-aln-s | strong="G02250" x-occurrence="1" x-occurrences="1" x-ugnt="ἀληθείας"
    \w the|x-occurrence="2" x-occurrences="3"\w*
    \x-aln-e\*
    \x-aln-s | strong="G19220" x-occurrence="1" x-occurrences="1" x-ugnt="ἐπίγνωσιν"
    \w knowledge|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G02250" x-occurrence="1" x-occurrences="1" x-ugnt="ἀληθείας"
    \w of|x-occurrence="4" x-occurrences="4"\w*
    \w truth|x-occurrence="1" x-occurrences="1"\w*
    \w the|x-occurrence="3" x-occurrences="3"\w*
    \x-aln-e\*
    \x-aln-s | strong="G35880" x-occurrence="1" x-occurrences="1" x-ugnt="τῆς"
    \w that|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G25960" x-occurrence="1" x-occurrences="1" x-ugnt="κατ’"
    \w agrees|x-occurrence="1" x-occurrences="1"\w*
    \w with|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*
    \x-aln-s | strong="G21500" x-occurrence="1" x-occurrences="1" x-ugnt="εὐσέβειαν"
    \w godliness|x-occurrence="1" x-occurrences="1"\w*
    \x-aln-e\*,
