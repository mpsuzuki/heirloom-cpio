AC_PREREQ([2.62])
AC_INIT([heirloom-cpio], [0.0.20251117], [https://github.com/mpsuzuki/heirloom-cpio/-/issues])
AM_INIT_AUTOMAKE([foreign])

# Set default prefix
AC_PREFIX_DEFAULT(["/usr/ccs"])

# Checks for programs.
AC_PROG_CC
AC_PROG_MAKE_SET
AC_PROG_RANLIB

AC_ARG_WITH([DEFBIN],
            [AS_HELP_STRING([--with-defbin=<dirname_DEFBIN>],
                            [set directory to search "sort" before $PATH, default is /usr/5bin])],
            [DEFBIN="$withval"],[DEFBIN="/usr/5bin"])
if test -n "$DEFBIN"
then
  AC_DEFINE_UNQUOTED([DEFBIN],["$DEFBIN"],
                     [directory for "tar" to search "sort" command])
fi

AC_ARG_WITH([SV3BIN],
            [AS_HELP_STRING([--with-sv3bin=<dirname_SV3BIN>],
                            [directory for "tar" to search "sort" before DEFBIN, default is none])],
            [SV3BIN="$withval"],[SV3BIN=""])
if test -n "$SV3BIN"
then
  AC_DEFINE_UNQUOTED([SV3BIN],["$SV3BIN"],
                     [directory for tar to search "sort" command before DEFBIN])
fi

unset subdirs
HEIRLOOM_FIND_LIBCOMMON
AS_IF([ test "x${build_included_libcommon}" = xyes ],
      [ AC_CONFIG_SUBDIRS( heirloom-libcommon  ) ])

HEIRLOOM_FIND_LIBUXRE
AS_IF([ test "x${build_included_libuxre}" = xyes ],
      [ AC_CONFIG_SUBDIRS( heirloom-libuxre ) ])

AC_HEADER_MAJOR
AC_CHECK_HEADERS([sys/mtio.h sys/scsi.h sys/st01.h sys/sysmacros.h sys/mkdev.h])
save_CPPFLAGS="${CPPFLAGS}"
CPPFLAGS="${CPPFLAGS} -D_GNU_SOURCE"
have_gnu_extension=no

# open(2) flags
AC_CHECK_DECLS([O_DIRECT, O_DIRECTORY, O_NOATIME, O_NOFOLLOW, O_PATH, O_TMPFILE],
               [have_gnu_extensions=yes],[],[#include <fcntl.h>])

# math constants
AC_CHECK_DECLS([M_PI, M_E],
               [have_gnu_extensions=yes],[],[#include <math.h>])

# cpp macro functions
AC_CHECK_DECLS([strdupa, strndupa],
               [have_gnu_extensions=yes],[],[#include <string.h>])

# utility functions
AC_CHECK_FUNCS([getline getdelim mempcpy strcasestr qsort_r],
               [have_gnu_extensions=yes])
AC_CHECK_TYPES([struct mtget],[],[],[#include <sys/mtio.h>])
AC_CHECK_TYPES([struct floppy_struct],[],[],[#include <linux/fd.h>])

CPPFLAGS="${save_CPPFLAGS}"
if test "x${have_gnu_extensions}" = xyes
then
  AC_DEFINE([HAVE__GNU_SOURCE],[1],[-D_GNU_SOURCE enable some extensions])
fi

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([GNUmakefile])
AC_OUTPUT
