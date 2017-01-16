.. _structure:
Resource Container Structure
============================

.. _structure-format:
Resource Container Format
-------------------------

Resource containers (RCs) exist as directories.
They may be optionally compressed or packaged so long as the compressed file follows standard naming conventions for the file extension.
E.g. a zipped RC would end in ``.zip``,
a tarred RC would end in ``.tar``,
a tarred+bzip2 RC would end in ``.tar.bz2`` etc.
A git repository is also a valid way to store RCs.

.. _structure-identifier:
Identifier
----------

.. note:: RC Identifers present a number of issues that are difficult to overcome.
    Therefore this may be deprecated and a suggestion for nomenclature left in it's place.

RCs are uniquely identified by a composition of :ref:`slug` s delimited by underscores.
This composition is called the RC identifier.

Usage
^^^^^

The identifier is used as a unique name when:

* storing RCs on the disk
* representing multiple RCs in an API endpoint
* storing RC's in a repository on git.door43.org

The identifier is also a convenient way to identify an RC without inspecting it's :ref:`manifest`.
This can be helpful when searching for RCs through an API.

Example
^^^^^^^

The composition of :ref:`slug` s is illustrated below:

.. code-block:: none

    [IETF language code]_[resource slug]_[project slug]_[RC type slug]

For example, the ULB book of Genesis in Portuguese would be given in the following way:

.. code-block:: none

    pt-br_ulb_gen_book

Example
~~~~~~~

An example of using the file or directory name to identify an RC is illustrated below.
By viewing the file name we are able to quickly identify that this RC contains the ULB version of the book Genesis translated in English:

.. code-block:: none

    en_ulb_gen_book/

.. note:: Although convenient, the directory or file name should not be solely relied upon when determining the composition of an RC.
    Therefore, whenever possible you should consult the :ref:`manifest`.

.. _structure-directory:
Directory Structure
-------------------

RCs must use the following top level directory structure:

.. code-block:: none

    my_resource_container/
        |-.git/
        |-.apps/
        |-LICENSE.md
        |-manifest.yml
        |-content/

- ``.git`` is optional and is usually only seen in active translations.
- ``.apps`` is where applications can store custom meta data about the RC. See :doc:`app_meta` for more information.
- ``LICENSE.md`` contains the appropriate license information for the RC.
- ``manifest.yml`` is the :ref:`manifest` that contains meta data about the RC.
- ``content`` contains the project files inside the RC.
  There is nothing special about the name of this directory.
  Depending on how many projects are stored in the RC you may use a different name or use several different folders
  as defined in the :ref:`manifest`.

  - See below for the basic structure of the project directory/directories.
  - In the case of USFM RC types there will be no project directory. See the :ref:`manifest` for details.

.. _structure-content:

Content Directory
-----------------

The file and folder structure of the project content directory in RCs is mostly the same across RC types.  The structure is as follows:

.. code-block:: none

    content/
        |-config.yml
        |-toc.yml
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

Where a .txt extension is shown above, the proper extension should be used according to the content_mime_type indicated in the ``manifest.yml``.
For example ``.usfm`` or ``.md``.

The directories within ``content`` shown above indicate chapters.
The two reserved chapter names "front" and "back" are used to contain, if applicable, the front and back matter.
You may use any of the reserved names (e.g. "intro.txt") in the "front" and "back" directories.

The files within each chapter represent the chunks of the chapter.
Often the chunk file names will be numeric (e.g. 01.txt) but that is not required.
The following reserved chunk names have special meaning:

- ``title.txt`` - the title of the chapter
- ``sub-title.txt`` - the sub title of the chapter
- ``intro.txt`` - the introduction to the chapter
- ``reference.txt`` - a reference displayed at the end of a chapter
- ``summary.txt`` - a summary displayed at the end of a chapter

In the case of front and back matter, the above named chunks apply to the project. e.g. the project title, project summary etc.

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

The ``config.yml`` file contains information specific to each RC type. If a particular RC type does not need this file it may be excluded.

.. _structure-toc:
Table of Contents
-----------------

.. include:: includes/note_keys_required.txt

Chapter directories and chunk files are often named with padded integers.
A side effect of this is the natural file order often represents the appropriate order.
However, you may also indicate the order of chapters and frames by providing a table of contents toc.yml within the content directory.
If no such file exists then the integer order followed by the natural order of the files will be used.

The table of contents is built with small blocks as shown below. All of the fields in the blocks are optional:

.. code-block:: yaml

    ---
      title: "My Title"
      sub-title: "My sub-title"
      link: "my-link"
      sections: []

The sections field allows you to nest more blocks. The link fields may accept the chunk that should be linked to.
Alternatively you may provide a fully qualified link as defined in :ref:`linking`.

Here's an example toc.yml from translationAcademy.
Generally speaking the title and sub-titles fields in this file should be the same as what is found in the subsequently named chunks.
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
