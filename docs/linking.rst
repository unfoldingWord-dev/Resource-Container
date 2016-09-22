Linking
=======

A resource link provides navigable directions to a Resource Container.

All resource links follow a very simple structure in two different flavors: Anonymous and Titled. The link path is the Resource Container slug where the slug delimiter is replaced by a slash ``/`` character.

Links may include arguments as required by the resource type. Arguments must be specified at the end of a link and separated from the link path by a slash as well.

Anonymous Link
---------------

These links have no title and are declared by enclosing the link in double brackets

``[[language/project/resource]]``

Titled Link
-----------

These links have a title and are indicated by enclosing the link title in single brackets and the link in parentheses.

.. code-block:: markdown

    [Link Title](language/project/resource)

Shorthand Links
---------------

Shorthand links may be used when the resource slug matches it's project slug.
For example:

``[[en/obs/obs/01:02]]`` could be written as ``[[en/obs/01:02]]``.

Shorthand links may only be used when linking to a passage in a book or linking to a resource as a whole. i.e.

- ``[[en/obs/01:02]]`` Links to OBS 1:2
- ``[[en/obs]]`` Links to OBS

External URLS
-------------

You may link to online media by simply using a url instead of a resource container id.

- ``[[https://www.google.com]]``
- ``[Google](https://www.google.com)``

Links where the path begins with ``http://`` or ``https://`` are treated as external urls.

Arguments
---------

Some Resource Containers can accept or require additional arguments in the link. These are described below. For more information about these types please see Resource Containers.

book
~~~~

These types accept an additional chapter and verse parameter formatted as is common in Bible passages.

Here are some examples:

- ``01:02`` verse two in chapter one
- ``01:02-04`` verses two through four in chapter one. This is allowed for convenience. It will resolve as ``01:02``.
- ``02`` chapter two. This will resolve to the first chunk of the chapter e.g. ``02:title``. NOTE: the first chunk is not necessarily the first verse. Additional front matter may exist.

Please note that the values in these arguments are not digits but slugs to chapters and chunks. Therefore care must be taken to not use them as digits. For example ``1:2`` will not resolve because there is no chapter ``1`` or chunk ``2``. Because these are just slugs we may link to other elements such as chapter titles or even project titles.

- ``01:title`` the title of chapter one
- ``title`` the title of the book

Links must always be direct therefore you may not indicate multiple ranges of passages.

- ``01:02,04`` **this is incorrect and will not resolve**

Complete Examples:

- ``[[en/obs/obs/01:02]]``
- ``[Open Bible Stories 1:2](en/obs/obs/01:02)``
- ``[[en/gen/ulb/01:02-06]]``
- ``[Genesus 1:2-6](en/gen/ulb/01:02-06)``

help
~~~~

See arguments for **book**. Differences are described below.

When linking to a help you must always link to a chapter + chunk combination. You cannot link to just a chapter.

dict
~~~~

Accepts a single dictionary term id as an argument. For example:

- ``aaron``
- ``abel``
- ``canaan``

Complete Examples:

- ``[[en/bible/tw/canaan]]``
- ``[Canaan](en/bible/tw/canaan)``

man
~~~~

Accepts a single module id as an argument For example:

- ``translate-unknowns``

Complete examples:

- ``[[en/ta-translate/vol1/translate-unknowns]]``
- ``[Translate Unknowns](en/ta-translate/vol1/translate-unknowns)``

Abbreviations
-------------

In certain cases it is appropriate to abbreviate a link. Below are a list of cases where you are allowed to use an abbreviation.

Links within the same resource
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When linking to a different part of the same resource you may just provide the arguments.

Example from tA Translate resource:

- ``[[translate-unknowns]]``
- ``[Translate Unknowns](translate-unknowns)``

Example from tW resource

- ``[[canaan]]``
- ``[Canaan](canaan)``

Links to any translation of a resource
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some times you may not wish to restrict the linked resource to a particular language. In that case you may exclude the language code from the beginning of the path and place an extra slash ``/`` in it's place.

Example:

- ``[[//ta-translate/vol1/translate-unknowns]]``
- ``[Translate Unknowns](//ta-translate/vol1/translate-unknowns)``

Automatically Linking Bible References
--------------------------------------

Bible references in any resource container should be automatically converted into resolvable links according to the linking rules for **book** resource types. Of course, if the reference is already a link nothing needs to be done.

Conversion of biblical references are limited to those resources that have been indexed on the users' device. Conversion should be performed based on any one of the following:

- a case *insensitive* match of the entire project title.
- a case *sensitive* match of the project slug where the first character is uppercase e.g. ``Gen``.

For each case above there must be a valid ``chapter:verse`` reference immediately after the matching word separated only by white space. For example:

.. code-block:: none

    Genesis 1:1
    genesis 1:1
    Gen 1:1
    Gen 1:1-3
    gen 1:1 -- not valid

If the user clicks on one such generated link where the resource container has not yet been downloaded they should be asked if they would like to download it. After downloading the resource container they should immediately follow the link.

Example
~~~~~~~

Given the French reference below:

``Genèse 1:1``

If the user has only downloaded the English resource the link will not resolve because the title ``Genesis`` or ``genesis`` does not match ``Genèse`` or ``genèse``. Neither does the camel case slug ``Gen`` match since it does not match the *entire* word.

If the user now downloads the French resource the link will resolve because ``Genèse`` or ``genèse`` does indeed match ``Genèse`` or ``genèse``. The result will be:

.. code-block:: markdown

    [Genèse 1:1](fr/gen/ulb/01:01)

When a match occurs there may be several different resources that could be used in the link such as ``ulb`` or ``udb``. When more than one resource slug is available use the following rules in order until a solution is found:

1. choose the first resource that has a translate_mode of 'all'.
2. choose the first resource that has a translate_mode of 'none.
3. choose the first resource found.

Care must be taken when formatting the chapter and chunk slugs. You must not assume a chapter is padded with a single ``0`` and likewise for chunks. When preparing the link you should attempt to compare the integer values found in the text with the chapter and verse slugs (this time parsed as integers) in order to identify the correct chapter and chunk.

Because chunks may contain a range of verses some judgment is required to determine if a verse resides within a chunk. For example chunk ``01`` may contain verses ``1-3`` whereas chunk ``02`` contains verses ``4-6``.

If no chapter or chunk can be found to satisfy the reference it should not be converted to a link.

Media Links
-----------

Media types are described in Resource Containers. Media defined in such a way is accessible to not only the resource containing them but any resource that links to it.

In order to use a media link you need only pre-pend a link with the media type as indicated below (this assumes a media type ``image`` exists for both of these resources):

- ``[[image:en/obs/obs/01:02]]``
- ``[[image:en/gen/ulb/01:02]]``

> NOTE: if a media link is titled the title will be used as the alt text.

You may notice a striking similarity between the media links shown above and their accompanying passage links show below:

- ``[[en/obs/obs/01:02]]``
- ``[[en/gen/ulb/01:02]]``

Media links are handled in exactly the same way as a normal link except that the arguments are used to reference the correct media file.

The similarity seen above is not required neither will this always be the case since the media files may be named whatever you wish (while adhering to the requirements in Resource Containers). However, it is strongly encouraged where appropriate since it makes the creation of content much easier.

