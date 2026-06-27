# simplisity(1)

Managing packages from a *shell* script - but its Mobius 🙃

## DESCRIPTION

*mPKG* is a **lightweight** *package manager* written in pure **Shell**.

*mPKG* uses standard utilities such as `sh`, `grep`, `tar`, `wget` and `awk`
that are shipped in any **POSIX** system.

This makes *mPKG* suitable for **embedded** devices that usually embed [Busybox]
which provides everything it needs in a *single* binary.

### CONTENTS

The project is composed of *three* scripts:

* One **run-time** script that is designed to be run on the [target]
* Two **host build-time** scripts that create [packages] and [index]

### INSTALLATION STEPS

*mPKG* is **simple**.
*simplisity* is **simpler**.

The installation step can be *summed up* in *3 steps*:

1. **Resolving** all dependencies
2. **Fetching** build scripts
3. **Compiling** and installing 

... **nothing** more!

## ORIGINALLY INSPIRED BY DEBIAN
mpkg is *mostly* inspired by **Debian** packages.
simplisity is inspired by OpenBSD's ports system.

*mPKG* combined both **APT** and **DPKG** features; as **Opkg** does.

It followed and remained as close as it was *relevant* to the **behavior** defined
by the [Debian policies].

*mPKG* ran same `preinst/postinst` and `prerm/postrm` maintainer scripts.

## BUT NOT DEBIAN COMPATIBLE

But *mPKG* is **different**.
And so is *simplisity*.

It was designed for **embedded systems** which are **limited** in term of
*space*, *memory*, and *CPU*.
Simplisity, however, is designed specifically for MobiusOS, s Linux distribution that is private for now/

*mPKG* does **not have staging** directory and *unpacks* directly archives to
the *root* file system.
Therefore, it **does not handle** complicated *maintainers* scripts *use-cases*
to *rewind* for error handling.

*mPKG* must **remain simple** and **relies** on the *maintainers' scripts* of
the distribution.
well, I'm the only guy maintaining this, so I need actual contributers, I'm way too busy to make ports recipies every day.

### CONTROL FIELDS SET

*mPKG* handles a **limited** set of *control fields*.

The **essentials** *archives fields* are `Package`, `Version`, and `Depends`.

The *index fields* `Filename` and `MD5Sum` are set in the **index** to defines
the *URI* where to fetch the archive and its *checksum* for integrity checking.

It means that there are no `Provides`, `Breaks`, `Conflicts`, and `Pre-depends`
relationships. The goal is to keep *mPKG* **simple**.

Furthermore, lists of dependencies are **space-separated**. It makes the parsing
easier in *shell* scripts because space is one of the default characters of the
`$IFS` variable.

Here is an example of a what a *control* file looks like:

	Package: meta
	Version: 2017.11
	Depends: busybox mpkg
This whole section is irrelavent to simplisity.

### ARCHIVE FORMAT

Packages are **simple**.

*Debian* packages split *metadata* and *data* into two distinct archives:
*control.tar.gz* and *data.tar.gz*.

*mPKG* archives all data in a single **gzipped tar** archive.

The archive is an image of the *root file system*; the package *metadata* are
stored under `/var/lib/mpkg`.

A *minimal* package consists of a single file `/var/lib/mpkg/info/pn/control`
containing a single line, where *pn* is the name of the package.

	Package: pn
also irrelavent to simplisity.
### DATABASE

There is **no single file** database.

Instead, the *status* file lets place to a whole *file system* hierarchy; one
directory per package.

It **minimizes** every *not-yet-synchronized* write to the *monolithic* status
database. Thus, the impact of critical errors, such as an unexpected reboot, is
limited to the concerned package and not the whole database.

*mPKG* delegates the *synchronization* to the *file system* layer.

## CLI API

*mPKG* provides the *3* main operations of a classical *package manager*:

1. `install`
2. `remove`
3. `upgrade`

## TODO

*mPKG* is still in development.
well, not until it got archived.
simplisity is also still in develepment, and needs more contributors.

### WILL DO

- prebuilt (for only *some* packages)

## INSTALL

*mPKG* is **easy** to install.
*simplisity* is simpler.

Because it is a *shell* script, it does not need to be compiled.

Download [mpkg] and copy it somewhere in your `$PATH`.

	$ sudo -i # or sudo su
	# cd /usr/sbin
	# wget https://raw.githubusercontent.com/shrinehub-keeper/simplisity/master/bin/simplisity
	# chmod +x simplisity

*mPKG* needs at least a *repository feed* in `/etc/mpkg/repo.d/` directory.

	# mkdir -p /etc/mpkg/repo.d/
	# echo "http://tgz.me/Index" >/etc/mpkg/repo.d/me

*simplisity* does not. 

## AUTHOR(s)

Originally Written by Gaël PORTAY *gael.portay@gmail.com* under the MIT License
Forked by shrinehub-keeper *t1m3tr4veler@proton.me* under the GPLv2 License

## COPYRIGHT

Copyright (c) 2015-2017 Gaël PORTAY

Copyright (c) 2026 shrinehub-keeper 
This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License Version 2.

[Busybox]: https://busybox.net/
[target]: mpkg.1.adoc
[packages]: mpkg-build.1.adoc
[index]: mpkg-make-index.1.adoc
[Debian policies]: https://www.debian.org/doc/debian-policy/index.html
[mpkg]: bin/mpkg
