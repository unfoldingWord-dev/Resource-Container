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
    If the RC contains more than one project just use the resource identifier with an optional :ref:`slug` formatted qualifier
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
        |-content/

- ``.git``: only exists when the RC is stored in a git repository.
- ``.apps``: is where applications can store custom meta data about the RC. See :doc:`app_meta` for more information.
- ``LICENSE.md``: contains the appropriate license information for the RC.
- ``manifest.yaml``: is the RC :ref:`manifest`.
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

.. note:: we use ``ext`` to indicate a wildcard file extension.
    You should use a file extension that is appropriate for content in your :ref:`Container Type <types>`.

.. code-block:: none

    content/
        ...
        |-front/
        |    |-title.ext
        |    |-sub-title.ext
        |    |-intro.ext
        |    |-reference.ext
        |    |-summary.ext
        ...

Once again, these files are not required but must fulfill their roles as defined:

**when in a chapter**

- ``title.txt`` - contains the chapter title
- ``sub-title.txt`` - contains the sub title of the chapter
- ``intro.txt`` - contains the introduction to the chapter
- ``reference.txt`` - contains a reference displayed at the end of a chapter
- ``summary.txt`` - contains a summary displayed at the end of a chapter

**when in "front"**

- ``title.txt`` - contains the resource title
- ``sub-title.txt`` - contains the sub title of the resource
- ``intro.txt`` - contains the introduction to the resource
- ``reference.txt`` - contains a reference displayed at the end of the front matter
- ``summary.txt`` - contains a summary displayed at the end of the front matter

.. _condensed:

Condensed vs Expanded Form
^^^^^^^^^^^^^^^^^^^^^^^^^^

Although not officially part of this specification, we offer two suggested forms for storing content.

**Expanded**

All :ref:`types` support the expanded form.
For example, here's :ref:`structure-content` that has a chapter 1 folder containing several chunks:

.. code-block:: none

    content/
        ...
        |-01/
        |    |-title.md
        |    |-01.md
        |    ...
        |    |-reference.txt
        ...


**Condensed**

Most :ref:`types` support a condensed form in which content in each folder is stored
in a single file:

.. code-block:: none

    content/
        ...
        |-01/
        |    |-01.txt
        ...

Where the file ``01.txt`` may contain the title, sub-title, intro, chunks etc. formatted appropriately so it can
be read by a client application.

.. _structure-content-sort:

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

.. include:: includes/note_keys_required.txt

Chapter directories and chunk files are often named with padded integers.
A side effect of this is the natural file order often represents the appropriate order.
However, you may also indicate the order of chapters and frames by providing a table of contents, ``toc.yaml``, within the content directory.
If no such file exists then the integer order followed by the natural order of the files will be used.

The table of contents is built with small blocks as shown below. All of the fields in the blocks are optional:

.. code-block:: yaml

    ---
      title: "My Title"
      sub-title: "My sub-title"
      link: "my-link"
      sections: []

The sections field allows you to nest more blocks. The link fields may accept the chunk that should be linked to.
Alternatively, you may provide a fully qualified link as defined in :ref:`linking`.

Here is an example ``toc.yaml`` from `translationAcademy <https://git.door43.org/Door43/en-ta>`_.
Generally speaking the title and sub-title fields in this file should be the same as what is found in the subsequently named chunks.
However, the TOC is allowed to deviate as necessary.

.. code-block:: yaml

    ---
      title: "translationAcademy Table of Contents"
      sub-title: ""
      link: ""
      sections: 
        - 
          title: "Introduction to translationAcademy"
          sub-title: "This page answers the question: What is in the Introduction?"
          link: ""
          sections: 
            - 
              title: "Introduction"
              sub-title: ""
              link: ""
              sections: 
                - 
                  title: ""
                  sub-title: ""
                  link: "ta-intro"
                  sections: []
                - 
                  title: ""
                  sub-title: ""
                  link: "uw-intro"
                  sections: []
        - 
          title: "Table Of Contents - Process Manual Vol 1"
          sub-title: "This page answers the question: What is in the process manual volume 1?"
          sections: 
            - 
              title: "Process Manual Volume 1"
              sub-title: ""
              link: ""
              sections: 
                - 
                  title: "1. Getting Started"
                  sub-title: ""
                  link: ""
                  sections: 
                    - 
                      title: ""
                      sub-title: ""
                      link: "process-manual"
                      sections: []
                    - 
                      title: ""
                      sub-title: ""
                      link: "getting-started"
                      sections: []
                - 
                  title: "2. Setting Up a Translation Team"
                  sub-title: ""
                  link: ""
                  sections: 
                    - 
                      title: ""
                      sub-title: ""
                      link: "setup-team"
                      sections: []
        - 
          title: "Table Of Contents - Translation Manual Volume 1"
          sub-title: "This page answers the question: What is in Volume 1 of the translation manual?"
          sections: []

Alternatively you can choose to use a simplified table of contents as shown below.

.. note:: We may deprecate this form due to the addition of content sorting instructions describe above.
  Since sorting is defined this form may not provide anything useful.

.. code-block:: yaml

    ---
    -
        chapter: '01'
        chunks:
            - title
            - '01'
            - '02'
            - '03'
            - '04'
            - '05'
            - '06'
            - '07'
            - '08'
            - '09'
            - '10'
            - '11'
            - '12'
            - '13'
            - '14'
            - '15'
            - '16'
            - reference
    -
        chapter: '02'
        chunks:
            - title
            - '01'
            - '02'
            - '03'
            - '04'
            - '05'
            - '06'
            - '07'
            - '08'
            - '09'
            - '10'
            - '11'
            - '12'
            - reference

The simple version will rely on the available content (titles, references, etc.) to generate the table of contents
(readable titles will be retrieved from the content itself).
