Resource Types
==============

Content can be displayed in different forms and used in different ways within the apps. Therefore several content types exist to manage different needs.

All resource types, unless otherwise specified, may employ any translation mode. Translation modes operate on a resource by resource basis and do not apply globally to all of the same type. For example: Organization X may create a help container that is not translatable. However, organization Y may also create a help container that is translatable.

Also note that where a .txt extension is shown below, the proper extension should be used according to the format indicated in the package.json. For example .usfm or .md.

The folder structure of containers are mostly the same. Differences between types may include the absense of some files or the inclusion of others. However, the folder-file heierarchy and reserved names apply in all cases.

.. code-block:: none

    content/
        |-config.yml
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

The directories shown above indicate chapters except for the two reserved folders front and back which contain, if applicable, the front matter and back matter of the container.

The files within each chapter represents the chunks of the chapter. Within each folder are additional reserved files title, sub-title, intro, reference and summary.

NOTE: With regard to Resource Container Links, it is currently undecided whether linking to reserved directories (chapters) or files (chunks) should be allowed.


Media
-----

The config.yml file contains information specific for the resource container. There is a reserved field media which allows different media to be assoicated with the resource

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

The url can point to either a single media file or a zip archive which contains many pieces of media.
The downloaded media files themselves can be named whatever you want so long as they adhere to the naming conventions for slugs as described above.

If media is served as a zip archive the archive should contain appropriately named media files which may optionally be organized within folders also appropriately named.

.. code-block:: none

    my_media.zip/
        |-media01.jpg
        |-media02.jpg
        ...
        |-dir01/
            |-media01.jpg

If you want to provide hierarchy to media files in a zip archive without using folders you may use an underscore _ to delimit the slugs.

.. code-block:: none

    my_obs_media.zip/
        |-01_01.jpg
        |-01_02.jpg

In the example above we have defined the accompanying images for the first two chunks in OBS.

Implementation Notes:
When downloaded, the media should be stored in a central location where each media type is stored under a folder named according to it's type. e.g. /media/image_large.
The examples above deal only with images, however it is possible to reference other media formats including video or audio content. For more information about how to use media types see Resource Container Links .

Book
----

Represents any text that is structured like a book. e.g. there are chapters and chunks.

Chapter directories and chunk files are often named with padded integers in order to provide the proper order of content. However, you may also indicate the order of chapters and frames by providing a table of contents toc.yml within the content directory. If no such file exists then the integer order followed by the natural order of the files will be used.

.. code-block:: none

    content/
        |-toc.yml
        ...

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
Related resource as shown above may be displayed along side the book content in order to provide contextual information.

The table of contents built with small blocks as shown below. All of the fields in the blocks are optional:

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

The simple version will rely on the available content (titles, references, etc.) to generate the table of contents.

Help
----

A helpful resource to supplement chunks in a book. e.g. notes or questions. For now all help resources must use the markdown format.

Each chunk contains one or more helps which correlate to the corresponding chunk in a book resource:

.. code-block:: none

    #In the beginning God created

    This introductory statement gives a summary of the rest of the chapter. AT: "This is about how God made...in the beginning." Some languages translate it as "A very long time ago God created." Translate it in a way that that shows that this actually happened and is not just a folk story.

    #In the beginning

    This refers to the start of the world and everything in it.

When parsed by an app the helps in this chunk are split at the headers. If there is preceding text (without a header) it will be displayed as a single help and a short snippet of the text will be used for the header if applicable.


Dictionary
----------

A standalone dictionary of terms. For now all dictionary resources must use the markdown format.

Rather than using interger styled names for folders as seen in other resource types the dictionary term is used as the chapter name. The translation of the term is placed inside a single 01.txt file:

.. code-block:: none

    content/
        |-config.yml
        |-aaron/
        |    |-01.txt
        |
        |-abel/
        ...
        |-unclean/

NOTE: if desired the dictionary terms may be split into more than one chunk.

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
