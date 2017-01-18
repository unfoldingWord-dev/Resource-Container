.. _manifest:

Manifest File
=============

.. include:: includes/note_keys_required.txt

Resource Containers (RCs) have a manifest.yml that describes it's content and structure.
Terms from the [Dublin Core Meta Data Initiative](http://dublincore.org/documents/dcmi-terms/) have been used when appropriate.

.. note:: the ``language`` key below conflicts with the dublin core specification which indicates it should be a string.

.. code-block:: yaml

    ---
    rc_version: '0.2'
    rc_type: 'book'
    format: 'text/usfm'
    modified: '2015-12-22T12:01:30-05:00'

    language:
        identifier: 'en'
        title: 'English'
        direction: 'ltr'

    resource:
        identifier: 'ulb'
        title: 'Unlocked Literal Bible'
        translatable: 'yes'
        versification: 'kjv'
        checker:
        - 'Wycliffe Associates'
        checking_level: '3'
        version: '3'
        description: ''
        contributor:
        - 'Wycliffe Associates'
        issued: '2015-12-17'
        rights: 'CC BY-SA 4.0'
        source:
        -
            language: 'en'
            resource: 'asv'
            project: 'gen'
            version: '1901'

    projects:
        -
            identifier: 'gen'
            title: 'Genesis'
            description: ''
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
