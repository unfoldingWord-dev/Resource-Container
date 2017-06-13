.. _linking:

Linking
=======

A Resource Container (RC) link allows one RC to reference content from another RC.
Also, web links may be used to reference content online.

.. note:: Applications using RCs may want to search the RC data for external links and cache the online content
    so the RC can be used while offline without missing content such as images.


How links are written depends on the file format.
For example, a link within a markdown file would be displayed with the title in brackets and the url in parenthesis:

.. code-block:: markdown

    [Link Title](https://example.com)

In markdown we support an additional link form that provides a link without a title.
The title in these cases will be automatically generated from the context:

.. code-block:: markdown

    [[https://example.com]]

RC links follow the same form as web links in that they have a `scheme` and `uri`.

.. code-block:: markdown

    scheme://uri

.. _linking-scheme:

Scheme
------

RC links are identified by the ``rc`` schema in the same way that websites are identified by ``http``.
The scheme tells the software how to process the uri.

.. code-block:: markdown

    rc://uri


URI
---

The uri in an RC link is composed of the following components.

- **language** - an `IETF <https://en.wikipedia.org/wiki/IETF_language_tag>`_ compatible language tag indicating the language of the resource
- **resource** - an :ref:`identifier` for the resource
- **type** - an :ref:`identifier` for the :ref:`Container Type <types>`
- **project** - an :ref:`identifier` for the project

Any additional information you include must be added after those mentioned above.

.. code-block:: markdown

    rc://language/resource/type/project/extra/information

.. _linking-glob:

Wildcards
^^^^^^^^^

Some times it can be helpful to create a generic link.
Such as when referencing an entire resource like the English Unlocked Literal Bible.

To facilitate this RC links support a wildcard ``*`` that can be used in place of any component in the :ref:`uri`.

.. note:: If the wildcard occurs at the end of the link you can exclude it entirely.

.. code-block:: markdown

    rc://en/ulb/book/*
    # or
    rc://en/ulb/book

You can also do things like link to a book in any language

.. code-block:: markdown

    rc://*/ulb/book/gen

.. _linking-resolution:

Resolution
^^^^^^^^^^

An RC link is resolved like a file path.
The first few components address which RC to use.
And any remaining components address the specific content inside the RC.

This is illustrated below:

.. code-block:: none

    # link
    rc://en/ulb/bundle/exo

    # bundle RC on file system
    en_ulb_bundle/
        ...
        |-01-GEN.usfm
        |-02-EXO.usfm <=== the manifest will indicate that exo points here
        ...

From this point we can lengthen the link to include a chapter :ref:`identifier`.

.. note:: If the RC is a :ref:`types-bundle` the client application is responsible for understanding
    how to resolve to the chapter or any other location in the content.

.. code-block:: markdown

    # link
    rc://en/obs/book/obs/01

    # book RC on file system
    en_obs_book_obs/
        ...
        |-content/
        |   |-01/ <=== link points here
        |   ...
        ...

Going a step further we can link to a specific chunk

.. code-block:: none

    # link
    rc://en/obs/book/obs/01/01

    # file system
    en_obs_book_obs/
        ...
        |-content/
            |-01/
                |-01.md <=== link points here

In some of the examples above the link was pointing to a directory.
In those cases the link should resolve to the first available file in order of the sorting priority described in :ref:`structure-content-sort`.

.. note:: Depending on the client application, several files may be combined together when displayed to the user.
    For example: when linking to a chapter in a book of the Bible it would make more sense to show at least the title
    and summary, if not the rest of the chapter, rather than just the title.

Examples
^^^^^^^^

book
~~~~

- ``[Genesis 1:2](rc://en/ulb/book/gen/01/02)``
- ``[Open Bible Stories 1:2](rc://en/obs/book/obs/01/02)``

help
~~~~

- ``[[rc://en/tq/help/gen/01/02]]`` - links to translationQuestions for Genesis 1:2
- ``[[rc://en/tn/help/gen/01/02]]`` - links to translationNotes for Genesis 1:2

dict
~~~~

- ``[Canaan](rc://en/tw/dict/bible/other/canaan)``

man
~~~

- ``[Translate Unknowns](rc://en/ta/man/translate/translate-unknown)``

bundle
~~~~~~

- ``[Genesis](rc://en/ulb/bundle/gen/01/01)``

.. note:: Linking to a :ref:`types-bundle` will resolve down to the project level.
    The application will need to support parsing the bundle format (if references are supported) in order to continue
    resolving the link.

    Formats that support references are:

    - usfm
    - osis

.. note:: When using RCs with multiple projects the application will need to inspect the :ref:`manifest` to determine
    which :ref:`structure-content` to read while resolving a link.

.. _linking-abbreviations:

Abbreviations
-------------

In certain cases it is appropriate to abbreviate a link.
Below are a list of cases where you are allowed to use an abbreviation.

.. _linking-internal:

Internal Links
^^^^^^^^^^^^^^

When linking to a different section within the same RC
you may leave off the :ref:`linking-scheme` and simply provide a UNIX styled file path.
File extensions are optional.

.. note:: you can use either an absolute path such as ``/my/path`` where ``/`` is the root directory of the RC
    or relative path like ``../my/path``.

For example, let's say we have the following RC:

.. code-block:: none

    en-ta/
        ...
        |-intro/
        |      |-ta-intro/
        |      |         |-title.md
        |      |         |-sub-title.md
        |      |         |-01.md    <====== link from here
        |      |
        |      ...
        |-checking/
        |      |-acceptable/        <====== to here
        |      |         |-title.md
        |      |         |-sub-title.md
        |      |         |-01.md
        |      ...
        ...

With an internal link we can reference the "Acceptable Style" article
from within the "Introduction to translationAcademy" in any of the following ways:

.. code-block:: none

    [Acceptable Style](/checking/acceptable)
    [Acceptable Style](../../checking/acceptable)

Notice some times it's more readable to use an absolute path instead of a relative path.

A better use case for relative paths would be in tW using the :ref:`condensed form <condensed>`.

.. code-block:: none

    en-tw/
        ...
        |-bible/
        |      |-other/
        |      |      |-aaron.md
        |      |      |-moses.md
        |      |      ...
        |      ...
        ...

From within aaron.md we can link to moses in any of the following ways:

.. code-block:: none

    [Moses](moses)
    [Moses](moses.md)
    [Moses](./moses.md)
    [Moses](../other/moses.md)
    [Moses](/bible/other/moses.md)

.. note:: For compatibility with displaying in online services such as github we suggest including the file extension
    when practical and to use relative paths rather than absolute paths.

.. _short-links:

Short Links
^^^^^^^^^^^

A short link is used to reference a resource but not a project.
Short links are composed of just the language and resource.

- ``en/tn``

Short links are used within the :ref:`manifest` when referring to related resources.

.. _linking-bible-refs:

Bible References
^^^^^^^^^^^^^^^^

Bible references in any RC may be automatically converted into resolvable links according to the linking rules for **book** resource types.
Of course, if the biblical reference is already a link nothing needs to be done.

Conversion of biblical references are limited to those resources that have been indexed on the users' device.
Conversion should be performed if in the text either of the following conditions is satisfied:

- a case *insensitive* match of the entire project title. e.g. ``Genesis`` is found in the text.
- a start case (first letter is uppercase) match of the project :ref:`identifier` e.g. ``Gen``.

For each case above there must be a valid ``chapter:verse`` reference immediately after the matching word separated a single white space.
For example:

.. code-block:: none

    Genesis 1:1
    genesis 1:1
    Gen 1:1
    Gen 1:1-3

The chapter and verse numbers should be converted to properly formatted :ref:`identifiers <identifier>`.

Example
~~~~~~~

Given the French reference below:

``Genèse 1:1``

If the user has only downloaded the English resource the link will not resolve because the title ``Genesis`` or ``genesis`` does not match ``Genèse`` or ``genèse``.
Neither does the camel case :ref:`identifier` ``Gen`` match since it does not match the *entire* word.

If the user now downloads the French resource the link will resolve because ``Genèse`` or ``genèse`` does indeed match ``Genèse`` or ``genèse``.
The result will be:

.. code-block:: markdown

    [Genèse 1:1](rc://fr/ulb/book/gen/01/01)

Multiple Matches
~~~~~~~~~~~~~~~~

When a match occurs there may be several different resources that could be used in the link such as ``ulb`` or ``udb``.
When more than one resource :ref:`identifier` is available use the following rules in order until a unique match is found:

1. use the same resource as indicated by the application context.
2. use the RC allowed by the translate_mode set in the application.
3. choose the first resource found or let the user choose (e.g. pop up).

Aligning Verses to Chunks
~~~~~~~~~~~~~~~~~~~~~~~~~

Because chunks may contain a range of verses, a passage reference may not exactly match up to a chunk.
Therefore some interpolation may be necessary. For both chapter and verse numbers perform the follow:

    Given a chapter or verse number **key**.
    And an equivalent sorted list **list** of chapters or verses in the matched resource

- incrementally compare the **key** against items in the **list**.
- if the integer value of the current **list** item is less than the **key**: continue.
- if the integer value of the current **list** item is greater than the **key**: use the previous item in the **list**.
- if the end of the **list** is reached: use the previous item in the **list**.
  
For example chunk ``01`` may contain verses ``1-3`` whereas chunk ``02`` contains verses ``4-6``.
Therefore, verse ``2`` would resolve to chunk ``01``.

If no chapter or chunk can be found to satisfy the reference it should not be converted to a link.
