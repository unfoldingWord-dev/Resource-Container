.. _manifest:

Manifest File
=============

.. include:: includes/note_keys_required.txt

Resource Containers (RCs) have a manifest.yml that describes it's content and structure.

.. code-block:: yaml

    ---
    rc.version: '0.2'
    rc.type: 'book'
    rc.identifier: 'en_ulb_gen_book'

    content_mime_type: 'text/usfm'
    modified_at: 20151222120130
    versification: 'kjv'

    language:
      ietf: 'en'
      name: 'English'
      dir: 'ltr'

    project:
      slug: 'gen'
      name: 'Genesis'
      desc: ''
      icon: ''
      sort: 1
      categories:
      - 'bible-ot'

    resource:
      slug: 'ulb'
      name: 'Unlocked Literal Bible'
      translate_mode: 'all'
      status:
        checking_entity:
        - 'Wycliffe Associates'
        checking_level: '3'
        version: '3'
        comments: ''
        contributors:
        - 'Wycliffe Associates'
        pub_date: '2015-12-17'
        license: 'CC BY-SA 4.0'
        source_translations:
        - rc.identifier: 'en_asv_gen'
          version: '1901'

.. _manifest-definitions:

Definitions
-----------

- ``rc.version``: the version of the RC.
- ``rc.type``: the type of RC being represented.
- ``rc.identifier``: the shortname of this RC, which should also be the directory or filename (e.g. en_ulb_gen.zip).
- ``content_mime_type``: the format of the text that is being stored inside the RC. The supported formats are currently:

  - text/usfm
  - text/markdown
  
- ``language``: the language information of the text inside this RC.
- ``project``: the project information of the text inside this RC.
- ``resource > status``: the translation status of the resource.
- ``resource > status > translate_mode``: The mode in which the resource may be translated.

  - all: it can always be translated.
  - gl: it can only be translated when gateway language mode is activated in the app.
  - none: it can never be translated.

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
