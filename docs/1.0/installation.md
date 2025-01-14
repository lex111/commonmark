---
layout: default
title: Installation
description: Instructions on how to install the league/commonmark library
redirect_from: /0.20/installation/
---

# Installation

The recommended installation method is via Composer.

In your project root just run:

~~~bash
composer require league/commonmark:^1.0
~~~

Ensure that you’ve set up your project to [autoload Composer-installed packages](https://getcomposer.org/doc/00-intro.md#autoloading).

## Versioning

[SemVer](http://semver.org/) will be followed closely.  **It's highly recommended that you use [Composer's caret operator](https://getcomposer.org/doc/articles/versions.md#caret) to ensure compatiblity**; for example: `^1.0`.  This is equivalent to `>=1.0 <2.0`.
