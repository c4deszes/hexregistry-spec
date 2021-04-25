# Package specification

## Package definition

A package is an immutable set of artifacts containing all the relevant files.

Each package consists of the followings files:

+ Hex artifact
+ Metainfo container
+ Local/remote attachments (optional)

## Package variants

Package variants are equivalent packages coming from the same codebase version
but the artifacts were generated with different variant attributes set.

## Package metainformation

The metainformation of packages are stored as JSON files.

### Identification

Each artifact must have the following attributes:

+ `id` is a string that uniquely identifies the projects within a repository, it
must match the following regular expression `[a-zA-Z0-9_]*`

+ `name` is a string used to represent the project similar to `id` however
there's no uniqueness requirement nor requirement on the characters that can be
used

+ `description` is a string used to describe the project in more details

### Versioning

+ `version` is a string used the version the artifact using semantic versioning,
it must match the regular expression on
[semver.org](https://semver.org/#is-there-a-suggested-regular-expression-regex-to-check-a-semver-string)

+ `variant` is a string used to indicate that the artifact was created with
certain options, it must match the following regular expression
`^([a-zA-Z0-9\.]+)(\-[a-zA-Z0-9\.]+)*$`, in other terms it must be a dash
separated list of strings

### Legal information

+ `license` can be free text referencing a publicly[^license] available license, otherwise
can be an object with the `name` and `link` attributes

  + `name` is a string

  + `link` is a string that must point to the license document

+ `authors` is a list of objects or strings containing information about
project contributors or project owners, when using objects the `name` attribute
must be specified

  + `name` is a string

### External references

+ `attachments` is an object containing links to other artifacts or anything
else that is related to the package such as documentation, external tools.
Linked artifacts can be either local files or remote files and links.

+ `scm` is an object containing link to the source control manager and to the
particular baseline that the artifact is based on

  + `repository` is a string, it must be a clickable link that directs to the
  main repository where the source code is located

  + `baseline` is a string, it must contain the information needed to be able
  to checkout the particular baseline this artifact is based on, e.g.: git hash

### Artifacts

+ `artifacts` is a list of objects containing hex file descriptions, it must
  have the following attributes

  + `file` is a string pointing to a local file

  + `regions` is an object describing what different memory regions in the hex
  file mean

  + `traceability` is an object describing links between hex file regions and
  certain package attributes, see [package traceability](package-traceability.md)

  + `verification` is an object describing links between hex file regions and
  fixed memory locations, see [package verification](package-verification.md)

### Dependencies

+ `dependencies` is an object containing package references

### Example

This is an example of a simple package consisting of a single artifact with
semantic version information located at the end of the flash region.

```json
{
    "id": "sample",
    "name": "Sample program",
    "description": "This is a sample program",
    "version": "1.0.0",
    "variant": "debug",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "artifacts": [
        {
            "file": "${id}-${version}-${variant}.hex",
            "regions": {
                "flash": {
                    "start": 128,
                    "end": 2000
                },
                "software_version": {
                    "start": 2000,
                    "length": 3
                }
            },
            "traceability": {
                "software-version": {
                    "format": "semver-min8",
                    "value": "${version}",
                    "region": "software_version"
                }
            }
        }
    ]
}
```

This is an example of a deployment using 2 packages.

```json
{
    "id": "sample",
    "name": "Sample deployment",
    "description": "This is a sample program",
    "version": "1.0.0",
    "variant": "debug",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "dependencies": {
        "sample_program": {
            "version": "${version}",
            "variant": "none"
        },
        "sample_eeprom": {
            "version": "${version}",
            "variant": "none"
        }
    }
}
```
