# WAZN

Copyright (c) 2019 WAZN Project  
Copyright (c) 2018-2019 uPlexa  
Copyright (c) 2014-2018 The Monero Project  
Copyright (c) 2012-2013 The Cryptonote developers

## Coverage

| Type      | Status |
|-----------|--------|
| License   | [![License](https://img.shields.io/badge/license-GPL--3.0-blue)](https://opensource.org/licenses/GPL-3.0)

## Builds

Ubuntu-x64
Win-x64

## Development resources

- Web: [wazn.io](https://wazn.io)
- Mail: [dev@wazn.io](mailto:dev@wazn.io)
- GitHub: [github.com/Project-WAZN/WAZN](https://github.com/Project-WAZN/WAZN)

## Introduction

WAZN is a private, secure, untraceable, decentralised digital currency. You are your bank, you control your funds, and nobody can trace your transfers unless you allow them to do so.

**Privacy:** WAZN uses a cryptographically sound system to allow you to send and receive funds without your transactions being easily revealed on the blockchain (the ledger of transactions that everyone has). This ensures that your purchases, receipts, and all transfers remain absolutely private by default.

**Security:** Using the power of a distributed peer-to-peer consensus network, every transaction on the network is cryptographically secured. Individual wallets have a 25 word mnemonic seed that is only displayed once, and can be written down to backup the wallet. Wallet files are encrypted with a passphrase to ensure they are useless if stolen.

**Untraceability:** By taking advantage of ring signatures, a special property of a certain type of cryptography, WAZN is able to ensure that transactions are not only untraceable, but have an optional measure of ambiguity that ensures that transactions cannot easily be tied back to an individual user or computer.

## Announcements

- Subscribe to our Reddit for important announcements regarding WAZN (https://www.reddit.com/r/WAZN/)

## About this project

This is the core implementation of WAZN. It is open source and completely free to use without restrictions, except for those specified in the license agreement above. There are no restrictions on anyone creating an alternative implementation of WAZN that uses the protocol and network in a compatible manner.

As with many development projects, the repository on Github is considered to be the "staging" area for the latest changes. Before changes are merged into that branch on the main repository, they are tested by individual developers in their own branches, submitted as a pull request, and then subsequently tested by contributors who focus on testing and code reviews. That having been said, the repository should be carefully considered before using it in a production environment, unless there is a patch in the repository for a particular show-stopping issue you are experiencing. It is generally a better idea to use a tagged release for stability.

**Anyone is welcome to contribute to WAZN's codebase!** If you have a fix or code change, feel free to submit it as a pull request directly to the "master" branch. In cases where the change is relatively small or does not affect other parts of the codebase it may be merged in immediately by any one of the collaborators. On the other hand, if the change is particularly large or complex, it is expected that it will be discussed at length either well in advance of the pull request being submitted, or even directly on the pull request.

## Contributing

If you want to help out, see [CONTRIBUTING](CONTRIBUTING.md) for a set of guidelines.

## Compiling WAZN from source

### Dependencies

The following table summarizes the tools and libraries required to build. A
few of the libraries are also included in this repository (marked as
"Vendored"). By default, the build uses the library installed on the system,
and ignores the vendored sources. However, if no library is found installed on
the system, then the vendored source will be built and used. The vendored
sources are also used for statically-linked builds because distribution
packages often include only shared library binaries (`.so`) but not static
library archives (`.a`).

[1] On Debian/Ubuntu `libgtest-dev` only includes sources and headers. You must
build the library binary manually. This can be done with the following command ```sudo apt-get install libgtest-dev && cd /usr/src/gtest && sudo cmake . && sudo make && sudo mv libg* /usr/lib/ ```  
[2] libnorm-dev is needed if your zmq library was built with libnorm, and not needed otherwise

Debian / Ubuntu one liner for all dependencies:  
``` sudo apt update && sudo apt install build-essential cmake pkg-config libboost-all-dev libssl-dev libzmq3-dev libunbound-dev libsodium-dev libunwind8-dev liblzma-dev libreadline6-dev libldns-dev libexpat1-dev doxygen graphviz libpgm-dev```

### Cloning the repository

Clone recursively to pull-in needed submodule(s):

`$ git clone --recursive https://github.com/Project-WAZN/WAZN`

If you already have a repo cloned, initialize and update:

`$ cd WAZN && git submodule init && git submodule update`

### Build instructions

WAZN uses the CMake build system and a top-level [Makefile](Makefile) that
invokes cmake commands as needed.

#### On Linux and OS X

* Install the dependencies
* Change to the root of the source code directory, change to the most recent release branch, and build:

        cd WAZN
        git checkout stable
        make release

*Optional*: If your machine has several cores and enough memory, enable
parallel build by running `make -j<number of threads>` instead of `make`. For
this to be worthwhile, the machine should have one core and about 2GB of RAM
available per thread.

*Note*: If cmake can not find zmq.hpp file on OS X, installing `zmq.hpp` from
https://github.com/zeromq/cppzmq to `/usr/local/include` should fix that error.

*Note*: The instructions above will compile the most stable release of the
WAZN software. If you would like to use and test the most recent software,
use ```git checkout master```. The master branch may contain updates that are
both unstable and incompatible with release software, though testing is always
encouraged. If you would like to checkout the most stable branch, please use ```git checkout stable```

* The resulting executables can be found in `build/release/bin`

* Add `PATH="$PATH:$HOME/WAZN/build/release/bin"` to `.profile`

* Run WAZN with `WAZNd --detach`

* **Optional**: build and run the test suite to verify the binaries:

        make release-test

    *NOTE*: `core_tests` test may take a few hours to complete.

* **Optional**: to build binaries suitable for debugging:

         make debug

* **Optional**: to build statically-linked binaries:

         make release-static

Dependencies need to be built with -fPIC. Static libraries usually aren't, so you may have to build them yourself with -fPIC. Refer to their documentation for how to build them.

* **Optional**: build documentation in `doc/html` (omit `HAVE_DOT=YES` if `graphviz` is not installed):

        HAVE_DOT=YES doxygen Doxyfile

#### On Windows:

Binaries for Windows are built on Windows using the MinGW toolchain within
[MSYS2 environment](https://www.msys2.org). The MSYS2 environment emulates a
POSIX system. The toolchain runs within the environment and *cross-compiles*
binaries that can run outside of the environment as a regular Windows
application.

**Preparing the build environment**

* Download and install the [MSYS2 installer](https://www.msys2.org), either the 64-bit or the 32-bit package, depending on your system.
* Open the MSYS shell via the `MSYS2 Shell` shortcut
* Update packages using pacman:  

        pacman -Syuu

* Exit the MSYS shell using Alt+F4  
* Edit the properties for the `MSYS2 Shell` shortcut changing "msys2_shell.bat" to "msys2_shell.cmd -mingw64" for 64-bit builds or "msys2_shell.cmd -mingw32" for 32-bit builds
* Restart MSYS shell via modified shortcut and update packages again using pacman:  

        pacman -Syuu

* Install dependencies:

To build for 64-bit Windows:

        pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake mingw-w64-x86_64-boost mingw-w64-x86_64-openssl mingw-w64-x86_64-zeromq mingw-w64-x86_64-libsodium mingw-w64-x86_64-hidapi

To build for 32-bit Windows:

        pacman -S mingw-w64-i686-toolchain make mingw-w64-i686-cmake mingw-w64-i686-boost mingw-w64-i686-openssl mingw-w64-i686-zeromq mingw-w64-i686-libsodium mingw-w64-i686-hidapi

* Open the MingW shell via `MinGW-w64-Win64 Shell` shortcut on 64-bit Windows
  or `MinGW-w64-Win64 Shell` shortcut on 32-bit Windows. Note that if you are
  running 64-bit Windows, you will have both 64-bit and 32-bit MinGW shells.

**Cloning**

* To git clone, run:

        git clone --recursive https://github.com/Project-WAZN/WAZN

**Building**

* Change to the cloned directory, run:

        cd WAZN

* If you would like a specific [version/tag](https://github.com/Project-WAZN/WAZN/tags), do a git checkout for that version. eg. 'v1.0.1'. If you don't care about the version and just want binaries from master, skip this step:

        git checkout stable

* If you are on a 64-bit system, run:

        make release-static-win64

* If you are on a 32-bit system, run:

        make release-static-win32

* The resulting executables can be found in `build/release/bin`

* **Optional**: to build Windows binaries suitable for debugging on a 64-bit system, run:

        make debug-static-win64

* **Optional**: to build Windows binaries suitable for debugging on a 32-bit system, run:

        make debug-static-win32

* The resulting executables can be found in `build/debug/bin`

#### On Linux for Android (using docker):

        # Build image (for ARM 32-bit)
        docker build -f utils/build_scripts/android32.Dockerfile -t uplexa-android .
        # Build image (for ARM 64-bit)
        docker build -f utils/build_scripts/android64.Dockerfile -t uplexa-android .
        # Create container
        docker create -it --name uplexa-android uplexa-android bash
        # Get binaries
        docker cp uplexa-android:/src/build/release/bin .

### Building portable statically linked binaries (Cross Compiling)

By default, in either dynamically or statically linked builds, binaries target the specific host processor on which the build happens and are not portable to other processors. Portable binaries can be built using the following targets:

* ```make release-static-linux-x86_64``` builds binaries on Linux on x86_64 portable across POSIX systems on x86_64 processors
* ```make release-static-linux-i686``` builds binaries on Linux on x86_64 or i686 portable across POSIX systems on i686 processors
* ```make release-static-linux-armv8``` builds binaries on Linux portable across POSIX systems on armv8 processors
* ```make release-static-linux-armv7``` builds binaries on Linux portable across POSIX systems on armv7 processors
* ```make release-static-linux-armv6``` builds binaries on Linux portable across POSIX systems on armv6 processors
* ```make release-static-win64``` builds binaries on 64-bit Windows portable across 64-bit Windows systems
* ```make release-static-win32``` builds binaries on 64-bit or 32-bit Windows portable across 32-bit Windows systems

### Cross Compiling

You can also cross-compile static binaries on Linux for Windows and macOS with the `depends` system.

* ```make depends target=x86_64-linux-gnu``` for 64-bit linux binaries.
* ```make depends target=x86_64-w64-mingw32``` for 64-bit windows binaries. Requires: python3 g++-mingw-w64-x86-64 wine1.6 bc
* ```make depends target=x86_64-apple-darwin11``` for macOS binaries. Requires: cmake imagemagick libcap-dev librsvg2-bin libz-dev libbz2-dev libtiff-tools python-dev
* ```make depends target=i686-linux-gnu``` for 32-bit linux binaries. Requires: g++-multilib bc
* ```make depends target=i686-w64-mingw32``` for 32-bit windows binaries. Requires: python3 g++-mingw-w64-i686
* ```make depends target=arm-linux-gnueabihf``` for armv7 binaries. Requires: g++-arm-linux-gnueabihf
* ```make depends target=aarch64-linux-gnu``` for armv8 binaries. Requires: g++-aarch64-linux-gnu

The required packages are the names for each toolchain on apt. Depending on your distro, they may have different names.

Using `depends` might also be easier to compile WAZN on Windows than using MSYS. Activate Windows Subsystem for Linux (WSL) with a distro (for example Ubuntu), install the apt build-essentials and follow the `depends` steps as depicted above.

The produced binaries still link libc dynamically. If the binary is compiled on a current distribution, it might not run on an older distribution with an older installation of libc. Passing `-DBACKCOMPAT=ON` to cmake will make sure that the binary will run on systems having at least libc version 2.17.

## Running waznd

The build places the binary in `bin/` sub-directory within the build directory
from which cmake was invoked (repository root by default). To run in
foreground:

    ./bin/waznd

To list all available options, run `./bin/waznd --help`.  Options can be
specified either on the command line or in a configuration file passed by the
`--config-file` argument.  To specify an option in the configuration file, add
a line with the syntax `argumentname=value`, where `argumentname` is the name
of the argument without the leading dashes, for example `log-level=1`.

To run in background:

    ./bin/waznd --log-file waznd.log --detach

To run as a systemd service, copy
[waznd.service](utils/systemd/waznd.service) to `/etc/systemd/system/` and
[waznd.conf](utils/conf/waznd.conf) to `/etc/`. The [example
service](utils/systemd/waznd.service) assumes that the user `wazn` exists
and its home is the data directory specified in the [example
config](utils/conf/waznd.conf).

If you're on Mac, you may need to add the `--max-concurrency 1` option to
wazn-wallet-cli, and possibly waznd, if you get crashes refreshing.

## Using Tor

While WAZN isn't made to integrate with Tor, it can be used wrapped with torsocks, by
setting the following configuration parameters and environment variables:

* `--p2p-bind-ip 127.0.0.1` on the command line or `p2p-bind-ip=127.0.0.1` in
  waznd.conf to disable listening for connections on external interfaces.
* `--no-igd` on the command line or `no-igd=1` in waznd.conf to disable IGD
  (UPnP port forwarding negotiation), which is pointless with Tor.
* `DNS_PUBLIC=tcp` or `DNS_PUBLIC=tcp://x.x.x.x` where x.x.x.x is the IP of the
  desired DNS server, for DNS requests to go over TCP, so that they are routed
  through Tor. When IP is not specified, waznd uses the default list of
  servers defined in [src/common/dns_utils.cpp](src/common/dns_utils.cpp).
* `TORSOCKS_ALLOW_INBOUND=1` to tell torsocks to allow waznd to bind to interfaces
   to accept connections from the wallet. On some Linux systems, torsocks
   allows binding to localhost by default, so setting this variable is only
   necessary to allow binding to local LAN/VPN interfaces to allow wallets to
   connect from remote hosts. On other systems, it may be needed for local wallets
   as well.
* Do NOT pass `--detach` when running through torsocks with systemd, (see
  [utils/systemd/waznd.service](utils/systemd/waznd.service) for details).
* If you use the wallet with a Tor daemon via the loopback IP (eg, 127.0.0.1:9050),
  then use `--untrusted-daemon` unless it is your own hidden service.

Example command line to start waznd through Tor:

    DNS_PUBLIC=tcp torsocks waznd --p2p-bind-ip 127.0.0.1 --no-igd


### Using Tor on Tails

TAILS ships with a very restrictive set of firewall rules. Therefore, you need
to add a rule to allow this connection too, in addition to telling torsocks to
allow inbound connections. Full example:

    sudo iptables -I OUTPUT 2 -p tcp -d 127.0.0.1 -m tcp --dport 18081 -j ACCEPT
    DNS_PUBLIC=tcp torsocks ./uplexad --p2p-bind-ip 127.0.0.1 --no-igd --rpc-bind-ip 127.0.0.1 \
        --data-dir /home/amnesia/Persistent/your/directory/to/the/blockchain
