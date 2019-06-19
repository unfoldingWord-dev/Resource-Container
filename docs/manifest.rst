.. _manifest:

Manifest File
=============

.. include:: includes/note_keys_required.txt

Resource Containers (RCs) have a ``manifest.yaml`` file that describes its content and structure.
Most of the information adheres to the `Dublin Core Meta Data Initiative <http://dublincore.org/documents/dcmi-terms>`_
and can be found nested within the ``dublin_core`` key.

.. code-block:: yaml

    ---
    dublin_core:
        conformsto: 'rc0.2'
        contributor:
            - 'A Contributor'
            - 'Another Contributor'
        creator: 'Someone Or Organization'
        description: 'One or two sentence description of the resource.'
        format: 'text/usfm'
        identifier: 'ulb'
        issued: '2015-12-17'
        language:
            identifier: 'en'
            title: 'English'
            direction: 'ltr'
        modified: '2015-12-22T12:01:30-05:00'
        publisher: 'Name of Publisher'
        relation:
            - 'en/udb'
            - 'en/tn'
            - 'en/tq'
            - 'en/tw'
        rights: 'CC BY-SA 4.0'
        source:
            -
                identifier: 'asv'
                language: 'en'
                version: '1901'
        subject: 'Bible'
        title: 'Unlocked Literal Bible'
        type: 'book'
        version: '3'

    checking:
        checking_entity:
            - 'Organization or Church network'
            - 'Organization or Church network'
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

    - ``conformsto``: the version of this specification used by the RC.
    - ``contributor``: an array of names or aliases of people that have contributed to the resource.
    - ``creator``: the entity in charge of creating the resource.
    - ``description``: a brief description of what the resource is.
    - ``format``: the file format of content within the RC, e.g. ``text/usfm``, ``text/markdown`` etc.
    - ``identifier``: a :ref:`identifier` formatted string uniquely identifying the resource.
    - ``issued``: the :ref:`date` of publication. Normally this should stay in sync with ``version`` increments.
    - ``language``: the language of the resource.
    - ``modified``: the date the resource was last modified. Occassionally, this may be more recent than ``issued`` if there are metadata changes that do not include changes to the content.
    - ``publisher``: the name of the entity that published the resource.
    - ``relation``:  a array of :ref:`short-links` to related resources. In these short links we do allow a link parameter that specifies the version of the resource it is related to. For example, `el-x-koine/ugnt?v=0.7`.
    - ``rights``: the license under which the resource is distributed.
    - ``title``: the title of the resource
    - ``type``: the RC :ref:`container type <types>`.
    - ``version``: the published iteration of the resource.

- ``projects``: an array of projects inside the RC.

    - ``categories``: an array of category :ref:`identifiers <identifier>` used for hierarchical ordering.
    - ``identifier``: an :ref:`identifier` formatted string uniquely identifying the project.
    - ``path``: the relative path to the project within the RC. Depending on the RC type this may be a directory or a file.
    - ``sort``: the sorting order of this project when viewed in relation to other projects in this RC.
    - ``title``: the title of the project.
    - ``versification``: the system used for placing verse markers and consequently chunk markers.

Generating From USFM
--------------------

See :ref:`usfm_manifest` for instructions on populating the manifest.yaml from the headers in a usfm file.
