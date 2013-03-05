SUBDIRS = tests

AM_CXXFLAGS = \
    $(TEMPI_CFLAGS) \
    $(GEGL_CFLAGS) \
    -I$(top_srcdir)

AM_LIBS = \
    $(TEMPI_LIBS) \
    $(GEGL_LIBS)

# use lib_LTLIBRARIES to build a shared lib:
lib_LTLIBRARIES = libtempi-@TEMPI_API_VERSION@-plugins-mikado.la

## Instruct libtool to include ABI version information in the generated shared
## library file (.so).  The library ABI version is defined in configure.ac, so
## that all version information is kept in one place.
libtempi_@TEMPI_API_VERSION@_plugins_mikado_la_LDFLAGS = -export-dynamic -version-info $(PLUGINS_MIKADO_LT_VERSION) $(AM_LIBS) $(LDADD)
libtempi_@TEMPI_API_VERSION@_plugins_mikado_la_LIBS = $(AM_LIBS)
libtempi_@TEMPI_API_VERSION@_plugins_mikado_la_CXXFLAGS = $(AM_CXXFLAGS)
libtempi_@TEMPI_API_VERSION@_plugins_mikado_la_includedir = $(includedir)/tempi-$(TEMPI_API_VERSION)

## Define the source file list for the "libtempi-@TEMPI_API_VERSION@.la"
## target.  Note that @TEMPI_API_VERSION@ is not interpreted by Automake and
## will therefore be treated as if it were literally part of the target name,
## and the variable name derived from that.
## PLEASE KEEP THEM IN ALPHABETICAL ORDER
libtempi_@TEMPI_API_VERSION@_plugins_mikado_la_SOURCES = \
    hello/getversion.cpp \
    mikado_library.cpp

EXTRA_DIST = \
    hello/getversion.h \
    mikado_library.h
