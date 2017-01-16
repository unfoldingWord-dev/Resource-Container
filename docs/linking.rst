.. _linking:

Linking
=======

A Resource Container (RC) link allows one RC to reference content from another RC.

All RC links follow a very simple structure in two different flavors.

* **Anonymous links** - have no title and are declared by enclsing the link in double brackets
* **Titled links** - have a title and are indicated by enclosing the link title in single brackets and the link in parentheses.

For example:

.. code-block:: markdown

    [[language/resource/project/type]]

    [Link Title](language/resource/project/type)

Structure
---------

The minimum form of a link is ``language/resource/project/type``.
We interpret this as the project content directory inside the RC.
This is illustrated below:

.. code-block:: none

    # link
    en/ulb/gen/book

    # file system
    en_ulb_gen_book/
        |-LICENSE.md
        |-manifest.yml
        |-content/ <-- link points here

From this point we can lengthen the link to include a chapter :ref:`slug` which resolves to the chapter directory.

.. code-block:: markdown

    # link
    en/ulb/gen/book/01

    # file system
    en_ulb_gen_book/
        |-LICENSE.md
        |-manifest.yml
        |-content/
            |-01/ <-- link points here

Going a step further we can link to a specific chunk

.. code-block:: none

    # link
    en/ulb/gen/book/01/01

    # file system
    en_ulb_gen_book/
        |-LICENSE.md
        |-manifest.yml
        |-content/
            |-01/
                |-01.usfm <-- link points here

In some of the examples above the link was not pointing directly at a file.
In those cases the link should resolve to the first available file in order of the sorting priority described in :ref:`structure-content-sort`.


External URLS
-------------

You may link to online media by simply using a url instead of an RC identifier.

- ``[[https://www.google.com]]``
- ``[Google](https://www.google.com)``

Links where the path begins with ``http://`` or ``https://`` are treated as external urls.

Examples
--------

book
~~~~

- ``[Genesis 1:2](en/ulb/gen/book/01/02)``
- ``[Open Bible Stories 1:2](/en/obs/obs/book/01/02)``

help
~~~~

- ``[[en/tq/gen/help/01/02]]`` - links to translationQuestions for Genesis 1:2
- ``[[en/tn/gen/help/01/02]]`` - links to translationNotes for Genesis 1:2

dict
~~~~

- ``[Canaan](en/tw/bible/dict/canaan)``

man
~~~

- ``[Translate Unknowns](en/ta-vol1/translate/man/translate-unknowns)``

img
~~~

- ``[Open Bible Stories 1:2](en/obs/obs/img/01/02)``
- ``[Genesus 1:2-6](en/ulb/gen/img/01/02)``

vid
~~~

- ``[Open Bible Stories 1:2](en/obs/obs/vid/01/02)``

audio
~~~~~

- ``[Open Bible Stories 1:2](en/obs/obs/audio/01/02)``

.. _linking-abbreviations:

Abbreviations
-------------

In certain cases it is appropriate to abbreviate a link.
Below are a list of cases where you are allowed to use an abbreviation.

Links within the same RC
~~~~~~~~~~~~~~~~~~~~~~~~

When linking to a different section within the same RC you may just provide the chapter/chunk :ref:`slug` s.

Manual example:

- ``[Translate Unknowns](translate-unknowns)``

Dictionary example:

- ``[Canaan](canaan)``

Book example:

- ``[Genesis 1:2](01/02)``

Links to any language
~~~~~~~~~~~~~~~~~~~~~

At times you may not wish to restrict the link to a partuclarl language of the RC.
In that case you may exclude the language code from the beginning of the path and place an extra slash ``/`` in it's place.

Example:

- ``[[//ta-vol1/translate/man/translate-unknowns]]``
- ``[Translate Unknowns](//ta-vol1/translate/man/translate-unknowns)``

.. _linking-bible-refs:

Automatically Linking Bible References
--------------------------------------

Bible references in any RC should be automatically converted into resolvable links according to the linking rules for **book** resource types. 
Of course, if the reference is already a link nothing needs to be done.

Conversion of biblical references are limited to those resources that have been indexed on the users' device.
Conversion should be performed based on any one of the following:

- a case *insensitive* match of the entire project title.
- a start case (first letter is uppercase) match of the project :ref:`slug` e.g. ``Gen``.

For each case above there must be a valid ``chapter:verse`` reference immediately after the matching word separated a single white space.
For example:

.. code-block:: none

    Genesis 1:1
    genesis 1:1
    Gen 1:1
    Gen 1:1-3

The chapter and verse numbers should be converted to properly formatted :ref:`slug` s.

Example
~~~~~~~

Given the French reference below:

``Genèse 1:1``

If the user has only downloaded the English resource the link will not resolve because the title ``Genesis`` or ``genesis`` does not match ``Genèse`` or ``genèse``.
Neither does the camel case :ref:`slug` ``Gen`` match since it does not match the *entire* word.

If the user now downloads the French resource the link will resolve because ``Genèse`` or ``genèse`` does indeed match ``Genèse`` or ``genèse``.
The result will be:

.. code-block:: markdown

    [Genèse 1:1](fr/ulb/gen/book/01/01)

Multiple Matches
~~~~~~~~~~~~~~~~

When a match occurs there may be several different resources that could be used in the link such as ``ulb`` or ``udb``.
When more than one resource :ref:`slug` is available use the following rules in order until a unique match is found:

1. use the same resource as indicated by the application context.
2. use the RC allowed by the translate_mode set in the application.
3. choose the first resource found or let the user choose (e.g. pop up).

Aligning Verses to Chunks
~~~~~~~~~~~~~~~~~~~~~~~~~

Because chunks may contain a range of verses, a passage reference may not exactly match up to a chunk.
Therefore some interpolation may be nessesary. For both chapter and verse numbers perform the follow:

    Given a chapter or verse number ``key``.
    And an equivalent sorted list ``list`` of chapters or verses in the matched resource 

- incrementally compare the key against items in the list.
- if the integer value of the current list item is less than the key: continue.
- if the integer value of the current list item is greater than the key: use the previous list item.
- if the end of the list is reached: use the previous list item.
  
For example chunk ``01`` may contain verses ``1-3`` whereas chunk ``02`` contains verses ``4-6``.
Therefore, verse ``2`` would resolve to chunk ``01``.

If no chapter or chunk can be found to satisfy the reference it should not be converted to a link.
