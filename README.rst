========
Gridsync
========

.. image:: https://api.travis-ci.org/gridsync/gridsync.svg
    :target: https://travis-ci.org/gridsync/gridsync
.. image:: https://ci.appveyor.com/api/projects/status/github/gridsync/gridsync?svg=true
    :target: https://ci.appveyor.com/project/crwood/gridsync
.. image:: https://buildbot.gridsync.io/png?builder=linux
    :target: https://buildbot.gridsync.io/builders/linux
.. image:: https://buildbot.gridsync.io/png?builder=osx
    :target: https://buildbot.gridsync.io/builders/osx
.. image:: https://buildbot.gridsync.io/png?builder=windows
    :target: https://buildbot.gridsync.io/builders/windows


 **WARNING**: *At present, Gridsync is in early stages of development and should not be relied upon.* **Do not use this software for anything important!**

Gridsync aims to provide a cross-platform, graphical user interface for `Tahoe-LAFS`_, the Least Authority File Store. It is intended to simplify the configuration and management of locally-running Tahoe-LAFS gateways and to provide user-friendly mechanisms for seamlessly backing up local files, synchronizing directories between devices, and sharing files and storage resources with other users across all major desktop platforms (GNU/Linux, Mac OS X, and Windows). More generally, Gridsync aims to duplicate most of the core functionality provided by other, proprietary "cloud" backup/synchronization services and utilities (such as Dropbox and BitTorrent Sync) but without demanding any sacrifice of the user's privacy or freedom -- and without requiring usage or knowledge of the command line. Accordingly, Gridsync is developed under the principle that secure file storage and backups should be freely available to everyone, without exception, without added barriers, and regardless of one's operating system choice.

.. _Tahoe-LAFS: https://tahoe-lafs.org


Why Gridsync?
-------------

Tahoe-LAFS already provides a number of highly desirable properties for file-storage: it is secure, decentralized, highly robust, free (as in both beer and speech), stable and mature, and written by a group of very talented developers. Unfortunately -- and despite all of its technical merits -- Tahoe-LAFS has a number of persistent usability issues which steepen its learning curve: its installation requires manual compilation from source on Windows and OS X, its configuration consists in hand-editing text files, its primary interface requires heavy command line usage, and many of its fundamental concepts (e.g., "dircap", "servers-of-happiness") are opaque to new users or otherwise demand additional reading of the project's extensive documentation. Accordingly, Tahoe-LAFS' userbase consists primarily in seasoned developers and system administrators; non-technical users are naturally excluded from enjoying Tahoe-LAFS' aforementioned advantages.

The Gridsync project intends to overcome some of Tahoe-LAFS' usability barriers by means of following features:

* Native, "batteries included" packaging on OS X and Windows -- Gridsync bundles will include Tahoe-LAFS and all required dependencies for a frictionless installation experience; no python installation or manual compilation is required
* A graphical user interface for managing all primary Tahoe-LAFS gateway functionality (e.g., starting, stopping, configuring nodes) -- the user will never have to edit a text file by hand or touch the command line
* Native look and feel -- Gridsync uses the Qt application framework, emulating native widgets on all target platforms; the user can expect Gridsync to behave like any other desktop application.
* Automated bi-directional file synchronization -- Gridsync will monitor local and remote directories, seamlessly storing or retrieving new versions of files as they appear (using Tahoe-LAFS' ``tahoe backup`` command and/or its forthcoming "Magic Folder" utility [*]_ ).
* Status indicators and desktop notifications -- the user will know, at a glance, when files are being uploaded or downloaded (via system tray icon animations) and will optionally receive notifications (via DBus on GNU/Linux, Notification Center on OS X, etc.) when operations have completed.
* 'One-click' sharing -- similar to BitTorrent ``magnet:`` links, the IANA-friendly `Gridsync URI specification`_ will allow users to easily join others' storage grids or to synchronize remote Tahoe-LAFS directories with the local filesystem.
* OS/Desktop-level integration -- Gridsync will (optionally) run at startup, install OS-level URI-handlers, and (eventually) provide context menus for sharing files directly in popular desktop file managers.

.. _Gridsync URI specification: https://github.com/gridsync/gridsync/blob/master/docs/uri_scheme.rst

.. [*] Tahoe-LAFS' "Magic Folder" functionality does not (yet) support Mac OS X or other BSD-based operating systems and is presently in pre-release/beta stage.


Sample screenshots
-----------

.. image:: https://raw.githubusercontent.com/gridsync/gridsync/master/images/screenshots/osx-bundle.png

.. image:: https://raw.githubusercontent.com/gridsync/gridsync/master/images/screenshots/osx-wizard.png

.. image:: https://raw.githubusercontent.com/gridsync/gridsync/master/images/screenshots/osx-notification.png

.. image:: https://raw.githubusercontent.com/gridsync/gridsync/master/images/screenshots/osx-menu.png


Current (complete -- or nearly complete) features:
--------------------------------------------------

* Native (.dmg/.app) installation for OS X (with Tahoe-LAFS and dependencies included)
* Single folder (.zip/.exe) bundle for Windows (with Tahoe-LAFS and dependencies included)
* Friendly setup wizard with pre-configured storage providers
* Background daemon to manage (create/start/stop) multiple local Tahoe-LAFS nodes
* Local filesystem monitoring (via `watchdog`_) for automated backups
* Remote filesystem monitoring for bi-directional synchronization (partial/broken; see warning below)
* System tray icon animations
* Desktop notifications
* Simple unified YAML configuration format

.. _watchdog: https://pypi.python.org/pypi/watchdog


In development / TODO / coming soon:
------------------------------------

* Unit/integration/system/user tests
* Drag-and-drop interface for folder management
* Integration of Tahoe-LAFS' "Magic Folders"
* `Magic-wormhole`_ support for simplified folder sharing.
* GNU/Linux distribution packaging (Debian, RPM, Arch PKGBUILD, Gentoo ebuilds, etc.)
* MSI/NSIS installer for Windows
* PyPI presence

.. _Magic-wormhole: https://github.com/warner/magic-wormhole

Planned features / coming later:
--------------------------------

* Tor integration and NAT traversal via onion services
* File manager/context menu integration for Finder (OS X), Explorer (Windows), Nautilus, Konqueror, Thunar, etc. (GNU/Linux)
* Visual/animated 'map' of shares distribution (think: a graphical version of https://bigasterisk.com/tahoe-playground/)


Known issues / caveats:
-----------------------

* Gridsync currently lacks a full test suite and the project, on the whole, should be considered alpha quality software at best. Expect major changes to the entire codebase before release.
* Presently, bi-directional sync works by calling ``tahoe backup`` on filesystem events and periodically polling the target/remote dircap for new snapshots (determining 'current' files based size and mtime). While some minimal conflict detection is in place and no local files are overwritten without first being stored remotely, this scheme is hackish and racey on the whole and should not be used for anything other than trivial, single-client backups (if at all). Consider this a placeholder for Tahoe-LAFS' upcoming "Magic Folders" functionality.
* Most items available through the systray menu are placeholders only. Again, expect everything here to change and/or go away in the future.
* Desktop notifications are currently spammy and trigger on every sync. These will also be fixed later to trigger on rare events (e.g., receiving a file update from another client, restoring from a previous snapshot, etc.)


Installation (development builds):
-------------

Linux (Debian-based systems):

1. ``apt-get install tahoe-lafs python3-pyqt5 python3-pip``
2. ``pip3 install git+https://github.com/gridsync/gridsync.git``

Mac OS X [*]_ :

1. Download `Gridsync.dmg`_
2. Drag the contained Gridsync.app bundle anywhere (e.g., `~/Applications`)

Windows (64-bit):

1. Download `Gridsync-win64.zip`_
2. Extract the contained Gridsync folder anywhere


.. _Gridsync.dmg: https://buildbot.gridsync.io/packages/Gridsync.dmg
.. _Gridsync-win64.zip: https://buildbot.gridsync.io/packages/Gridsync-win64.zip


.. [*] Mac OS X users may have to explicitly allow third-party apps in order to use Gridsync ("System Preferences" -> "Security & Privacy" -> "General" -> "Allow apps downloaded from:" -> "Anywhere").


Running:
--------

Linux:

* From the command-line: ``gridsync`` (or ``gridsync --help`` for available options)

Mac OS X:

* Double click ``Gridsync.app``

Windows:

* Double click ``Gridsync.exe``


Contributing:
-------------

Contributions of any sort (e.g., suggestions, criticisms, bug reports, pull requests) are more than welcome. Any persons interested in aiding the development of Gridsync are encouraged to do so by opening a `GitHub Issue`_ or by contacting its primary developer: `chris@gridsync.io`_

.. _GitHub Issue: https://github.com/crwood/gridsync/issues
.. _chris@gridsync.io: mailto:chris@gridsync.io

License:
--------

Copyright (C) 2015-2016  Christopher R. Wood

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, write to the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
