AC_INIT([libvmod-leveldb], [0.1.0])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_SRCDIR(src/vmod.vcc)
AM_CONFIG_HEADER(config.h)
m4_ifndef([VARNISH_VMOD_INCLUDES], AC_MSG_ERROR([varnish.m4 is required.]))

AM_INIT_AUTOMAKE([foreign color-tests parallel-tests])

AC_DISABLE_STATIC

AC_PROG_CC
AC_PROG_LIBTOOL

AC_ARG_ENABLE([gcov],
  [AS_HELP_STRING([--enable-gcov],
     [enable code coverage analysis (default=no)])],
  [AC_PATH_PROG([LCOV], [lcov])
   AC_PATH_PROG([GENHTML], [genhtml])
   CFLAGS="$CFLAGS -fprofile-arcs -ftest-coverage"])
AM_CONDITIONAL(USE_LCOV, [test x$LCOV != x && test x$GENHTML != x])

AC_CHECK_LIB([leveldb], [leveldb_open], [], [
  AC_MSG_ERROR([leveldb is required.])
])

AC_PATH_PROGS([PYTHON], [python3 python2.7 python2.6])
test -z "$PYTHON" && AC_MSG_ERROR([Python 2.6 or greater is required.])

AC_PATH_PROG([RST2MAN], [rst2man])
test -z "$RST2MAN" && AC_MSG_ERROR([rst2man is required.])

PKG_CHECK_VAR([VARNISH_BINDIR], [varnishapi], [bindir])
PKG_CHECK_VAR([VARNISH_SBINDIR], [varnishapi], [sbindir])

VARNISH_VMOD_INCLUDES
VARNISH_VMOD_DIR
VARNISH_VMODTOOL

AC_PATH_PROG([VARNISHTEST], [varnishtest], [], [$VARNISH_BINDIR:$PATH])
AC_PATH_PROG([VARNISHD], [varnishd], [], [$VARNISH_SBINDIR:$PATH])

VMOD_TESTS="$(cd $srcdir/src && echo tests/*.vtc)"
AC_SUBST(VMOD_TESTS)

AC_CONFIG_FILES([Makefile src/Makefile])
AC_OUTPUT
