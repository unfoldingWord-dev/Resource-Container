.. _structure:

Resource Container Structure
============================

Resource containers (RCs) exist as directories.
They may be optionally compressed or packaged so long as the compressed file follows standard naming conventions for the file extension.  For example:

- a zipped RC would end in ``.zip``,
- a tarred RC would end in ``.tar``,
- a tarred+bzip2 RC would end in ``.tar.bz2``

A git repository is also a valid way to store RCs.

.. note:: When naming an RC directory or repository the best practice is to use a combination of the resource and
    project identifiers e.g. ``en-ulb-gen``.
    If the RC contains more than one project just use the resource identifier with an optional :ref:`identifier` formatted qualifier
    e.g. ``en-ulb-nt`` where ``nt`` is the custom qualifier denoting the New Testament.

.. _structure-directory:

Directory Structure
-------------------

RCs have a folder structure like the following:

.. code-block:: none

    my_resource_container/
        |-.git/
        |-.apps/
        |-LICENSE.md
        |-manifest.yaml
        |-media.yaml
        |-content/

- ``.git``: only exists when the RC is stored as a git repository.
- ``.apps``: is where applications can store custom meta data about the RC. See :doc:`app_meta` for more information.
- ``LICENSE.md``: contains the appropriate license information for the RC.
- ``manifest.yaml``: is the RC :ref:`manifest`.
- ``media.yaml``: contains definitions of :ref:`media` that has been generated from the content of the RC and where to find them.
- ``content``: contains the project files. The name of this directory is subject to the :ref:`manifest`.
  It is also possible for there to be multiple :ref:`Projects Directories <structure-content>` or be excluded altogether.

.. _structure-content:

Project Directory
-----------------

The project directory is where all of the translation information exists for a single project within an RC.
Files in this directory may contain either meta data or readable text.

.. code-block:: none

    content/
        |-config.yaml
        |-toc.yaml
        |-front/
        |-back/

The folders and files illustrated above are reserved.
Although not required, when used they must fulfill their roles as defined:

- ``config.yaml`` contains meta data for the project at varying levels of granularity as specified in the :ref:`types`.
- ``toc.yaml`` contains the :ref:`structure-toc`.
- ``front`` directory contains the front matter.
- ``back`` directory contains the back matter.

Furthermore, there are reserved chunk files which may exist in any folder
including the reserved `front` and `back` folders:

.. note:: we use ``md`` for the file extension in this example.
    You should use a file extension that is appropriate for content in your :ref:`Container Type <types>`.

.. code-block:: none

    content/
        ...
        |-front/
        |    |-title.md
        |    |-sub-title.md
        |    |-intro.md
        |    |-reference.md
        |    |-summary.md
        ...

Once again, these files are not required but must fulfill their roles as defined:

**when in a chapter**

- ``title.md`` - contains the chapter title
- ``sub-title.md`` - contains the sub title of the chapter
- ``intro.md`` - contains the introduction to the chapter
- ``reference.md`` - contains a reference displayed at the end of a chapter
- ``summary.md`` - contains a summary displayed at the end of a chapter

**when in "front"**

- ``title.md`` - contains the resource title
- ``sub-title.md`` - contains the sub title of the resource
- ``intro.md`` - contains the introduction to the resource
- ``reference.md`` - contains a reference displayed at the end of the front matter
- ``summary.md`` - contains a summary displayed at the end of the front matter

.. _condensed:

Condensed vs Expanded Form
^^^^^^^^^^^^^^^^^^^^^^^^^^

At times content can be structured slightly differently for added convenience.
If the :ref:`container type <types>` supports it an RC may use one form or the other or both simultaneously.

**Expanded**

All :ref:`types` support the expanded form.
This form is most suitable for active translations because collaborators are less likely to interfere with other's files.
And it decouples formatting from structure.
For example, here's a :ref:`structure-content` that has a chapter 1 folder containing several chunks:

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.md
        |    |-01.md
        |    ...
        |    |-reference.md
        |-02/
        ...


**Condensed**

Most :ref:`types` support a condensed form in which content in each chapter folder is stored
in a single file.
this form is most suitable for content being delivered as source text:

.. code-block:: none

    content/
        ...
        |-01.md
        |-02.md
        ...

Where the file ``01.md`` may contain the title, sub-title, intro, chunks, etc. for chapter 1.

.. _structure-content-sort:

Naming Chapters and Chunks
^^^^^^^^^^^^^^^^^^^^^^^^^^

When naming folders/files for chapters/chunks we *suggest* you zero pad the chapter or chunk to match the longest chapter or chunk in the given book.

For example:
In Psalms there are 150 chapters making the longest number contain 3 digits. So could create a chapter 1 folder as `001`.

Next, in Psalms the longest chapter is 119 which contains 176 verses (3 digits long). So we could create a verse 1 file as `001.md`.

In this manner we could create files for Psalm 1:1 as `001/001.md`. Doing this allows files to be sorted correctly when viewed in a web application such as git.door43.org.


Content Sort Order
^^^^^^^^^^^^^^^^^^

When utilizing content in an RC the order is very important.
The content sorting rules are defined as:

**Chapters**

1. front matter directory
2. numeric chapter directories sorted numerically in ascending order
3. non-numeric chapter directories sorted alphabetically
4. back matter directory

**Chunks**

1. title
2. sub-title
3. intro
4. numeric chunks sorted numerically in ascending order
5. non-numeric chunks sorted alphabetically
6. reference
7. summary

.. _structure-config:

Config
------

The ``config.yaml`` file contains information specific to each :ref:`RC type <types>`. If a particular :ref:`RC type <types>` does not need this file it may be excluded.

.. _structure-toc:

Table of Contents
-----------------

Chapter directories and chunk files are often named with padded integers.
A side effect of this is the natural file order often represents the appropriate order.
However, you may also indicate the order of chapters and frames by providing a table of contents, ``toc.yaml``, within the :ref:`structure-content`.

The table of contents is built with small blocks as shown below. All of the fields in the blocks are optional:

.. code-block:: yaml

    ---
      title: "My Title"
      sub-title: "My sub-title"
      link: "my-link"
      sections: []

- ``title`` a header in the table of contents
- ``sub-title`` a sub heading in the table of contents
- ``sections`` allow you to nest more blocks.
- ``link`` is the chapter :ref:`identifier` that should be linked to. Alternatively, you may provide a fully qualified link as defined in :ref:`linking`.

Here is an example ``toc.yaml`` from `translationAcademy <https://git.door43.org/Door43/en-ta>`_.
Generally speaking the title and sub-title fields in this file should be the same as what is found in the subsequently named chunks.
However, the TOC is allowed to deviate as necessary.

.. code-block:: yaml

    ---
    title: "Table of Contents"
    sections:
      - title: "1. Getting Started"
        sections:
          - title: "Introduction to the Process Manual"
            link: process-manual

      - title: "2. Setting Up a Translation Team"
        sections:
          - title: "Setting Up A Translation Team"
            link: setup-team

      - title: "3. Translating"
        sections:
          - title: "Training Before Translation Begins"
            link: pretranslation-training
          - title: "Choosing a Translation Platform"
            link: platforms
          - title: "Setting Up translationStudio"
            link: setup-ts

      - title: "4. Checking"
        sections:
          - title: "Training Before Checking Begins"
            link: prechecking-training
          - title: "How to Check"
            link: required-checking

      - title: "5. Publishing"
        sections:
          - title: "Introduction to Publishing"
            link: intro-publishing
          - title: "Source Text Process"
            link: source-text-process

      - title: "6. Distributing"
        sections:
          - title: "Introduction to Distribution"
            link: intro-share
          - title: "How to Share Content"
            link: share-content
