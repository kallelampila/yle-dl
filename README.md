Download videos from Yle servers

[![Build Status](https://travis-ci.org/aajanki/yle-dl.svg?branch=master)](https://travis-ci.org/aajanki/yle-dl)

Copyright (C) 2010-2016 Antti Ajanki, antti.ajanki@iki.fi

License: GPLv3

Homepage: https://aajanki.github.com/yle-dl/index-en.html

Source code: https://github.com/aajanki/yle-dl

yle-dl is a tool for downloading media files from the video streaming
services of the Finnish national broadcasting company Yle: [Yle
Areena], [Elävä Arkisto] and [Yle news].

[Yle Areena]:https://areena.yle.fi/
[Elävä arkisto]:http://yle.fi/aihe/elava-arkisto
[Yle news]:http://yle.fi/uutiset/

Installation
------------

### 1. Install the dependencies ###

* python (2.6 or newer)
* pycrypto
* PHP interpreter
* PHP extensions: bcmath, curl, mcrypt and SimpleXML

Optionally (for downloading Areena audio streams):

* rtmpdump (version 2.4 or newer, preferably the latest development version from the project homepage at https://rtmpdump.mplayerhq.hu/)

Enable the PHP extensions by appending the following lines with the
correct paths in the [php.ini]:
[php.ini]:https://secure.php.net/manual/en/configuration.file.php

```
extension=/path/to/curl.so
extension=/path/to/mcrypt.so
```

### 2. Install yle-dl ###

```
sudo make install
```

Installation on Debian unstable (May 2016)/Ubuntu 16.04
-------------------------------------------------------

### 1. Install the dependencies ###

```
sudo apt-get install rtmpdump python python-crypto php-cli php-curl php-mcrypt php-xml php-bcmath
sudo phpenmod mcrypt
```

### 2. Install yle-dl ###

```
sudo make install
```

Installation on Debian Jessie (8.5)/Ubuntu 15.10 or older
---------------------------------------------------------

### 1. Install the dependencies ###

```
sudo apt-get install rtmpdump python python-crypto php5-cli php5-curl php5-mcrypt
sudo php5enmod mcrypt
```

### 2. Install yle-dl ###

```
sudo make install
```

Installation on Mac OS X
------------------------

### 1. Install the dependencies ###

[Install the PHP interpreter](https://secure.php.net/manual/en/install.macosx.php) and the other libraries:

```
brew install --HEAD rtmpdump
brew install homebrew/php/php55-mcrypt
pip install pycrypto
```

Enable the PHP extensions by appending the following lines with the
correct paths in the [php.ini]:
[php.ini]:https://secure.php.net/manual/en/configuration.file.php

```
extension=/path/to/curl.so
extension=/path/to/mcrypt.so
```

### 2. Install yle-dl ###

```
make install
```

Installation with youtube-dl as an alternative downloader backend
-----------------------------------------------------------------

By default, yle-dl downloads streams from Yle Areena using the
included copy of AdobeHDS.php. If the default downloader does not work
for some reason, it is possible to use youtube-dl (for Python 2)
instead. yle-dl will automatically fall back to youtube-dl if it is
installed and downloading with AdobeHDS.php fails.

Follow the above installation instructions (except for the PHP and the
PHP libraries) and additionally install youtube-dl:

* Debian/Ubuntu: `pip install --upgrade youtube_dl`
* Mac OS X: `brew install youtube-dl`
* Other operating systems: `pip install --upgrade youtube_dl`

Uninstalling earlier versions
-----------------------------

Starting from version 1.99.9, yle-dl doesn't anymore require a
modified rtmpdump or plugin. Instead, everything is now downloadable
with the plain rtmpdump. To remove the remnants of previous versions
run `make uninstall-old-rtmpdump`.

Packages for various distros
----------------------------

[A list of available
packages](https://aajanki.github.com/yle-dl/index-en.html)

RPM packaging:

contrib/yle-dl.spec is a spec file for creating a RPM-package for
Fedora.

Usage
-----

```
yle-dl [options] URL
```

or

```
yle-dl [options] -i filename
```

where URL is the address of the Areena or Elävä arkisto web page where
you would normally watch the video in a browser.

yle-dl options:

* `-o filename`       Save stream to the named file

* `-i filename`       Read input URLs to process from the named file, one URL per line

* `--latestepisode`   Download the latest episodes

* `--showurl`         Print the URL of the stream, don't download

* `--showtitle`       Print stream title, don't download

* `--vfat`            Create Windows-compatible filenames

* `--audiolang lang`  Select stream's audio language if available, lang = fin (default) or swe

* `--sublang lang`    Download stream's subtitle language, lang = fin, swe, smi, none or all (default)

* `--maxbitrate br`   Maximum bitrate stream to download, integer in kB/s or "best" or "worst". Not all streams support limited bitrates. Only approximate when downloading using AdobeHDS.php.

* `--rtmpdump path`   Set path to rtmpdump binary

* `--adobehds cmd`    Set command for executing AdobeHDS.php script

* `--postprocess cmd` Execute a command cmd after a successful download. The command is called with the downloaded FLV file as the first parameter and subtitle files (if any) as the following parameters.

* `--proxy uri`       Proxy for downloading stream manifests. Example: `--proxy socks5://localhost:7777`

* `--destdir dir`     Save files to dir

* `--pipe`            Dump stream to stdout for piping to media player. E.g. `yle-dl --pipe URL | vlc -`.

* `--backend vals`    Downloaders that are tried until one of them succeeds (a comma-separated list). Possible values: `adobehdsphp` (download HDS streams using AdobeHDS.php), `youtubedl` (download HDS streams using youtube-dl).

* `-V, --verbose`     Show verbose debug output

Type `yle-dl --help` to see the full list of options.

Any unrecognized options will be relayed to rtmpdump process (when
downloading RTMP streams).

Firewall must allow outgoing traffic on ports 80 and 1935.

Examples
--------

```
yle-dl https://areena.yle.fi/1-1544491 -o video.flv
```

```
yle-dl --backend youtubedl https://areena.yle.fi/1-1544491 -o video.flv
```

```
yle-dl http://yle.fi/aihe/artikkeli/2010/10/28/studio-julmahuvi-roudasta-rospuuttoon
```

Playing in mplayer (or vlc and others) without downloading first:

```
yle-dl --pipe https://areena.yle.fi/1-2409251 | mplayer -cache 1024 -
```

Executing a script to postprocess a downloaded video (see the example postprocessing script at scripts/muxmp4):

```
yle-dl --postprocess scripts/muxmp4 https://areena.yle.fi/1-1864726
```
