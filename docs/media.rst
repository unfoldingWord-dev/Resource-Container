.. _media:

Media
=====

Resource Containers (RCs) have a ``media.yaml`` file that describes media
generated from the content within the RC. Media is most often generated
for the purpose of distributing a consumable form of the RC's content.


Media Block
-----------
RC's may be compiled into many different forms.
Therefore, you may include many different media blocks each representing a single format.
A minimal media block requires the following keys:

- ``identifier`` an :ref:`identifier` representing the media type
- ``version`` the version of the media file. This does not necessarily correspond to the RC version.
- ``contributor`` a list of people who have contributed in generating this media type. e.g. printers, sound engineers, etc.
- ``url`` the web address where the media file can be downloaded.

**Example:**

.. code-block:: yaml

  identifier: 'pdf'
  version: '1'
  contributor: []
  url: 'http://cdn.door43.org/en/ta/v1/pdf/en_ta_v1.pdf'


Variable Expansion
^^^^^^^^^^^^^^^^^^

The value of the ``url`` key may also include variables to make the path dynamic.
However, the variables are limited to keys within the media block and only scalar (non-list) values are allowed.
e.g. you can use the ``version`` key but not ``contributor`` since ``contributor`` is not a scalar value.

Additionally, you may map the ``version`` key to the version found in the :ref:`manifest` by settings it's value to ``{latest}``.

**Example:**

.. code-block:: yaml

  identifier: 'pdf'
  version: '{latest}'
  contributor: []
  url: 'http://cdn.door43.org/en/ta/v1/{identifier}/en_ta_v{version}.pdf'

Media Quality
^^^^^^^^^^^^^

For certain media types it is important to note the quality such as audio and video media.
For such types an additional key may be added to the media block.

- ``quality`` indicates the quality (such as bit rate or frame rate) of the media.

**Example:**

.. code-block:: yaml

  identifier: 'mp3'
  version: '1'
  quality: '64kbps'
  contributor: []
  url: 'https://cdn.door43.org/en/obs/v4/media/mp3/v1/{quality}/obs.zip'


Chapter Media
^^^^^^^^^^^^^

In order to support the concept of chapters. We introduce a new key to the media block:

- ``chapter_url`` this is similar to the ``url`` key except it contains a pseudo variable named ``chapter`` that can be expanded across all chapters in a resource.

**Example:**

.. code-block:: yaml

  identifier: 'mp3'
  version: '1'
  quality: '64kbps'
  contributor: []
  url: 'https://cdn.door43.org/en/obs/v4/media/mp3/v1/{quality}/obs.zip'
  chapter_url: 'https://cdn.door43.org/en/obs/v4/media/mp3/v1/{quality}/obs_{chapter}.mp3'

Media Scope
-----------

There are two different scopes for media blocks: resource, and projects.
The resource scope allows you to define a media type that encompasses the entire RC while the projects scope encompasses individual projects within the RC.
All resource scoped media blocks should be nested within a top level ``resource`` key.
All projects scoped media blocks should be nested within a top level ``projects`` key.

**Example:**

.. code-block:: yaml

  ---
  resource:
    version: '1'
    media: []
  projects: []

The projects scope is a list while the resource scope is simply a dictionary (because there's only one resource in the RC).

Each project scope must include the following project keys:

- ``identifier`` the project identifier as found within the :ref:`manifest`.
- ``version`` the version of the RC that is represented in the subsequent media.

The resource scope only needs the ``version`` key.

If you expect to keep your media up to date with the latest version of the RC, as found within the :ref:`manifest`,
you may use the variable expansion ``{latest}`` with the ``version`` key above.
`hint: if you use {latest} in a media block you should probably also use it in the parent scope`.

.. note:: The two project keys are currently **not** available for variable expansion within child media blocks.

Putting It All Together
-----------------------

.. code-block:: yaml

  ---
  resource:
    version: '{latest}'
    media:
      -
        identifier: 'pdf'
        version: '{latest}'
        contributor: []
        url: 'https://cdn.door43.org/en/ulb/v{version}/media/{identifier}/ulb.pdf'

  projects:
    -
      identifier: 'gen'
      version: '{latest}'
      media:
        -
          identifier: 'mp4'
          version: '{latest}'
          contributor:
            - 'Narrator: Steve Lossing'
            - 'Checker: Brad Harrington'
            - 'Engineer: Brad Harrington'
          quality:
            - '360p'
            - '720p'
          url: 'https://cdn.door43.org/en/ulb/v{version}/media/{identifier}/{quality}/gen_chapter_videos.zip'
          chapter_url: 'https://cdn.door43.org/en/ulb/v{version}/media/{identifier}/{quality}/gen_{chapter}.mp4'
        -
          identifier: 'pdf'
          version: '{latest}'
          contributor: []
          url: 'https://cdn.door43.org/en/ulb/v{version}/media/{identifier}/01-GEN.pdf'


Media Storage
-------------

The responsibility for storing media is left to those creating it.
Media will not be accepted for storage in `DCS <https://git.door43.org>`_
in order to reduce load on the server and maintain speed and reliability.

When preparing media for distribution we suggest uploading files to a
content delivery network (CDN).
