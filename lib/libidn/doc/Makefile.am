## Process this file with automake to produce Makefile.in
# Copyright (C) 2002, 2003, 2004 Simon Josefsson.
#
# This file is part of GNU Libidn.
#
# GNU Libidn is free software; you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as
# published by the Free Software Foundation; either version 2.1 of
# the License, or (at your option) any later version.
#
# GNU Libidn is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with GNU Libidn; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA

SUBDIRS = specifications tld java
if ENABLE_GTK_DOC
SUBDIRS += reference
endif

EXTRA_DIST = gdoc libidn.html libidn.ps libidn.pdf lgpl.texi texinfo.css \
	components.fig

info_TEXINFOS = libidn.texi
libidn_TEXINFOS = fdl.texi $(gdoc_TEXINFOS) \
	components.eps components.png components.pdf

components.eps: components.fig
	fig2dev -L eps components.fig components.eps

components.png: components.fig
	fig2dev -L png components.fig components.png

components.pdf: components.fig
	fig2dev -L pdf components.fig components.pdf

AM_MAKEINFOHTMLFLAGS = --no-split --number-sections --css-include=texinfo.css

# Man pages.

dist_man_MANS = idn.1 $(gdoc_MANS)
MAINTAINERCLEANFILES = $(dist_man_MANS)

idn.1: $(top_srcdir)/src/idn.c $(top_srcdir)/src/idn.ggo \
		$(top_srcdir)/configure.ac
	$(HELP2MAN) \
		--name="Internationalized Domain Names command line tool" \
		--output=$@ $(top_builddir)/src/idn

# GDOC

GDOC_SRC = $(top_srcdir)/lib/idna.c $(top_srcdir)/lib/nfkc.c	\
	$(top_srcdir)/lib/pr29.c $(top_srcdir)/lib/punycode.c	\
	$(top_srcdir)/lib/stringprep.c $(top_srcdir)/lib/tld.c	\
	$(top_srcdir)/lib/toutf8.c $(top_srcdir)/lib/version.c
GDOC_TEXI_PREFIX = texi/
GDOC_MAN_PREFIX = man/
GDOC_MAN_EXTRA_ARGS = -module $(PACKAGE) -sourceversion $(VERSION) \
	-bugsto $(PACKAGE_BUGREPORT) -includefuncprefix -seeinfo $(PACKAGE) \
	-copyright "2002, 2003, 2004 Simon Josefsson" -verbatimcopying
include $(srcdir)/Makefile.gdoci
