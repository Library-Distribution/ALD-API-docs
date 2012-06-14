ALD is a model for library and app distribution for AutoHotkey.

## ALD providers
ALD providers implement the ALD [[HTTP API]]. This way, they allow up- and download of AutoHotkey libs and apps as well as the retrieval of information about it. They can (but are not required to) implement a web user interface.

A first draft for a provider is available in this repository. It is hosted at http://maulesel.ahk4.net (https://ahk4.net/user/maulesel) (thanks a lot to daonlyfreez!).

## ALD clients
ALD clients can use the ALD API on any given provider. They must be able to handle the possible differences between these.

Planned clients include: a user and command line interface for uploading (see [Ununoctium](../../Ununoctium)) and downloading (installing) packages from a given provider (see [marmoreal](../../marmoreal)).