## Process this file with automake to produce Makefile.in
bin_PROGRAMS = dc

dc_SOURCES = dc.c misc.c eval.c stack.c array.c numeric.c string.c
noinst_HEADERS = dc.h dc-proto.h dc-regdef.h

AM_CPPFLAGS = -I$(srcdir)/.. -I$(srcdir)/../h
LDADD = ../lib/libbc.a

MAINTAINERCLEANFILES = Makefile.in

AM_CFLAGS = @CFLAGS@ -Wall -funsigned-char

$(PROGRAMS): $(LDADD)
