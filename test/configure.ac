dnl 
dnl   Copyright 2001-2013 Adrian Thurston <thurston@complang.org>
dnl
  
AC_INIT(ragel-suite, 1.0.1)
PUBDATE="November 2014"

EXPECTED_VER=0.13.0.4

AM_INIT_AUTOMAKE([foreign])
AC_SUBST(PUBDATE)
AC_CONFIG_HEADER(config.h)

: ${CFLAGS="-Wall -g"}
: ${CXXFLAGS="-Wall -g"}

dnl Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_RANLIB

dnl Set test on c++ compiler.
AC_LANG_CPLUSPLUS

dnl Check for definition of MAKE.
AC_PROG_MAKE_SET

dnl
dnl Which ragel are we testing. Default is the one in parent's source dir.
dnl
AC_ARG_WITH(subject,
	[AC_HELP_STRING([--with-subject], [location of ragel to test])],
	[
		SUBJECT_BIN="$withval/bin/ragel"
		SUBJECT_CPPFLAGS="-I$withval/include"
		SUBJECT_LDFLAGS="-L$withval/lib"
		SUBJECT_LD_LIBRARY_PATH="$withval/lib"
	],
	[
		SUBJECT_BIN="\${abs_top_builddir}/../src/ragel"
		SUBJECT_CPPFLAGS="-I\${abs_top_builddir}/../src -I\${abs_top_builddir}/../aapl"
		SUBJECT_LDFLAGS="-L\${abs_top_builddir}/../src/.libs"
		SUBJECT_LD_LIBRARY_PATH="\${abs_top_builddir}/../src/.libs"
	]
)

AC_SUBST(SUBJECT_BIN)
AC_SUBST(SUBJECT_CPPFLAGS)
AC_SUBST(SUBJECT_LDFLAGS)
AC_SUBST(SUBJECT_LD_LIBRARY_PATH)

AC_ARG_WITH(ragel-src,
	[AC_HELP_STRING([--with-ragel-src], [location of ragel src code])],
		[RAGEL_SRC="$withval"],
		[RAGEL_SRC="\${abs_top_builddir}/../src"] )

AC_SUBST(RAGEL_SRC)

dnl Check for colm.
if test -z "$EXPECTED_VER"; then
	AC_ERROR( [check colm: expected version not passed in] )
fi

AC_ARG_WITH(deps,
	[AC_HELP_STRING([--with-deps], [generic dependency location])],
	[DEPS="$withval"],
	[DEPS="/opt/colm"]
)

AC_ARG_WITH(colm,
	[AC_HELP_STRING([--with-colm], [location of colm install])],
	[
		CPPFLAGS="${CPPFLAGS} -I$withval/include"
		LDFLAGS="${LDFLAGS} -L$withval/lib"
		LD_LIBRARY_PATH="$withval/lib"
		COLM="$withval/bin/colm"
	],
	[
		CPPFLAGS="${CPPFLAGS} -I$DEPS/include"
		LDFLAGS="${LDFLAGS} -L$DEPS/lib"
		LD_LIBRARY_PATH="$DEPS/lib"
		COLM="$DEPS/bin/colm"
	]
)

AC_CHECK_FILES(
	[$COLM],
	[],
	[AC_ERROR([colm is required to build this package])]
)
AC_SUBST(COLM)
AC_SUBST(LD_LIBRARY_PATH)

INSTALLED_VER=`$COLM -v | sed -n -e '1 { s/^.*version //; s/ .*$//; p; }'`
if test "x$INSTALLED_VER" != "x$EXPECTED_VER"; then
	AC_ERROR( [check colm: expected version $EXPECTED_VER, but $INSTALLED_VER is installed] )
fi

SED_SUBST=["\
	-e 's|@SUBJECT_BIN@|${SUBJECT_BIN}|g' \
	-e 's|@SUBJECT_CPPFLAGS@|${SUBJECT_CPPFLAGS}|g' \
	-e 's|@SUBJECT_LDFLAGS@|${SUBJECT_LDFLAGS}|g' \
	-e 's|@SUBJECT_LD_LIBRARY_PATH@|${SUBJECT_LD_LIBRARY_PATH}|g' \
	-e 's|@LD_LIBRARY_PATH@|${LD_LIBRARY_PATH}|g' \
	-e 's|@CXX@|${CXX}|g' \
	-e 's|@CC@|${CC}|g' \
"]

AC_SUBST(SED_SUBST)

dnl write output files
AC_OUTPUT([
		Makefile
])

echo "configuration of ragel test package complete"
