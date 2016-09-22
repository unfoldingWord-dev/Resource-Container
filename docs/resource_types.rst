Resource Types
==============

Content can be displayed in different forms and used in different ways within an application. Therefore several resource types exist to support different needs.

All resource types, unless otherwise specified, may employ any translation mode. Translation modes indicate to an application under what conditions a resource container is translatable.



Book
----

Represents any text that is structured like a book. e.g. there are chapters and chunks.

Related resources can be indicated in the config.yml file:

.. code-block:: yaml

    ---
    content:
      01:      // chapter
        01:    // chunk
          words: 
            - "//bible/tw/creation"
            - "//bible/tw/god"
            - "//bible/tw/heaven"
            - "//bible/tw/holyspirit"
          questions: 
            - "//gen/tq/01:01"
          notes: 
            - "//gen/tn/01:01"
          images: 
            - "image://gen/ulb/01:01"

Implementation Notes:
Related resources as shown above may be displayed in the application along the side of the book content in order to provide contextual information.

Help
----

A helpful resource to supplement chunks in a book. e.g. notes or questions. Currently all help resources must use the markdown format.

Each chunk contains one or more helps which correlate to the corresponding chunk in a book resource:

.. code-block:: none

    #In the beginning God created

    This introductory statement gives a summary of the rest of the chapter. AT: "This is about how God made...in the beginning." Some languages translate it as "A very long time ago God created." Translate it in a way that that shows that this actually happened and is not just a folk story.

    #In the beginning

    This refers to the start of the world and everything in it.

When parsed by an app the helps in this chunk are split at the headers. If there is preceding text (without a header) it will be displayed as a single help and a short snippet of the text will be used for the header if applicable.


Dictionary
----------

A standalone dictionary of terms. Currently all dictionary resources must use the markdown format.

The dictionary terms are used as the chapter slug and the translation of the term is placed inside a single 01.txt file:

.. code-block:: none

    content/
        |-config.yml
        |-aaron/
        |    |-01.txt
        |
        |-abel/
        ...
        |-unclean/

NOTE: lengthy dictionary terms may be split into more than one chunk.

The 01.txt file contains the translation of the term where the header is the title of the term and the rest is the description:

.. code-block:: none

    #Aaron

    God chose Aaron to be the first high priest for the people of Israel.

The config.yml is used to indicate related terms, aliases, and examples.

.. code-block:: yaml

    ---
      aaron: 
        see_also: 
          - "covenant"
          - "testimony"
        aliases:
          - aaronalias # note: not a real alias for this word
        examples:
          - "09-15"
          - "10-05"

Examples are tricky because a dict may be referenced by many different projects/resources. Therefore we cannot specify a resource link but instead must simply provide the chapter and chunk that contains the example.


Manual
------

A user manual. For now manual resources must use the markdown format.

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

The 01.txt file contains the translation of the module. The title.txt file contains the name of the module. And sub-title.txt contains the question that is answered by this module.

NOTE: if desired the module can be split into multiple chunks.
The config.yml indicates recommended and dependent modules:

.. code-block:: yaml

    ---
      translate-unknowns: 
        recommended: 
          - "translate-names"
          - "translate-transliterate"
        dependencies: 
          - "figs-sentences"

Dependencies are id's of modules that should be read before this one. Recommendations are modules that would likely benefit the reader next.
