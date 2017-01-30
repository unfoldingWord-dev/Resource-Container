.. _usfm_manifest:

USFM to Manifest
================

When converting a USFM file such as ``01-GEN.usfm`` into an RC
follow the rules below when populating the :ref:`manifest`.

Map
---

Some values are known by default since USFM is always used for Bible projects:

- `dublin_core:type` = ``book``
- `dublin_core:conformsto` = ``rc0.2``
- `dublin_core:format` = ``text/usfm``
- `dublin_core:subject` = ``Bible translation``
- `projects:categories` = ``bible-ot`` or ``bible-nt`` depending on the project identifier

The rest must be parsed from the USFM:

**\id_<CODE>_(Name of file, Book name, Language, Last edited)**

- ``code`` -> `projects:identifier`
- ``Book name`` -> `dublin_core:title`
- ``Language`` -> `dublin_core:language:title`
- ``Last edited`` -> `dublin_core:modified`

**\h_text...**

- ``text`` -> `projects:title`

TODO: We still need to identify other things:

- `dublin_core:identifier`
- `dublin_core:language:identifier`
- `dublin_core:language:direction`
- `dublin_core:rights`
- `dublin_core:contributor`
- `projects:versification`
