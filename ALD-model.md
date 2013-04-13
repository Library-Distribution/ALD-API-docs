---
title: ALD model
layout: docs
---
ALD is a model for library and app distribution for AutoHotkey.

## ALD providers
ALD providers implement the ALD [HTTP API](HTTP-API). This way, they allow up- and download of AutoHotkey libs and apps as well as the retrieval of information about it. They can (but are not required to) implement a web user interface.

A first draft for a provider is available in this repository. It is hosted at <http://api.libba.net> (<https://api.libba.net>) (webinterface at <http://libba.net>).

## ALD clients
ALD clients can use the ALD API on any given provider. They must be able to handle the possible differences between these.

Planned clients include: a user and command line interface for uploading (see [Ununoctium](https://github.com/Library-Distribution/Ununoctium)) and downloading (installing) packages from a given provider (see [marmoreal](https://github.com/Library-Distribution/marmoreal)).