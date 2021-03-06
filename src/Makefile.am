## Makefile.am
##
## Copyright (C) 2010,2018-2020 Free Software Foundation, Inc.
##
## This file is part of LibreDWG.
##
## LibreDWG is free software: you can redistribute it and/or modify it
## under the terms of the GNU General Public License as published by
## the Free Software Foundation, either version 3 of the License, or
## (at your option) any later version.
##
## LibreDWG is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty
## of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
## See the GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with this program.  If not, see <http://www.gnu.org/licenses/>.

lib_LTLIBRARIES = libredwg.la
WARN_CFLAGS = @WARN_CFLAGS@
AM_CFLAGS   = -I$(top_srcdir)/include -I. $(WARN_CFLAGS)
libredwg_la_LDFLAGS = -version-info $(LIBREDWG_SO_VERSION) -no-undefined -lm

libredwg_la_SOURCES = \
	dwg.c \
	common.c \
	bits.c \
	decode.c \
        decode_r2007.c \
	reedsolomon.c \
        print.c \
        free.c \
        hash.c \
	dynapi.c \
	dwg_api.c \
	$(EXTRA_HEADERS)
if !DISABLE_DXF
libredwg_la_SOURCES += \
        out_json.c \
        out_dxf.c \
        out_dxfb.c \
        out_geojson.c
endif
if USE_WRITE
libredwg_la_SOURCES += \
	encode.c
#       encode_r2007.c \
#       in_json.c
if !DISABLE_DXF
libredwg_la_SOURCES += \
	in_dxf.c
endif
endif

EXTRA_HEADERS = \
	dwg.spec \
        header_variables.spec \
        header_variables_r11.spec \
        header_variables_dxf.spec \
        common_entity_data.spec \
        common_entity_handle_data.spec \
        header.spec \
        auxheader.spec \
	r2004_file_header.spec \
        summaryinfo.spec \
        appinfo.spec \
        filedeplist.spec \
        security.spec \
        vbaproject.spec \
        template.spec \
        objfreespace.spec \
        revhistory.spec \
	classes.inc \
	objects.inc \
	spec.h \
	common.h \
	bits.h \
	myalloca.h \
	decode.h \
	dec_macros.h \
	free.h \
        print.h \
	logging.h \
	reedsolomon.h \
        hash.h \
	dynapi.h \
	out_json.h
if !DISABLE_DXF
EXTRA_HEADERS += \
	out_dxf.h
endif
if USE_WRITE
EXTRA_HEADERS += \
	encode.h
if !DISABLE_DXF
EXTRA_HEADERS += \
	in_dxf.h \
	in_json.h
endif
endif

EXTRA_DIST = \
	$(include_HEADERS) \
	$(EXTRA_HEADERS) \
	gen-dynapi.pl

$(srcdir)/dynapi.c: $(srcdir)/gen-dynapi.pl $(top_srcdir)/include/dwg.h
	echo make regen-dynapi might be needed

.c.i:
	$(AM_V_CC)depbase=`echo $@ | sed 's|[^/]*$$|$(DEPDIR)/&|;s|\.o$$||'`;\
	$(COMPILE) -c -E -o $@ $<

.c.ii: # no linemarkers
	$(AM_V_CC)depbase=`echo $@ | sed 's|[^/]*$$|$(DEPDIR)/&|;s|\.o$$||'`;\
	$(COMPILE) -c -E -P -o $@ $<

.c.ic: # replace the original .c with the expanded version.
	$(AM_V_CC)depbase=`echo $@ | sed 's|[^/]*$$|$(DEPDIR)/&|;s|\.o$$||'`;\
	$(COMPILE) -c -E -P -o $@ $<
	$(CLANG_FORMAT) -i $@
	$(am__mv) $< $<.orig
	$(am__mv) $@ $<

# emacs flymake-mode
check-syntax:
	test -n "$(CHK_SOURCES)" && \
	  nice $(COMPILE) -O0 -o /dev/null -S $(CHK_SOURCES)
.PHONY: check-syntax

if ENABLE_GCOV
AM_CFLAGS += @GCOV_CFLAGS@
LIBS      += @GCOV_LIBS@
gcov:
	-test -f $(builddir)/gcov.log && rm $(builddir)/gcov.log
	$(GCOV) *.gcno | tee -a $(builddir)/gcov.log
endif
MAINTAINERCLEANFILES = *~ *.i *_flymake.[ch]
if ENABLE_GCOV
CLEANFILES  = *.gcov *.gcno *.gcda gcov.log
endif
