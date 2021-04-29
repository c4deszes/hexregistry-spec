# Bootloader - Application Example

This demonstrates a common problem being solved in multiple ways.

The problem is the delivery of a bootloader, an application and eeprom data.
`hexregistry` tries to solve this by allowing dependencies to be defined.

## Solution 1 - single package

In this case a single package is delivered which contains all three artifacts.
The advantage of this is that only a single metainformation file needs to be
created, however if the bootloader has it's own slower lifecycle then it
increases the redundancy in the registry.

```json
{
    "id": "example",
    "name": "Example program",
    "description": "This is an example program",
    "version": "1.0.0",
    "variant": "debug",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "artifacts": [
        {
            "file": "${id}-${version}-${variant}-bootloader.hex",
            "regions": {
                //...
            },
            "traceability": {
                //...
            }
        },
        {
            "file": "${id}-${version}-${variant}.hex",
            "regions": {
                //...
            },
            "traceability": {
                //...
            }
        },
        {
            "file": "${id}-${version}-${variant}-eeprom.hex",
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

## Solution 2 - dependent package

If the bootloader is versioned differently then it may make sense to have it as
a different package and reference it from the application. The example below
implements this, when the application is pulled it will transitively include the
bootloader as well.

Bootloader metainfo file:

```json
{
    "id": "bootloader",
    "name": "Bootloader",
    "description": "This is a bootloader",
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

Application and data metainfo:

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
    "dependencies": {
        "bootloader": {
            "version": "1.0.0",
            "variant": "release"
        }
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
        },
        {
            "file": "${id}-${version}-${variant}-eeprom.hex",
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

## Solution 3 - deployment

In the previous solution the redundancy of the bootloader was eliminated, but in
some cases the same application may be used with different bootloaders and this
would make application artifacts redundant if you had to create multiple
packages that linked them to different bootloaders.

Bootloader metainfo file:

```json
{
    "id": "bootloader",
    "name": "Bootloader",
    "description": "This is a bootloader",
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

Application and data metainfo:

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
        },
        {
            "file": "${id}-${version}-${variant}-eeprom.hex",
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

Deployment metainfo:

```json
{
    "id": "application-deployment",
    "name": "Application deployment",
    "description": "This is a deployment",
    "version": "1.0.0",
    "variant": "release",
    "license": "MIT",
    "authors": [
        "John Doe"
    ],
    "dependencies": {
        "bootloader": {
            "version": "${version}",
            "variant": "${variant}"
        },
        "application": {
            "version": "${version}",
            "variant": "${variant}"
        }
    }
}
```
