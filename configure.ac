dnl Process this file with autoconf to produce a configure script.
AC_INIT([bc],[1.06])
AC_CONFIG_SRCDIR([doc/bc.1])
AM_INIT_AUTOMAKE
AM_CONFIG_HEADER(config.h)

AC_DEFINE(DC_VERSION,"1.3")
AC_DEFINE_UNQUOTED(BC_COPYRIGHT, dnl
 ["Copyright 1991-1994, 1997, 1998, 2000 Free Software Foundation, Inc."])
AC_DEFINE_UNQUOTED(DC_COPYRIGHT, dnl
 ["Copyright 1994, 1997, 1998, 2000 Free Software Foundation, Inc."])

AC_PROG_CC
AC_USE_SYSTEM_EXTENSIONS
dnl AC_SEARCH_LIBS([strerror],[cposix])

AM_PROG_LEX
AC_PROG_YACC
AC_PROG_INSTALL
AC_PROG_RANLIB
AC_PROG_MAKE_SET

AC_CHECK_HEADERS(stdarg.h stddef.h stdlib.h string.h limits.h unistd.h lib.h)
AC_C_CONST
AC_TYPE_SIZE_T
AC_CHECK_TYPE(ptrdiff_t, size_t)

AC_FUNC_VPRINTF
AC_CHECK_FUNCS(isgraph setvbuf)

AC_ARG_WITH(pkg,[  --with-pkg              use software installed in /usr/pkg tree], [
   CPPFLAGS="$CPPFLAGS -I/usr/pkg/include"
   LDFLAGS="$LDFLAGS -L/usr/pkg/lib"
   echo Using /usr/pkg/include and /usr/pkg/lib
])

bcle=n
AC_ARG_WITH(libedit,[  --with-libedit          support fancy BSD command input 
editing], [
   AC_CHECK_LIB(termcap,tgetent,TERMLIB=-ltermcap)
   LDSAVE=$LDFLAGS
   LDFLAGS="$LDFLAGS $TERMLIB"
   AC_CHECK_LIB(edit,el_gets,
      [AC_CHECK_HEADER(histedit.h,
      READLINELIB="-ledit $TERMLIB";bcle=y)],
      READLINELIB="")
   if test "$bcle" = "y"; then
     echo Using the libedit library.
     AC_DEFINE(LIBEDIT,1)
   fi
   LDFLAGS=$LDSAVE
])
 
bcrl=n
AC_ARG_WITH(readline,[  --with-readline         support fancy command input editing], [
   AC_CHECK_LIB(ncurses,tparm,TERMLIB=-lncurses,
     AC_CHECK_LIB(termcap,tgetent,TERMLIB=-ltermcap))
   LDSAVE=$LDFLAGS
   LDFLAGS="$LDFLAGS $TERMLIB"
   AC_CHECK_LIB(readline,readline,
      [AC_CHECK_HEADER(readline/readline.h,
      READLINELIB="-lreadline $TERMLIB";bcrl=y)],
      READLINELIB="")
   if test "$bcrl" = "y" ; then
     echo Using the readline library.
     AC_DEFINE(READLINE,1)
   fi
   LDFLAGS=$LDSAVE
])

if test "$LEX" = "flex" ; then
 LEX="flex -I8"
else
 if test "$bcrl" = "y" ; then
  AC_MSG_WARN(readline works only with flex.)
 fi
fi

if test "$bcrl" = "y" -a "$bcle" = "y" ; then
  AC_MSG_ERROR(Can not use both readline and libedit.  Aborting.)
fi

if test "$LEX" = "lex" -a `uname -s` = "SunOS" ; then
  LEXLIB=""
  echo "SunOS using lex does not have a -ll."
fi

AC_SUBST(READLINELIB)
AC_ARG_PROGRAM
AC_CONFIG_FILES([Makefile bc/Makefile dc/Makefile doc/Makefile lib/Makefile])
AC_OUTPUT
