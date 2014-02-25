Heroku buildpack: Python Extras
===============================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps with added support for Gem and Node package dependencies.

It uses:

* [pip](http://www.pip-installer.org/)
* [bundler](http://gembundler.com/)
* [npm](https://npmjs.org/)

Additional (optionally) included libraries:

* [libmemcached](http://libmemcached.org/libMemcached.html)
* [GEOS](http://trac.osgeo.org/geos/)

Installation
------------

Use [heroku-buildpacks](https://github.com/heroku/heroku-buildpacks):

``` bash
$ heroku plugins:install https://github.com/heroku/heroku-buildpacks
$ heroku buildpacks:set localmed/python-extras
```

Usage
-----

``` bash
$ ls
Procfile  requirements.txt  Gemfile  Gemfile.lock  web.py

$ git push heroku master
...
-----> Fetching custom tar buildpack... done
-----> Python app detected
-----> No runtime.txt provided; assuming python-2.7.6.
-----> Preparing Python runtime (python-2.7.6)
-----> Installing Distribute (0.6.36)
-----> Installing Pip (1.3.1)
-----> Installing dependencies using Pip (1.3.1)
       ...
-----> Installing gem dependencies using bundler version 1.2.1
       ...
-----> Installing node packages using npm version 1.0.106
       ...
```

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root.

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug.

### Gem Dependencies

If your project contains a `Gemfile`, bundler will be used to install Gem dependencies.

### Node Dependencies

If your project contains a `package.json`, npm will be used install Node dependencies.

### Specify a Runtime

You can also provide arbitrary releases Python with a `runtime.txt` file.

``` bash
$ cat runtime.txt
python-3.3.3
```

Runtime options include:

- python-2.7.6
- python-3.3.3
- pypy-1.9 (experimental)

Configuration
-------------

*Note: not all versions of libraries are supported.**

| Variable           | Type    | Default      | Description |
| ------------------ | ------- | ------------ | ----------- |
| CLEAR_CACHE        | Boolean | 0            | Clear the cache before compilation. Installs dependencies from scratch |
| FRESH_PYTHON       | Boolean | 0            | Reinstall python |
| PYTHON_VERSION     | String  | python-2.7.6 | Version of Python to install. Using `runtime.txt` is preferred. |
| SETUPTOOLS_VERSION | String  | 2.2          | Version of setuptools to install.<br>Options: 2.2 |
| PIP_VERSION        | String  | 1.5.4        | Version of pip to install.<br>Options: 1.5.4 |
| SKIP_PYLIBMC       | Boolean | 0            | Do not install libmemcached (for pylibmc) |
| SKIP_GEOS          | Boolean | 0            | Do not install GEOS (for GeoDjango) |
| GEOS_VERSION       | String  | 3.3.5        | Version of GEOS to install<br>Options: 3.3.5 |
| SKIP_BUNDLER       | Boolean | 0            | Do not install Ruby dependencies using bundler |
| BUNDLER_VERSION    | String  | 1.5.2        | Version of bundler to install.<br>Options: 1.5.2, 1.3.5, 1.2.2 |
| SKIP_NPM           | Boolean | 0            | Do not install Node dependencies using npm |
| NODE_VERSION       | String  | 0.10.26      | Version of Node to install.<br>Options: 0.11.9, 0.10.26, 0.9.12 |
| SKIP_COLLECTSTATIC | Boolean | 0            | Do not run `collectstatic` |
