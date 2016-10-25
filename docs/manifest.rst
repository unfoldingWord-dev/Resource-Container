Manifest File
=============

All resource containers have a package.json file which looks something like this:

.. code-block:: json

    {
      "package_version": 7,
      "modified_at": 20151222120130,
      "content_mime_type": "text/usfm",
      "versification_slug": "some-slug",

      "language": {
        "slug": "en",
        "name": "English",
        "dir": "ltr"
      },

      "project": {
        "slug": "gen",
        "name": "Genesis",
        "description": "",
        "icon": "",
        "sort": 1,
        "categories": [
          "bible-ot"
        ]
      },

      "resource": {
        "slug": "ulb",
        "name": "Unlocked Literal Bible",
        "type": "book",
        "status": {
          "translate_mode": "all",
          "checking_entity": [
            "Wycliffe Associates"
          ],
          "checking_level": "3",
          "version": "3",
          "comments": "",
          "contributors": [
            "Wycliffe Associates"
          ],
          "pub_date": "2015-12-17",
          "license": "CC BY-SA 4.0",
          "checks_performed": [
            "keyword",
            "metaphor",
            "etc..."
          ],
          "source_translations": [
            {
              "language_slug": "en",
              "resource_slug": "asv",
              "version": "1901"
            }
          ]
        },
      },

      "chunk_status": [
        "00-title",
        "01-01",
        "01-02",
        "01-title",
        "01-reference",
        "02-01"
      ]
    }


Definitions
-----------

- package_version: indicates the version of the resource container spec that is implemented by the current container.
- modified_at: indicates when this resource container was last modified
- content_mime_type: the format of the text that is being stored inside this resource container. The supported formats are currently:
  - text/usfm
  - text/markdown
- versification_slug: the versification system used to define the chunks in this resource.
- resource: the resource that is stored in this resource container
  - type: the type of resource being represented in this resource container.
  - translate_mode: indicates if/when this resource can be translated
    - all: it can always be translated.
    - gl: it can only be translated when gateway language mode is activated in the app.
    - none: it can never be translated.
- language: the language of the text that is being stored inside this resource container.
- project: the project to which the content stored in this resource container belong.
- chunk_status: checking status of chunks in this resource container.
- status > translate_mode: The mode in which the resource may be translated.


Changing Identifying Properties
------------------------------

An identifying property is any property used in the generation of the resource container slug.

Sometimes it is desirable to change certain identifying properties. For example a mistake may have been made when choosing the language or the resource.

Not all identifying properties are allowed to be modified after a resource container has been created. The properties that can be changed are:

- language
- resource

Care should be taken since this will also change the slug of the resource container.

If while changing an identifying property the resource container will conflict with an existing resource container the user should be asked if they would like to merge the two resource containers or cancel the change. See Merging Resource Containers for more information about merging.

In order to fully change an identifying property the following steps must be taken

1. change the value of the property in the package.json.
2. update any usages of the resource container's slug to the new slug.
