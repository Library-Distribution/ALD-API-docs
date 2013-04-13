---
title: ALD Package Format
---
## File format
ALD packages are standard ZIP files. They are usually stored with the extensions `.alp` but may have any other as well.

## The definition
The package contains a file called `definition.ald`. This is a XML file which follows the rules of [this XSD schema](http://api.libba.net/schema.xsd). It defines the files included in the package as well as any metadata.

## Other files
The package may contain the files defined in the definition and nothing else. No file that is defined may be missing, no file that is not defined may be present. If this condition is not met, ALD providers must reject the package on upload.