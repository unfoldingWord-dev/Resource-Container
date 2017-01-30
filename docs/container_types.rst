.. _types:

Container Types
===============

Resource Containers (RCs) can be used to represent different forms of data.
These different forms are represented by the following RC Types.

.. note:: The types below are noted by ``Type Name (Type Slug)``
    e.g. ``Book (book)``

.. _types-book:

Book (book)
-----------

Represents any text that is structured like a book, there are chapters and chunks.

Config
^^^^^^
A book may reference supplementary RCs in it's ``config.yaml`` file via :ref:`linking`.
Such references must be grouped by the corresponding RC type.
The order of elements in each of these groups should be respected.

.. code-block:: yaml

    ---
    content:
      01:      // chapter
        01:    // chunk
          dict:
            - '//tw/bible/dict/creation'
            - '//tw/bible/dict/god'
            - '//tw/bible/dict/heaven'
            - '//tw/bible/dict/holyspirit'
          help:
            - '//tq/gen/help/01/01'
            - '//tn/gen/help/01/01'
          img:
            - '//ulb/gen/img/01/01'

.. note:: Implementation Notes:
    References to external RCs may be displayed in the application along the side of the book content in order to provide contextual information.

.. _types-image:

Image (img)
-----------

Represents a set of images that follows the same structure as a book.
In most cases you will want to provide a single image to an equivalent chunk in the related book.
However, any image can be included so long as it follows the requirements for identifiers.

Below is an example with a format of ``image/png``.

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.png
        |    |-sub-title.png
        |    |-intro.png
        |    |-reference.png
        |    |-summary.png
        |    |-01.png
        |    |-02.png
        |    ...
        ...
        |-front/
        |-back/
        ...


.. _types-audio:

Audio (audio)
-------------

Represents a set of audio files that follows the same structure as a book.
It is valid to provide a single audio file to any equilvilant chunk in a book.

Below is an example with a format of ``audio/mp3``.

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.mp3
        |    |-sub-title.mp3
        |    |-intro.mp3
        |    |-reference.mp3
        |    |-summary.mp3
        |    |-01.mp3
        |    |-02.mp3
        |    ...
        ...
        |-front/
        |-back/
        ...
        

.. _types-video:

Video (vid)
-----------

Represents a set of video files that follows the same structure as a book.
It is valid to provide a single video file to any equivalent chunk in a book.

Below is an example with a format of ``video/mp4``.

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.mp4
        |    |-sub-title.mp4
        |    |-intro.mp4
        |    |-reference.mp4
        |    |-summary.mp4
        |    |-01.mp4
        |    |-02.mp4
        |    ...
        ...
        |-front/
        |-back/
        ...
        

Help (help)
-----------

A helpful resource to supplement chunks in a book. e.g. notes or questions.
Currently all help RCs must use the markdown format.

Each chunk contains one or more helps which correlate to the corresponding chunk in a book RC:

.. code-block:: markdown

    # In the beginning God created

    This introductory statement gives a summary of the rest of the chapter. AT: "This is about how God made...in the beginning." Some languages translate it as "A very long time ago God created." Translate it in a way that that shows that this actually happened and is not just a folk story.

    # In the beginning

    This refers to the start of the world and everything in it.

When parsed by an app the helps in this chunk are split at the headers.
If there is preceding text (without a header) it will be displayed as a single help and a short snippet of the text will be used for the header if applicable.


.. _types-dictionary:

Dictionary (dict)
-----------------

A standalone dictionary of terms. Currently all dictionary RCs must use the markdown format.

The dictionary terms are used as the chapter :ref:`slug` and the description of the term is placed inside a ``01.txt`` file:

.. code-block:: none

    content/
        |-config.yaml
        |-aaron/
        |    |-01.txt
        |
        |-abel/
        ...
        |-unclean/

.. note:: Lengthy dictionary terms may be split into more than one chunk.

The ``01.txt`` file contains the description of the term where the header is the title of the term and the rest is the description:

.. code-block:: markdown

    # Aaron

    God chose Aaron to be the first high priest for the people of Israel.

The ``config.yaml`` file is used to indicate related terms, aliases, definition title, and examples.

.. code-block:: yaml

    ---
      aaron:
        def_title: 'Description'
        see_also: 
          - 'covenant'
          - 'testimony'
        aliases:
          - aaronalias # note: not a real alias for this word
        examples:
          - '09-15'
          - '10-05'

Examples are tricky because a ``dict`` may be referenced by many different resources and projects.
Therefore we cannot specify a RC link but instead must simply provide the chapter and chunk that contains the example.


.. _types-manual:

Manual (man)
------------

A user manual. For now manual RCs must use the markdown format.

Manuals are a collection of modules (articles):

.. code-block:: none

    content/
        ...
        |-translate-unknowns
        |    |-title.txt
        |    |-sub-title.txt
        |    |-01.txt
        ...
        |-writing-decisions/

The ``01.txt`` file contains the translation of the module.

.. note:: If desired the module can be split into multiple chunks.

The ``config.yaml`` file indicates recommended and dependent modules:

.. code-block:: yaml

    ---
      translate-unknowns: 
        recommended: 
          - 'translate-names'
          - 'translate-transliterate'
        dependencies: 
          - 'figs-sentences'

Dependencies are :ref:`slug` s of modules that should be read before this one.
Recommendations are modules that would likely benefit the reader next.

.. _types-usfm:

Bundle (bundle)
---------------

A bundle is simply a flat directory (no sub-folders) with a single file for each project.
This type is particularly suited for `USFM <http://ubsicap.github.io/usfm/>`_ when providing "USFM Bundles".

A project block in the :ref:`manifest`:

.. code-block:: yaml

    ---
      projects:
        -
          identifier: 'gen'
          title: 'Genesis'
          versification: 'kjv'
          sort: 1
          path: './01-GEN.usfm'
          categories:
          - 'bible-ot'

Directory structure

.. code-block:: none

    myrc/
        |-01-GEN.usfm
        |-LICENSE.md
        |-manifest.yaml

.. note:: When your application supports "USFM Bundles" it can identify the them in two ways

    - attempt to read the :ref:`manifest` to determine type as ``bundle`` and the format as ``text/usfm``.
    - look for any ``*.usfm`` files in the root directory if the :ref:`manifest` does not exist.

    In this way the application will satisfy both the ``Bundle`` RC type described above and generic "USFM Bundles"
    as is common in the industry.
