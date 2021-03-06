#[![ZNC](http://wiki.znc.in/resources/assets/wiki.png)](http://znc.in) - An advanced IRC bouncer [![Build Status](https://travis-ci.org/znc/znc.svg?branch=master)](https://travis-ci.org/znc/znc)

## Table of contents

- Minimal Requirements
- Optional Requirements
- Installing ZNC
- Setting up znc.conf
- Special config options
- Using ZNC
- File Locations
- ZNC's config file
- Writing own modules
- Further infos

## Minimal Requirements

Core:

* GNU make
* pkg-config
* GCC 4.7 or clang 3.2
* autoconf and automake (if building from git)

## Optional Requirements

SSL/TLS support:
* openssl 0.9.7d or later
    * try installing openssl-dev, openssl-devel or libssl-dev
    * Mac OS X: OpenSSL from Homebrew is prefered over system
        * use `USE_SYSTEM_OPENSSL=true` as environment variable to force `configure` to use the (deprecated) Mac OS X version

modperl:
* perl and its bundled libperl
* SWIG if building from git

modpython:
* python and its bundled libpython
* perl is required
* Mac OS X: Python from Homebrew is preferred over system version
* SWIG if building from git

cyrusauth:
* This module needs cyrus-sasl2

Character Encodings:
* To get proper character encoding and charsets install ICU (`libicu4-dev`)

## Installing ZNC

Installation from source code is performed via `automake` tool chain:
Run `./autogen.sh` if building from git first to generate the `configure` script.

```shell
./configure
make
make install
```

You can use `./configure --help` if you want to get a list of options, though the defaults should be suiting most needs. 

## Setting up znc.conf

Before starting ZNC the first time you need to generate a configuration file in `~/.znc/` by running `znc --makeconf`.

If you want to connect to ZNC via TLS generate a certificate first by running `znc --makepem`.

## Special config options

When you create your ZNC configuration file via --makeconf, you are asked two
questions which might not be easy to understand.

> Number of lines to buffer per channel

How many messages should be buffered for each channel. When you connect to ZNC
you get a buffer replay for each channel which shows what was said last. This
option selects the number of lines this replay should consist of. Increasing
this can greatly increase ZNC's memory usage if you are hosting many users.
The default value should be fine for most setups.

> Would you like to keep buffers after replay?

If this is disabled, you get the buffer playback only once and then it is
deleted. If this is enabled, the buffer is not deleted. This may be useful if
you regularly use more than one client to connect to ZNC.

## Using ZNC

Once you have started ZNC you can connect with your favorite IRC-client to ZNC.
You should use `username:password` as the server password (e.g. `/pass user:pass`).

Once you are connected you can do `/msg *status help` for some commands.
Every module you have loaded (`/msg *status listmods`) should additionally provide
	/msg *modulename help

## File Locations

In its data dir (`~/.znc` is default) ZNC saves most of its data. The only
exception are modules and module data, which are saved in `<prefix>/lib/znc`
and `<prefix>/share/znc`, and the znc binary itself.
More modules (e.g. if you install some later) can be saved in
`<data dir>/modules` (-> `~/.znc/modules`).

In the datadir are only two files:

- `znc.pid` - The pid of the currently running ZNC instance.
- `znc.pem` - This is the server certificate ZNC uses for listening and is created
 with `znc --makepem`.

These directories are also in there:

- configs - Contains `znc.conf` (ZNC's config file) and backups of older configs.
- modules - ZNC also looks in here for a module.
- moddata - Global modules save their settings here.
  (e.g. webadmin saves the current skin name in here)
- users   - This is per-user data and mainly contains just a moddata directory.

## ZNC's config file

This file shouldn't be too hard too understand. An explanation of all the
items can be found on the [Configuration](http://wiki.znc.in/Configuration)-Page.
Warning: better not to edit config, while ZNC is running.

To rehash the config file, you can send ZNC SIGHUP via
	pkill -SIGHUP znc
or you can login to ZNC and use
	/msg *status rehash

If you changed some settings while ZNC is running, a simple
	pkill -SIGUSR1 znc
will make ZNC rewrite its config file. Alternatively you can use this:
	/msg *status saveconfig

## Writing own modules

You can write your own modules in either C++, python or perl.

C++ modules are compiled by either saving them in the modules source dir and
running make or with the znc-buildmod shell script.

For additional info look in the wiki:

- [Writing modules](http://wiki.znc.in/Writing_modules)

Perl modules are loaded through the global module [ModPerl](http://wiki.znc.in/Modperl).

Python modules are loaded through the global module [ModPython](http://wiki.znc.in/Modpython).

## Further infos

Please visit http://znc.in/ or #znc on freenode if you still have questions.

You can get the latest development version with git:
	git clone git://github.com/znc/znc.git
