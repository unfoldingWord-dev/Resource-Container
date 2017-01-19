.. _manifest:

Manifest File
=============

.. include:: includes/note_keys_required.txt

Resource Containers (RCs) have a manifest.yml that describes it's content and structure.
Terms from the `Dublin Core Meta Data Initiative <http://dublincore.org/documents/dcmi-terms>`_ have been used when appropriate.

.. code-block:: yaml

    ---

    dublin_core:
        identifier: 'en-ulb'
        language:
            identifier: 'en'
            title: 'English'
            direction: 'ltr'
        type: 'book'
        title: 'Unlocked Literal Bible'
        format: 'text/usfm'
        source:
        -
            language: 'en'
            identifier: 'en-asv'
            version: '1901'
        rights: 'CC BY-SA 4.0'
        creator: 'Wycliffe Associates'
        subject: 'Bible translation'
        description: 'The Unlocked Literal Bible is an open-licensed version of the Bible that is intended to provide a form-centric translation of the Bible.'
        publisher: 'Door43'
        contributor:
            - 'NamesOfContributors'
        relation:
            - 'en-udb'
            - 'en-tn'
        version: '3'
        issued: '2015-12-17'
        modified: '2015-12-22T12:01:30-05:00'
        conformsto: 'rc0.2'

    checking:
        checking_entity:
        - 'Wycliffe Associates'
        checking_level: '3'

    projects:
        -
            identifier: 'gen'
            title: 'Genesis'
            versification: 'kjv'
            sort: 1
            path: './content'
            categories:
            - 'bible-ot'

.. _manifest-definitions:

Definitions
-----------

- ``rc.version``: the version of the RC.
- ``rc.type``: the type of RC being represented.
- ``rc.identifier``: the shortname of this RC, which should also be the directory or filename (e.g. en_ulb_gen.zip).
- ``content_mime_type``: the format of the content that is being stored inside the RC. e.g.:

  - text/usfm
  - text/markdown
  - image/png
  - audio/mp3
  - video/mp4
  
- ``language``: the language information of the text inside this RC.
- ``projects``: an array of projects inside the RC. Each object is a project definition inside the RC.

  - ``rc.path``: the path to the project within the RC. This will be a directory or in some cases a file. In all of our
    examples we will use the directory `content`.

- ``resource > status``: the translation status of the resource.
- ``resource > status > translate_mode``: The mode in which the resource may be translated.

  - yes: it can always be translated.
  - gateway: it can only be translated when gateway language mode is activated in the app.
  - no: it can never be translated.

- ``versification``: indicates the versification system used to defined chunks in this RC. See Resource Catalog and Chunk Markers for more information.

.. _manifest-changing-lang:

Changing Language & Resource
----------------------------

Some times it is desirable to change certain the language and resource of a RC. For example a mistake may have been made when originally defining the RC.

Care should be taken since changing the language and/or resource will also change the identifier of the RC.

Implementation Notes
~~~~~~~~~~~~~~~~~~~~

If while changing either of these properties the RC will conflict with an existing RC (on the current file system) the user should be asked if they would like to merge the two RC or cancel the change. See Merging RCs for more information about merging.

In order to fully change either of these properties the following steps must be taken

1. change the value of the property in the ``manifest.yml``.
2. update the name of the RC folder to the new identifier.
3. update any external usages of the RC's identifier to the new identifier.
