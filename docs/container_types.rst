.. _types:

Container Types
===============

Resource Containers (RCs) can be used to represent different forms of data.
These different forms are represented by the following types.

The types below are noted by ``Type Name (Type Slug)`` e.g. ``Book (book)``

.. note:: Most types support a condensed from. See :ref:`condensed` for details.

.. _types-book:

Book (book)
-----------

Represents any text that is structured like a book where there are chapters and chunks.

Books of the Bible should use `usfm` for the file format.
Open Bible Stories should use `markdown` for the file format.

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.md
        |    |-sub-title.md
        |    |-intro.md
        |    |-01.md
        |    |-02.md
        |    ...
        |    |-reference.md
        |    |-summary.md
        ...

Help (help)
-----------
.. note:: This type does not support the :ref:`condensed form <condensed>`.

A helpful resource to supplement chunks in a book e.g. notes or questions, and is structured in the same was as a :ref:`types-book`.
All help RCs must use the markdown format.

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

A standalone dictionary of terms. All dictionary RCs must use the markdown format.

The dictionary terms are used as the chapter :ref:`identifier` and is most often organized in the :ref:`condensed form <condensed>`.

.. code-block:: none

    content/
        ...
        |-aaron.md
        |-abel.md
        ...

.. note:: If desired, lengthy dictionary terms may use the :ref:`expanded form <condensed>` and be split into multiple chunks.

The ``01.txt`` file contains the description of the term. The term title must always be at the top of the file as a
h1 heading (a single #). :ref:`Links <linking>` may be used to reference other words, or content in other containers.

.. code-block:: markdown

    # Aaron #

    ## Word Data: ##

    * Strongs: H0175
    * Part of speech: Proper Noun

    ## Facts: ##

    Aaron was Moses' older brother. God chose Aaron to be the first high priest for the people of Israel.

    * Aaron helped Moses speak to Pharaoh about letting the Israelites go free.
    * While the Israelites were traveling through the desert, Aaron sinned by making an idol for the people to worship.
    * God also appointed Aaron and his descendants to be the [priests](../kt/priest) for the people of Israel.

    (Translation suggestions: [How to Translate Names](rc://en/ta/man/translate/translate-names))

    (See also: [Priest](../kt/priest.md), [Moses](../other/moses.md), [Israel](../other/israel.md))

    ## Bible References: ##

    * [1 Chronicles 23:12-14](rc://en/tn/help/1ch/23/12)
    * [Acts 07:38-40](rc://en/tn/help/act/07/38)
    * [Exodus 28:1-3](rc://en/tn/help/exo/28/01)
    * [Luke 01:5-7](rc://en/tn/help/luk/01/05)
    * [Numbers 16:44-46](rc://en/tn/help/num/16/44)

    ## Examples from the Bible stories: ##

    * __[09:15](rc://en/tn/help/obs/09/15)__ God warned Moses and __Aaron__  that Pharaoh would be stubborn.
    * __[10:05](rc://en/tn/help/obs/10/05)__ Pharaoh called Moses and __Aaron__  and told them that if they stopped the plague, the Israelites could leave Egypt.
    * __[13:09](rc://en/tn/help/obs/13/09)__ God chose Moses' brother, __Aaron__, and Aaron's descendants to be his priests.
    * __[13:11](rc://en/tn/help/obs/13/11)__ So they (the Israelites) brought gold to __Aaron__  and asked him to form it into an idol for them!
    * __[14:07](rc://en/tn/help/obs/14/07)__ They (the Israelites) became angry with Moses and __Aaron__  and said, "Oh, why did you bring us to this horrible place?"


The ``config.yaml`` file contains extra details about the term that may be helpful for some automation tools.

.. code-block:: yaml

    ---
      aaron:
        false_positives: []
        occurrences:
          - 'rc://en/ulb/book/1ch/23/12'
          - 'rc://en/ulb/book/1ch/07/38'
          - 'rc://en/ulb/book/1ch/28/01'
          - 'rc://en/ulb/book/1ch/01/05'
          - 'rc://en/ulb/book/1ch/16/44'
          - 'rc://en/obs/book/obs/09/15'
          - 'rc://en/obs/book/obs/10/05'
          - 'rc://en/obs/book/obs/13/09'
          - 'rc://en/obs/book/obs/13/11'
          - 'rc://en/obs/book/obs/14/07'

Generally, ``false_positives`` and ``occurrences`` are mutually exclusive.
That is, you should probably only have one or the other.

If ``false_positives`` exists, it is a list of places that should be excluded.
For example, if a typical regex search for "Aaron" would turn up instances that should not be shown to the user,
they should be listed here.

Alternatively, if ``occurrences`` exist,
then it specifies the entire list of occurrences of this word in the given resource.
If this key exists then a regex search should not be performed by the software.

.. _types-manual:

Manual (man)
------------

A user manual. All manuals must use the markdown format.

Manuals are a collection of modules/articles:

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

.. note:: If desired the module can be split into additional chunks.

The ``config.yaml`` file indicates recommended and dependent modules:

.. code-block:: yaml

    ---
      translate-unknowns:
        recommended:
          - 'translate-names'
          - 'translate-transliterate'
        dependencies:
          - 'figs-sentences'

Dependencies are :ref:`identifier` s of modules that should be read before this one.
Recommendations are modules that would likely benefit the reader next.

.. _types-bundle:

Bundle (bundle)
---------------

A bundle is simply a flat directory (no sub-folders) with a single file for each project. e.g. there is no :ref:`structure-content`.
This type is particularly suited for `USFM <http://ubsicap.github.io/usfm/>`_ when providing "USFM Bundles".

When defining a project in the :ref:`manifest` be sure the path is pointing to a file and not a directory.

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

RC file structure:

.. code-block:: none

    my_rc/
        ...
        |-01-GEN.usfm
        |-manifest.yaml

.. note:: When your application supports "USFM Bundles" it can identify the them in two ways

    - attempt to read the :ref:`manifest` to determine type as ``bundle`` and the format as ``text/usfm``.
    - look for any ``*.usfm`` files in the root directory if the :ref:`manifest` does not exist.

    In this way the application will satisfy both the ``Bundle`` RC type described above and generic "USFM Bundles"
    as is common in the industry.
