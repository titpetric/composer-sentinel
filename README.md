composer-sentinel
=================

A quick composer/packagist private repository generator.


What does it do?
----------------

```
Usage: ./composer-sentinel [command]

Commands:
        build: Build the package repository.
        pull: Pull the package sources from their repositories/locations.
```

Defining packages
-----------------

Edit `composer-sentinel.json` to add folders as you wish:

```
{
  "sources": [
    { "name": "local_project" },
    { "name": "monotek_minitpl" },
    { "name": "test_psr0" }
  ],
  "archive": {
    "directory": "dist",
    "url": "http://packages.mtk.lan/composer"
  }
}
```

The property `name` is the local location of your packages sources. These can be
source trees of your existing git or svn repositories, or they can be unversioned
directories which you want to build into Composer packages.

Each folder needs to contain a `composer.json` file. Please check the
[Packagist documentation](https://packagist.org/about) on how to create this file.


Building packages
-----------------

Building the Composer repository is simple. After you configure your packages,
just run `./composer-sentinel build` to generate repository under `build/` folder.

Configuring the archive properties gives you control over the URL and subfolder
where the packages are stored and delivered from.

Packages are automatically versioned. Caveat:

1. If you define "version" in "composer.json", it has to be "X.Y" (and not X.Y.Z+),
2. If the version isn't defined, it is assumed to be "1.0",
3. This version gets appended with "ymd.hi" (ie. 1.0.130429.1519)

This type of version control allows automatic updating in your projects with
`composer update` within a given version branch. Additional versioning or tagging
can be done by yourself using your VCS, if you need it.


VCS support
-----------

You can add the `"type": "git", "source": "[url]"` properties to your package
sources list. The `[url]` can be either a local or remote git repository.

If you define `type` and `source`, you can use `./composer-sentinel pull` to
generate the initial code checkout, or to pull code from a remote git repository.

Currently only `git` is supported to the extent documented above. You can
use subversion as you wish, you're currently left to check out and update
your subversion repository manually.


Why?
----

I needed a lightweight composer packager that would check out and use local
folders to build packages and version them automatically. I needed it to be
pretty stupid.

If you need something smarter, take a look at [satis](https://github.com/composer/satis)
or [packagist](https://github.com/composer/packagist).
