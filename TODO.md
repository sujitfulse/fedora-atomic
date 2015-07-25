/usr/lib/passwd
---------------

This is necessary because of the semantics of upgrades and /etc.
OSTree only has "updated files win", and there is no %post type thing
that calls out to programs to do semantic merges of things like
/etc/passwd.

Thus, /usr/lib/passwd contains the system users, and /etc/passwd
contains locally-added users.

https://sourceware.org/bugzilla/show_bug.cgi?id=16142

This is presently implemented by the `nss-altfiles` package and
http://fedorapeople.org/~walters/Use-usr-lib-passwd-for-system-users-if-it-exists.patch

/var
----

RPMs should stop shipping static content here. See:

https://people.gnome.org/~walters/ostree/doc/layout.html

https://www.mail-archive.com/systemd-devel@lists.freedesktop.org/msg17210.html

Anaconda
--------

Anaconda would need to understand how to install using OSTree.  This
should not be too difficult for an initial version - ideally hidden
behind a boot-time "ostree" commandline option.

Dracut
------

OSTree, when replicating content from a build server, effectively reverts
https://fedoraproject.org/wiki/Features/DracutHostOnly change.

Furthermore, at the moment we hardcode /etc/machine-id:
https://bugzilla.gnome.org/show_bug.cgi?id=724833

Yum/dnf/rpm
-----------

Initially, these programs should operate in read-only mode when OSTree
is underneath them.  This is mostly implemented now.

A much longer term phase would allow installing packages on top of an
OSTree base.  This requires numerous changes.  See:

https://lists.fedoraproject.org/pipermail/desktop/2014-February/009305.html
