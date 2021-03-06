#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.61)
AC_INIT([dnscap], [1.1.0], [dnscap-users@dns-oarc.net], [dnscap], [https://github.com/DNS-OARC/dnscap/issues])
AM_INIT_AUTOMAKE([-Wall -Werror foreign subdir-objects])
AC_CONFIG_SRCDIR([src/dnscap.c])
AC_CONFIG_HEADER([src/config.h])
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
AC_CANONICAL_HOST
AM_PROG_AR
LT_INIT([disable-static])

# pcap_thread
AX_PCAP_THREAD

# Checks for libraries.
AC_CHECK_LIB([bind], [ns_initparse], [], [AC_CHECK_LIB([bind], [__ns_initparse])])
AC_CHECK_LIB([resolv], [res_mkquery], [], [
	AC_CHECK_LIB([resolv], [__res_mkquery], [], [
		AC_CHECK_LIB([resolv], [res_9_mkquery])
	])
])
AC_CHECK_LIB([dl], [dlopen])
have_ldns=
AC_CHECK_LIB([ldns], [ldns_wire2pkt], [have_ldns=yes])
AM_CONDITIONAL([LDNS], [test "x$have_ldns" = "xyes"])

# Check for OS specific libraries
case "$host_os" in
    # HPUX
    hpux*)
        AC_CHECK_LIB([hplx], [main])
        ;;

    # Solaris
    solaris*)
        AC_CHECK_LIB([rt], [main])
        AC_CHECK_LIB([md5], [main])
        AC_CHECK_LIB([socket], [main])
        AC_CHECK_LIB([nsl], [main])
        ;;
esac

# Checks for header files.
AC_HEADER_RESOLV
AC_CHECK_HEADERS([arpa/inet.h fcntl.h netdb.h netinet/in.h stdlib.h string.h])
AC_CHECK_HEADERS([sys/ioctl.h sys/param.h sys/socket.h sys/time.h unistd.h])
AC_CHECK_HEADERS([ldns/ldns.h arpa/nameser_compat.h])

# Checks for library functions.
AC_CHECK_FUNCS([snprintf])
AC_CHECK_FUNCS([setreuid setresuid setregid setresgid setegid seteuid])
AC_CHECK_FUNC([ns_initparse],
    [AC_DEFINE([HAVE_NS_INITPARSE], [1], [Define to 1 if you have the `ns_initparse' function.])],
    AC_CHECK_FUNC(__ns_initparse,
        [AC_DEFINE([HAVE_NS_INITPARSE], [1], [Define to 1 if you have the `ns_initparse' function.])]
    )
)
AC_CHECK_FUNC([ns_parserr],
    [AC_DEFINE([HAVE_NS_PARSERR], [1], [Define to 1 if you have the `ns_parserr' function.])],
    AC_CHECK_FUNC(__ns_parserr,
        [AC_DEFINE([HAVE_NS_PARSERR], [1], [Define to 1 if you have the `ns_parserr' function.])]
    )
)
AC_CHECK_FUNC([ns_sprintrr],
    [AC_DEFINE([HAVE_NS_SPRINTRR], [1], [Define to 1 if you have the `ns_sprintrr' function.])],
    AC_CHECK_FUNC(__ns_sprintrr,
        [AC_DEFINE([HAVE_NS_SPRINTRR], [1], [Define to 1 if you have the `ns_sprintrr' function.])]
    )
)
AC_CHECK_FUNC([ns_name_uncompress],
    [AC_DEFINE([HAVE_NS_NAME_UNCOMPRESS], [1], [Define to 1 if you have the `ns_name_uncompress' function.])],
    AC_CHECK_FUNC(__ns_name_uncompress,
        [AC_DEFINE([HAVE_NS_NAME_UNCOMPRESS], [1], [Define to 1 if you have the `ns_name_uncompress' function.])]
    )
)
AC_CHECK_FUNC([p_rcode],
    [AC_DEFINE([HAVE_P_RCODE], [1], [Define to 1 if you have the `p_rcode' function.])],
    AC_CHECK_FUNC(__p_rcode,
        [AC_DEFINE([HAVE_P_RCODE], [1], [Define to 1 if you have the `p_rcode' function.])]
    )
)
AC_CHECK_FUNCS([__assertion_failed])

# Check for SECCOMP
SECCOMPFLAGS=
AC_ARG_ENABLE(seccomp, AC_HELP_STRING([--enable-seccomp], [Linux seccomp-bpf sandbox]))
case "$enable_seccomp" in
    yes)
        AC_DEFINE_UNQUOTED([USE_SECCOMP], [1], [Define this to enable Linux seccomp-bpf sandbox.])
        SECCOMPFLAGS="-lseccomp -fPIE -fstack-protector-all -Wl,-z,relro -Wformat -Wformat-security -Werror=format-security -D_FORTIFY_SOURCE=2"
        ;;

    *)
        ;;
esac
AC_SUBST(SECCOMPFLAGS, ["$SECCOMPFLAGS"])

# Output Makefiles
AC_CONFIG_FILES([
    Makefile
    src/Makefile
    plugins/Makefile
    plugins/pcapdump/Makefile
    plugins/rssm/Makefile
    plugins/txtout/Makefile
])
AC_OUTPUT
