# Resource Container Documentation

See http://resource-container.readthedocs.io/ for the documentation, this repo is the source files.


If you want to suggest a change, please fork this repo and create a PR, or create an Issue.

## About
The goal of Resource Containers is to make translation material portable, structured, and user friendly.

This specification is designed in conjunction with, but not dependent on, the [Door43 Resource Catalog API](https://github.com/unfoldingWord-dev/door43.org/wiki/API-v3-Resource-Catalog-Endpoint).

## Building
Install the sphinx engine

    sudo apt-get install python-sphinx

Install the theme with [pip](https://pypi.org/project/pip/)

    pip install sphinx_rtd_theme

Then run the build script.

> NOTE: this top level makefile is just a shorcut to building the html.

    make
