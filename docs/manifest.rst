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
        type: 'book'
        conformsto: 'rc0.2'
        format: 'text/usfm'
        identifier: 'en-ulb'
        title: 'Unlocked Literal Bible'
        subject: 'Bible translation'
        description: 'The Unlocked Literal Bible is an open-licensed version of the Bible that is intended to provide a form-centric translation of the Bible.'
        language:
            identifier: 'en'
            title: 'English'
            direction: 'ltr'
        source:
        -
            language: 'en'
            identifier: 'en-asv'
            version: '1901'
        rights: 'CC BY-SA 4.0'
        creator: 'Wycliffe Associates'
        contributor:
            - 'NamesOfContributors'
        relation:
            - 'en-udb'
            - 'en-tn'
        publisher: 'Door43'
        issued: '2015-12-17'
        modified: '2015-12-22T12:01:30-05:00'
        version: '3'

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


- ``dublin_core``

    - ``type``: the RC type.
    - ``conformsto``: the version of the RC specification used by the RC.
    - ``format``: the file format of content within the RC e.g.

        - ``text/usfm``
        - ``image/png``
        - ``audio/mp3``

    - ``identifier``: a :ref:`slug` formatted string uniquely identifying the resource.
    - ``contributor``: an array of names or aliases to people that have contributed to the resource.
    - ``relation``: identifiers of other related RCs.
    - ``publisher``: the name of the individual or organization responsible for publishing the resource.
    - ``issued``: the :ref:`date` of publication.

- ``projects``: an array of projects inside the RC.

    - ``identifier``: a :ref:`slug` formatted string uniquely identifying the project.
    - ``versification``: the system used for placing verse markers and consequently chunk markers.
    - ``path``: the relative path to the project within the RC. Depending on the RC type this may be a directory or a file.
