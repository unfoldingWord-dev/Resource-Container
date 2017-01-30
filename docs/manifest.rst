.. _manifest:

Manifest File
=============

.. include:: includes/note_keys_required.txt

Resource Containers (RCs) have a manifest.yaml that describes it's content and structure.
Most of the information adheres to the `Dublin Core Meta Data Initiative <http://dublincore.org/documents/dcmi-terms>`_
and can be found nested within the ``dublin_core`` key.

.. code-block:: yaml

    ---

    dublin_core:
        conformsto: 'rc0.2'
        contributor:
            - 'A Contributor'
            - 'Another Contributor'
        creator: 'Wycliffe Associates'
        description: 'The Unlocked Literal Bible is an open-licensed version of the Bible that is intended to provide a form-centric translation of the Bible.'
        format: 'text/usfm'
        identifier: 'en-ulb'
        issued: '2015-12-17'
        language:
            identifier: 'en'
            title: 'English'
            direction: 'ltr'
        modified: '2015-12-22T12:01:30-05:00'
        publisher: 'Door43'
        relation:
            - 'en-udb'
            - 'en-tn'
        rights: 'CC BY-SA 4.0'
        source:
            -
                identifier: 'en-asv'
                language: 'en'
                version: '1901'
        subject: 'Bible translation'
        title: 'Unlocked Literal Bible'
        type: 'book'
        version: '3'

    checking:
        checking_entity:
            - 'Wycliffe Associates'
        checking_level: '3'

    projects:
        -
            categories:
                - 'bible-ot'
            identifier: 'gen'
            path: './content'
            sort: 1
            title: 'Genesis'
            versification: 'kjv'

.. _manifest-definitions:

Definitions
-----------


- ``dublin_core``

    - ``conformsto``: the version of the RC specification used by the RC.
    - ``contributor``: an array of names or aliases to people that have contributed to the resource.
    - ``format``: the file format of content within the RC e.g.

        - ``text/usfm``
        - ``image/png``
        - ``audio/mp3``

    - ``identifier``: a :ref:`slug` formatted string uniquely identifying the resource.
    - ``issued``: the :ref:`date` of publication.
    - ``publisher``: the name of the individual or organization responsible for publishing the resource.
    - ``relation``: identifiers of other related RCs.
    - ``type``: the RC type.

- ``projects``: an array of projects inside the RC.

    - ``identifier``: a :ref:`slug` formatted string uniquely identifying the project.
    - ``path``: the relative path to the project within the RC. Depending on the RC type this may be a directory or a file.
    - ``versification``: the system used for placing verse markers and consequently chunk markers.

Generating From USFM
--------------------

See :ref:`usfm_manifest` for instructions on populating the manifest.yaml from the headers in a usfm file.