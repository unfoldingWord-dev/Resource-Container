Resource Container Structure
============================

Container Format
----------------

A resource container may exist in two forms:

- a compressed archive with the extension .tsrc as in: some_resource_container.tsrc
- a directory as in: some_resource_container/

In both cases the internal structure is exactly the same. Compressing a resource container and changing the file extension to .tsrc simply makes the resource container portable.

Slug
----

The resource container slug is primarily used to create unique file names for storing resource containers on the disk. In the case of creating a translation this is also the name of the online repository where the resource container is uploaded on Door43. These slugs are also used when linking from one resource container to another. See Resource Container Links for more information about linking.

The slug is also a convenient way to identify a resource container without opening it to inspect the package.json. This can be helpful when searching for resource containers through an API that only returns the resource container slugs.

Slugs are structured as indicated below. Each component of the slug is delimited by an underscore _.

.. code-block:: none

    [language slug]_[project slug]_[resource slug]

Each component of the slug may include alphanumeric characters and dashes -. See Resource Types for a list of valid resource types that may be used in the slug.

NOTE: slug components may not contain any underscores _ since this is the delimiter.

An example of using the slug to identify a resource container is illustrated below. By viewing the file name we are able to quickly identify that this resource container contains the ULB version of the book Genesis translated in English:

.. code-block:: none

    en_gen_ulb.tsrc

As mentioned above, the slug is not the strict authority regarding the nature of the resource container. Therefore whenever a resource container is utilized the package.json should always be consulted and has the final word. This is important since a user could change the name of the file between exporting it and sharing it with someone else.

In order to correctly generate the slug of a resource container you must consult the package.json which contains all of the necessary information.



Directory Structure
-------------------

Resource containers may be zip archives with the .tsrc (translationStudio) extension, or directories.

.. code-block:: none

    my_resource_container.tsrc/
        |-.git/
        |-LICENSE.md
        |-package.json
        |-content/

- the .git directory is optional and is usually only seen in active translations.
- LICENSE.md contains the appropriate license information for the resource container.
- package.json contains meta data about the resource container.
- the content directory contains any other files needed by the resource type.

