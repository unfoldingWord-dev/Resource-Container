.. _media:

Media
=====

Resource Containers (RCs) have a ``media.yaml`` file that describes media
generated from the content within the RC. Media is most often generated
for the purpose of distributing a consumable form of the RC's content.

.. code-block:: yaml

  ---
  projects:
    -
      identifier: 'obs'
      version: '4'
      media:
        -
          identifier: 'mp3'
          version: '1'
          contributor: 'Narrator: Steve Lossing, Checker: Brad Harrington, Engineer: Brad Harrington'
          quality:
            - '64kbps'
            - '32kbps'
          file_url: 'https://cdn.door43.org/en/obs/v4/media/mp3/v1/en_obs_{chapter}_{quality}.mp3'
          zip_url: 'https://cdn.door43.org/en/obs/v4/media/mp3/v1/en_obs_{quality}.zip'
        -
          identifier: 'mp4'
          version: '1'
          contributor:
          quality:
            - '360p'
            - '720p'
          file_url: 'https://cdn.door43.org/en/obs/v4/media/mp4/v1/en_obs_{chapter}_{quality}.mp4'
          zip_url: 'https://cdn.door43.org/en/obs/v4/media/mp4/v1/en_obs_{quality}.zip'
        -
          identifier: 'pdf'
          version: '1'
          contributor:
          quality:
          file_url: 'https://cdn.door43.org/en/obs/v4/media/pdf/v1/en_obs.pdf'
          zip_url:

Because RCs may contain multiple projects, media is grouped by projects as defined in the :ref:`manifest`.

Definitions:
------------

- ``projects->identifier`` the identifier of the project
- ``projects->version`` the version of the resource that is represented by the media. This is the same value represented in the :ref:`manifest`.
- ``projects->media`` an array of media definitions
- ``projects->media->identifier`` the identifier of the media
- ``projects->media->version`` the version of the media provided. This is is seperate from the version of the project.
- ``projects->media->contributor`` the people who have contributed to the creation of the media. This is different from people who actually wrote the content in the RC.
- ``projects->media->quality`` where applicable, this is an array of perceivable quality in which the media is available.
- ``projects->media->file_url`` the location where a single media file can be found. This may contain expressions for pattern replacement.
- ``projects->media->zip_url`` the location where a zip archive of all the media can be found. This may contain expressions for pattern replacement.

For instances where the location of media accepts parameters,
such as ``{chapter}`` or ``{quality}``,
You should insert appropriate values.

Media Storage
-------------

The responsibility for storing media is left to those creating it.
Media will not be accepted for storage in `DCS <https://git.door43.org>`_
in order to reduce load on the server and maintain speed and reliability.

When preparing media for distribution we suggest uploading files to a
content delivery network (CDN).
