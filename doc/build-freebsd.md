Building on FreeBSD
--------------------

(Updated as of FreeBSD 11.0)

This guide describes how to build gridcoinresearchdd, command-line utilities, and GUI on FreeBSD.

Preparing the Build
--------------------

Install the required dependencies the usual way you [install software on FreeBSD](https://www.freebsd.org/doc/en/books/handbook/ports.html) - either with `pkg` or via the Ports collection. The example commands below use `pkg` which is usually run as `root` or via `sudo`. If you want to use `sudo`, and you haven't set it up: [use this guide](http://www.freebsdwiki.net/index.php/Sudo%2C_configuring) to setup `sudo` access on FreeBSD.

```bash
pkg install autoconf automake boost-libs git gmake libevent libtool pkgconf

```
Clone the repository and cd into it:

``` bash
git clone https://github.com/gridcoin-community/Gridcoin-Research
cd Gridcoin-Research
git checkout master
```
To Build
---------------------

You need to use GNU make (`gmake`) instead of `make`.
(`libressl` instead of `openssl` will also work)

For the wallet (optional):

    pkg install db5

This will give a warning "configure: WARNING: Found Berkeley DB other
than 4.8; wallets opened by this build will not be portable!", but as FreeBSD never
had a binary release, this may not matter.

Then build using:

    ./autogen.sh
    ./configure --with-incompatible-bdb BDB_CFLAGS="-I/usr/local/include/db5" BDB_LIBS="-L/usr/local/lib -ldb_cxx-5"
    gmake

*Note on debugging*: The version of `gdb` installed by default is [ancient and considered harmful](https://wiki.freebsd.org/GdbRetirement).
It is not suitable for debugging a multi-threaded C++ program, not even for getting backtraces. Please install the package `gdb` and
use the versioned gdb command e.g. `gdb7111`.
