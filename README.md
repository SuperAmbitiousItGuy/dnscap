# dnscap README

## Building from Git repository

If you are building `dnscap` from it's Git repository you will first need
to initiate the Git submodules that exists and later create autoconf/automake
files.

```
git clone https://github.com/DNS-OARC/dnscap.git
cd dnscap
git submodule update --init
./autogen.sh
./configure [options]
```

## Linking with libbind

If you plan to use dnscap's -x/-X features, then you might need
to have libbind installed.   These features use functions such
as ns_parserr().  On some systems these functions will be found
in libresolv.  If not, then you might need to install libbind.
I suggest first building dnscap on your system as-is, then run

```$ ./dnscap -x foo```

If you see an error, install libbind either from your
OS package system or by downloading the source from
http://www.isc.org/downloads/current

## 64-bit libraries

If you need to link against 64-bit libraries found in non-standard
locations, provide the location by setting LDFLAGS before running
configure:

```$ env LDFLAGS=-L/usr/lib64 ./configure```


## FreeBSD (and other BSDs?)

If you've installed libbind for -x/-X then it probably went into
/usr/local and you'll need to tell configure how to find it:

```$ env CFLAGS=-I/usr/local/include LDFLAGS=-L/usr/local/lib ./configure```

Also note that we have observed significant memory leaks on FreeBSD
(7.2) when using -x/-X.  To rectify:

1. cd /usr/ports/dns/libbind
1. make config
1. de-select "Compile with thread support"
1. reinstall the libbind port
1. recompile and install dnscap
