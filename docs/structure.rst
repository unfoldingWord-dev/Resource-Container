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
  It is also possible for there to be multiple directories or excluded altogether.

.. _structure-content:

Project Directory
-----------------

The folder structure of the project directory in RCs is mostly the same across RC types.
The most common structure is indicated below:

.. code-block:: none

    content/
        |-config.yaml
        |-toc.yaml
        |-front/
        |-01/
        |    |-title.txt
        |    |-sub-title.txt
        |    |-intro.txt
        |    |-01.txt
        |    |-02.txt
        |    ...
        |    |-reference.txt
        |    |-summary.txt
        ...
        |-back/

.. note:: Where a .txt extension is shown above, the proper extension should be used according to the format
    indicated in the :ref:`manifest`. For example ``.usfm`` or ``.md``.

The directories within ``content`` shown above indicate chapters.
There are two special chapters named ``front`` and ``back`` that contain, if applicable, the front and back matter.

The files within each chapter represent the chunks of the chapter.
Often the chunk file names will be numeric (e.g. ``01.txt``) but that is not required.
The following reserved chunk names have special meaning:

- ``title.txt`` - the title of the chapter
- ``sub-title.txt`` - the sub title of the chapter
- ``intro.txt`` - the introduction to the chapter
- ``reference.txt`` - a reference displayed at the end of a chapter
- ``summary.txt`` - a summary displayed at the end of a chapter

In the case of front and back matter, the above named chunks apply to the project, such as the project title, project summary, etc.

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

The ``config.yaml`` file contains information specific to each RC type. If a particular RC type does not need this file it may be excluded.

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
