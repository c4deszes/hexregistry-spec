# Recommendations

## Metainfo

### Identification

+ The artifact id shall be unique across the organization

+ Each artifact should have a unique name to improve searchability

+ The artifact name should not contain abbreviations

+ Each artifact should have a description at least 100 characters long

### Versioning

+ An already published artifact should have no change in the variant attribute
order

### Legal

+ License should have a link attribute

+ Authors should have contact information accessible either in string format,
e.g.: `John Doe <john.doe@example.org>` or through additional attributes e.g.:

    ```json
    {
        //...
        "authors": [
            {
                "name": "John Doe",
                "email": "john.doe@example.org"
            }
        ]
        //...
    }
    ```

### External references

+ Attachments should be variant aware, unless the same files or links are
relevant for all variants

+ For local files the attachment name should be the same as the file extension,
unless there are multiple attachments with the same extension or the extension
is not relevant to the content, e.g.: zip file of documentation

## Packages

+ Avoid transitive dependencies

+ Avoid diamond dependency
