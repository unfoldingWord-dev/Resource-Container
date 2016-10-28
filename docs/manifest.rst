Manifest File
=============

All resource containers have a package.json file which looks something like this:

.. code-block:: json

    {
      "package_version": "0.2",
      "container_type": "book",
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
        "desc": "",
        "icon": "",
        "sort": 1,
        "categories": [
          "bible-ot"
        ]
      },

      "resource": {
        "slug": "ulb",
        "name": "Unlocked Literal Bible",
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
          "license": "CC BY-SA",
          "checks_performed": [
            "keyword",
            "metaphor",
            "etc..."
          ],
          "source_translations": [
            {
              "language_slug": "en",
              "resource_slug": "obs",
              "version": "3"
            }
          ]
        },
      },

      "chunk_status": {
        "00-title": "draft",
        "01-01": "reviewed",
        "01-02": "draft2",
        "01-title": "draft2",
        "01-reference": draft2",
        "02-01": "reviewed"
      }
    }


Definitions
-----------

- package_version: the version of the resource container syntax. This includes changes to the package.json and directory structure.
- container_type: the specific type of resource container being represented.
- content_mime_type: the format of the text that is being stored inside the resource container. The supported formats are currently:
 - text/usfm
 - text/markdown
- language: the language of the text that is being stored inside this resource container.
- project: the project to which the content stored in this resource container belong.
- resource > status: the translation status of the resource.
- resource > status > translate_mode: The mode in which the resource may be translated.
 - all: it can always be translated.
 - gl: it can only be translated when gateway language mode is activated in the app.
 - none: it can never be translated.
- versification_slug indicates the versification system used to defined chunks in this resource container. See Resource Catalog and Chunk Markers for more information.
- chunk_status: the current stage of each chunk


Changing Language & Resource
------------------------------

Some times it is desirable to change certain the language and resource of a resource container. For example a mistake may have been made when originally defining the resource container.

Care should be taken since changing the language and/or resource will also change the slug of the resource container.

Implimentation Notes:
If while changing either of these properties the resource container will conflict with an existing resource container (on the current file system) the user should be asked if they would like to merge the two resource containers or cancel the change. See Merging Resource Containers for more information about merging.

In order to fully change either of these properties the following steps must be taken

1. change the value of the property in the package.json.
2. update the name of the resource container folder to the new slug.
3. update any external usages of the resource container's slug to the new slug.
