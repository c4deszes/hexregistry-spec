# Debug software example

This demonstrates how debugging variants can be managed and how debugging
information can be delivered.

## Variant management

In this case with each software release two packages are delivered, one as a
debug variant and another as a release variant.

Release package metainfo:

```json
{
    "id": "application",
    "name": "Application",
    "description": "This is an application",
    "version": "1.0.0",
    "variant": "release",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "artifacts": [
        {
            "file": "${id}-${version}-${variant}.hex",
            "regions": {
                //...
            },
            "traceability": {
                //...
            }
        }
    ]
}
```

Debug package metainfo:

```json
{
    "id": "application",
    "name": "Application",
    "description": "This is an application",
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
                //...
            },
            "traceability": {
                //...
            }
        }
    ]
}
```

## Debug version

In this case only special or intermediate software releases are generated with
debugging enabled. Semver is used to indicate that it's a prerelease version and
that debugging is enabled.

The advantage of this approach is that you don't need to maintain variants but
this may create confusion around the released versions and the differences
between debug and release software.

```json
{
    "id": "application",
    "name": "Application",
    "description": "This is an application",
    "version": "1.0.0-dev.3+18",
    "variant": "none",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "artifacts": [
        {
            "file": "${id}-${version}-${variant}.hex",
            "regions": {
                //...
            },
            "traceability": {
                //...
            }
        }
    ]
}
```

## Debugging information

The easiest way to include debugging symbols inside the released package is by
using attachments.

```json
{
    "id": "application",
    "name": "Application",
    "description": "This is an application",
    "version": "1.0.0",
    "variant": "debug",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "attachments": {
        "symbols": "${id}-${version}-${variant}.elf"
    },
    "artifacts": [
        {
            "file": "${id}-${version}-${variant}.hex",
            "regions": {
                //...
            },
            "traceability": {
                //...
            }
        }
    ]
}
```
