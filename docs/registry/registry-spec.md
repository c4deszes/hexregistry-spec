# Registry specification

## Registry definition

A registry is storage location for multiple packages with the purpose of
allowing multiple people to access the same packages.

## Registry layout

The registry layout is designed with the intent of allowing registry
implementations from simple to very complex. For small registries this could be
a folder containing the packages, and for larger ones it could be a webserver
with access control.

Each registry shall organize packages based on the package `id` and `version`.

The client accesses packages by appending the `id` and `version` to the registry
root like so `registry-root/<id>/<version>`.

For search purposes the registry should keep an `index.json` file at the root
of the repository that contains a list of all uploaded packages and variant
possibilities.

## Registry index

To be done
