.. _structure:
Resource Container Structure
============================

.. _structure-format:
Container Format
----------------

Resource containers exist as directories. They may be optionally compressed or packaged so long as the compressed file follows standard naming conventions for the file extension. E.g. a zipped resource container would end in `.zip`, a tarred resource container would end in `.tar`, a tarred+bzip2 resource container would end in `.tar.bz2` etc.  A git repository is also a valid way to store Resource Containers.

.. _structure-slug:
Container Slug
--------------

The `container_slug` provides a unique name for the given resource container.

Usage
~~~~~

The `container_slug` is used to create unique file names for storing resource containers on the disk or when a unique name is needed for displaying multiple projects at the same level (like an API endpoint). These are also the names of the repositories where the resource container is uploaded to on git.door43.org. These `container_slugs` are used when linking from one resource container to another. See :ref:`linking` for more information about linking.

The `container_slug` is also a convenient way to identify a resource container without opening it to inspect the `package.json`. This can be helpful when searching for resource containers through an API that only returns the resource container slugs.

Structure
~~~~~~~~~

Container slugs are structured as indicated below. Each component of the `container_slug` is delimited by an underscore (_).

.. code-block:: none

    [IETF language code]_[resource slug]_[project slug]_[container type]

Each component of the `container_slug` may include alphanumeric characters and dashes (-). The `container_slug` components may not contain any underscores (_) since this is the delimiter.

See :ref:`types` for a list of valid container types that may be used in the slug.

Example
~~~~~~~

An example of using the file or directory name to identify a resource container is illustrated below. By viewing the file name we are able to quickly identify that this resource container contains the ULB version of the book Genesis translated in English:

.. code-block:: none

    en_ulb_gen_book/

.. note:: The directory or file name is not the strict authority regarding the nature of the resource container. Therefore, whenever a resource container is utilized the `container_slug` field in the `package.json` manifest file should always be consulted and has the final word.


.. _structure-directory:
Directory Structure
-------------------

Resource containers must use the following top level directory structure:

.. code-block:: none

    my_resource_container/
        |-.git/
        |-LICENSE.md
        |-package.json
        |-content/

- the .git directory is optional and is usually only seen in active translations.
- LICENSE.md contains the appropriate license information for the resource container.
- `package.json` is the manifest file that contains meta data about the resource container.
- the `content` directory contains any other files needed by the container type, including the content itself.

  - See below for the basic structure of this directory
  - A mime type of `text/usfm` is allowed to omit the `content` directory in order to conform to the USFM standard.  For example, this is acceptable:

.. code-block:: none

    usfm_resource_container/
        |-.git/
        |-01-GEN.usfm
        |-02-EXO.usfm
        |-...
        |-LICENSE.md
        |-package.json


.. _structure-content:
Content Directory
-----------------

The file and folder structure of the content directory in resource containers is mostly the same across container types.  The structure is as follows:

.. code-block:: none

    content/
        |-config.yml
        |-toc.yml
        |-01/
        |    |-title.txt
        |    |-sub-title.txt
        |    |-intro.txt
        |    |-reference.txt
        |    |-summary.txt
        |    |-01.txt
        |    |-02.txt
        |    ...
        ...
        |-front/
        |-back/
        ...

Where a .txt extension is shown above, the proper extension should be used according to the content_mime_type indicated in the `package.json`. For example `.usfm` or `.md`.

The directories shown above indicate chapters. The two reserved chapter names "front" and "back" are used to contain the front and back matter, if applicable. You may use any of the named files (e.g. "intro.txt") in the "front" or "back" directories.

The files within each chapter represents the chunks of the chapter. Often the chunk file names will be numeric (e.g. 01.txt) but that is not required. The following chunk names have special meaning:

- `title.txt` - the title of the chapter
- `sub-title.txt` - the sub title of the chapter
- `intro.txt` - the introduction to the chapter
- `reference.txt` - a reference displayed at the end of a chapter
- `summary.txt` - a summary displayed at the end of a chapter

In the case of front and back matter, the above named chunks apply to the project. e.g. the project title, project summary etc.


.. _structure-config:
Config
------

The `config.yml` file contains information specific to each container type. If a particular container type does not need this file it may be excluded.

.. _structure-toc:
Table of Contents
-----------------

.. include:: includes/note_keys_required.txt

Chapter directories and chunk files are often named with padded integers. A side effect of this is the natural file order often represents the appropriate order. However, you may also indicate the order of chapters and frames by providing a table of contents toc.yml within the content directory. If no such file exists then the integer order followed by the natural order of the files will be used.

The table of contents is built with small blocks as shown below. All of the fields in the blocks are optional:

.. code-block:: yaml

    ---
      title: "My Title"
      sub-title: "My sub-title"
      link: "my-link"
      sections: []

The sections field allows you to nest more blocks. The link fields may accept the chunk that should be linked to. Alternatively you may provide a fully qualified link as defined in :ref:`linking`.

Here's an example toc.yml from translationAcademy. Generally speaking the title and sub-titles fields in this file should be the same as what is found in the subsquently named chunks. However, the TOC is allowed to deviate as nessesary.

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

The simple version will rely on the available content (titles, references, etc.) to generate the table of contents (readable titles will be retrieved from the content itself).
