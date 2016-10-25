Resource Container Structure
============================

Container Format
----------------

A resource container may exist in two forms:

- a compressed archive with the extension .tsrc as in: some_resource_container.tsrc
- a directory as in: some_resource_container/

In both cases the internal structure is exactly the same. Compressing a resource container and changing the file extension to .tsrc simply makes the resource container portable.

Slug
----

The resource container slug is primarily used to create unique file names for storing resource containers on the disk. In the case of creating a translation this is also the name of the online repository where the resource container is uploaded on Door43. These slugs are also used when linking from one resource container to another. See Resource Container Links for more information about linking.

The slug is also a convenient way to identify a resource container without opening it to inspect the package.json. This can be helpful when searching for resource containers through an API that only returns the resource container slugs.

Slugs are structured as indicated below. Each component of the slug is delimited by an underscore _.

.. code-block:: none

    [language slug]_[project slug]_[resource slug]

Each component of the slug may include alphanumeric characters and dashes -. See Resource Types for a list of valid resource types that may be used in the slug.

NOTE: slug components may not contain any underscores _ since this is the delimiter.

An example of using the slug to identify a resource container is illustrated below. By viewing the file name we are able to quickly identify that this resource container contains the ULB version of the book Genesis translated in English:

.. code-block:: none

    en_gen_ulb.tsrc

As mentioned above, the slug is not the strict authority regarding the nature of the resource container. Therefore whenever a resource container is utilized the package.json should always be consulted and has the final word. This is important since a user could change the name of the file between exporting it and sharing it with someone else.

In order to correctly generate the slug of a resource container you must consult the package.json which contains all of the necessary information.



Directory Structure
-------------------

Resource containers may be zip archives with the .tsrc (translationStudio) extension, or directories.

.. code-block:: none

    my_resource_container.tsrc/
        |-.git/
        |-LICENSE.md
        |-package.json
        |-content/

- the .git directory is optional and is usually only seen in active translations.
- LICENSE.md contains the appropriate license information for the resource container.
- package.json contains meta data about the resource container.
- the content directory contains any other files needed by the resource type.



Content
-------

The folder structure of the content directory in resource containers are mostly the same. Differences between resource types may include the absense of some files or the inclusion of others.

Note: that where a .txt extension is shown below, the proper extension should be used according to the content_mime_type indicated in the package.json. For example .usfm or .md.

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

The directories shown above indicate chapters except for the two reserved folders `front` and `back` which contain, if applicable, the front matter and back matter of the container.

The files within each chapter represents the chunks of the chapter. Within each folder are additional reserved files:

- title
- sub-title
- intro
- reference
- summary



Config
------

The `config.yml` file contains information specific to the resource type. However, there is a reserved field `media` which allows different media to be assoicated with the resource container.

.. code-block:: none

    ---
      media:
        image: 
            mime_type: "image/jpeg"
            size: 37620940
            url: "https://cdn.door43.org/en/obs/v3/jpg/360px.zip"
        image_large: 
            mime_type: "image/jpeg"
            size: 807010466
            url: "https://cdn.door43.org/en/obs/v3/jpg/2160px.zip"
        single_image: 
            mime_type: "image/jpeg"
            size: 80701
            url: "https://cdn.door43.org/en/obs/v3/jpg/01_01.jpg"

In the above example there are three different media types:

- image
- image_large
- single_image

These media types are utilized via Resource Container Links .

The `url` can point to either a single media file or a zip archive which contains many pieces of media.
The downloaded media files themselves can be named whatever you want so long as they adhere to the naming conventions for slugs.

If media is served as a zip archive the archive should contain appropriately named media files which may optionally be organized within folders also appropriately named.

.. code-block:: none

    my_media.zip/
        |-01/
        |   |-01.jpg
        |   |-02.jpg
        |
        |-02/
        |   |-01.jpg
        |   |-02.jpg
        ...

If you want to provide hierarchy to media files in a zip archive without using folders you may use an underscore _ to delimit the slugs.

.. code-block:: none

    my_obs_media.zip/
        |-01_01.jpg
        |-01_02.jpg
        ...

Implementation Notes:
When downloaded, the media should be stored in a central location where each media type is stored under a folder named according to it's type. e.g. /media/image_large.
The examples above deal only with images, however it is possible to reference other media formats including video or audio content. For more information about how to use media types see Resource Container Links .



Table of Contents
-----------------

Chapter directories and chunk files are often named with padded integers. A side effect of this is the natural file order often represents the appropriate order. However, you may also indicate the order of chapters and frames by providing a table of contents toc.yml within the content directory. If no such file exists then the integer order followed by the natural order of the files will be used.

The table of contents is built with small blocks as shown below. All of the fields in the blocks are optional:

.. code-block:: yaml

    ---
      title: "My Title"
      sub-title: "My sub-title"
      link: "my-link"
      sections: []

The sections field allows you to nest more blocks. The link fields may accept the chunk that should be linked to. Alternatively you may provide a fully qualified link as defined in Resource Container Links.

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

The simple version will rely on the available content (titles, references, etc.) to generate the table of contents. i.e. Readable titles will be retrieved from the content itself.
