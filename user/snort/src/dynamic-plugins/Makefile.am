## $Id
AUTOMAKE_OPTIONS=foreign no-dependencies

noinst_LIBRARIES = libdynamic.a

#SUBDIRS = sf_engine

libdynamic_a_SOURCES = \
sf_dynamic_common.h \
sf_dynamic_detection.h \
sf_dynamic_engine.h \
sf_dynamic_meta.h \
sf_dynamic_plugins.c \
sf_dynamic_preprocessor.h \
sp_dynamic.c \
sp_dynamic.h \
sp_preprocopt.c \
sp_preprocopt.h

INCLUDES = @INCLUDES@

SUBDIRS = sf_engine sf_preproc_example
