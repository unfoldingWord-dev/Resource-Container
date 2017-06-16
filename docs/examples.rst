.. _examples:

Resource Container Examples
===========================

Here is a list of examples of valid Resource Containers that illustrate the different ways that the specification may be implemented.

USFM Scripture Bundle Example
-----------------------------

Here is what a combined USFM Scripture ``bundle`` looks like: `English Unlocked Literal Bible <https://git.door43.org/Door43-Catalog/en-ulb>`_.  If you are familiar with standard USFM zip files you'll notice that this looks the same except that it has the added ``manifest.yaml`` file that provides extra metadata about the resource.

Book Example
------------

Here is what a simple ``book`` type looks like: `English Open Bible Stories <https://git.door43.org/Door43/en-obs>`_.

Dictionary Example
------------------

Here is what a ``dict`` type looks like: `English translationWords <https://git.door43.org/Door43/en-tw>`_.

Manual Example
--------------

Here is what a ``man`` type looks like with multiple ``projects`` in the same repository: `English translationAcademy <https://git.door43.org/Door43/en-ta>`_.

Notice how the ``projects`` array in the `manifest.yaml <https://git.door43.org/Door43/en-ta/raw/master/manifest.yaml>`_ file lists all the projects that are in that repository.  Note further that there is a ``sort`` key in the ``projects`` array that will tell apps the order in which they should present the projects to the user.

Help Example
------------

Here is what a ``help`` type looks like with multiple ``projects`` in the same repository: `English translationQuestions <https://git.door43.org/Door43/en-tq>`_.

Notice how the ``projects`` array in the `manifest.yaml <https://git.door43.org/Door43/en-tq/raw/master/manifest.yaml>`_ file lists all the projects that are in that repository.
