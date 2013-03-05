AC_PREREQ([2.61])
AC_INIT(tempi-plugins-mikado, [0.3.1], [https://github.com/aalex/tempi-plugins-mikado/issues], [tempi-plugins-mikado], [https://github.com/aalex/tempi-plugins-mikado])
AC_CONFIG_MACRO_DIR([m4])
AC_MSG_NOTICE([Configuring tempi-plugins-mikado])
AM_INIT_AUTOMAKE([subdir-objects -Wno-portability])
AC_CONFIG_HEADERS([plugins-mikado/config.h])
AC_CANONICAL_HOST # fills $host variable
# Process this file with autoconf to produce a configure script.

m4_define([tempi_api_version], [0.3])
AC_SUBST([TEMPI_API_VERSION],    [tempi_api_version])
AC_SUBST([PLUGINS_MIKADO_LT_VERSION], [0:0:0])

# Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LIBTOOL

AC_CHECK_PROGS([DOXYGEN], [doxygen])
if test -z "$DOXYGEN";
    then AC_MSG_WARN([Doxygen not found - continuing without Doxygen support])
fi
AM_CONDITIONAL([HAVE_DOXYGEN], [test -n "$DOXYGEN"])

# Special cases, copied from SPIN
case "$host" in *-darwin* | *-macos10*)
  #AC_LANG_DEFINES_PROVIDED # remove autogen.sh warnings on OSX
  have_osx=true
  AM_CONDITIONAL([HAVE_OSX], true)
  AC_DEFINE(HAVE_OSX,[1],[Apple Mac OS X operating system detected])
  AC_MSG_NOTICE([Apple Mac OS X operating system detected])
  LIBTOOL="glibtool"
  SRCDIR=`pwd`
  #LIBTOOL="libtool"
  CPPFLAGS="$CPPFLAGS -D__Darwin"
  LDFLAGS="-undefined dynamic_lookup -Wl,-headerpad_max_install_names"
  if test -d /opt/local ; then
    CPPFLAGS="$CPPFLAGS -I/usr/local/include -I/opt/local/include"
    LDFLAGS="$LDFLAGS -L/usr/local/lib -L/opt/local/lib"
  elif test -d /sw ; then
    CPPFLAGS="$CPPFLAGS -I/sw/include"
    LDFLAGS="$LDFLAGS -L/sw/lib"
  fi
  # OSX does not include /usr/local/lib/pkgconfig path, so we export it here
  # (for everyone, just in case)
  # TODO: also need to add the path for linux64
  PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
  export PKG_CONFIG_PATH
  ;;
*)
  AM_CONDITIONAL([HAVE_OSX], false)
  dnl : evil:
  dnl AC_DEFINE(HAVE_OSX,[0],[Apple Mac OS X operating system not detected])
  AC_MSG_NOTICE([You are not on Apple Mac OS X.])
  ;;
esac

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_SIZE_T
AC_CHECK_TYPES([ptrdiff_t])

# Checks for libraries
AC_CHECK_LIB([m],[sqrt])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h stddef.h stdlib.h string.h sys/time.h])

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_CHECK_FUNCS([gettimeofday memmove mkdir setenv sqrt strchr strpbrk uname])

dnl AX_PTHREAD

# Tempi
PKG_CHECK_MODULES([TEMPI], [tempi-0.3], have_tempi=true, have_tempi=false)
if test "x${have_tempi}" = "xfalse" ; then
    AC_MSG_WARN([missing package: libtempi-0.3-dev])
else
    AC_DEFINE(HAVE_GLIB,[1],[Tempi headers successfully detected])
fi

# GEGL
PKG_CHECK_MODULES([LIBGEGL], [gegl], have_gegl=true, have_gegl=false)
if test "x${have_gegl}" = "xfalse" ; then
    AC_MSG_ERROR([missing package: libgegl-dev])
fi

CFLAGS+=" -fPIC -Wall -g3 -O0" dnl -Wextra -Wfatal-errors -Werror "

# Override the template file name of the generated .pc file, so that there
# is no need to rename the template file when the API version changes.
AC_CONFIG_FILES([
        Makefile 
        plugins-mikado/Makefile
        plugins-mikado/tests/Makefile
        ])

AC_OUTPUT

