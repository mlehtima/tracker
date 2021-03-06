AM_VALAFLAGS =                                         \
	--includedir=libtracker-data                   \
	--header tracker-sparql-query.h                \
	--vapi tracker-sparql-query.vapi               \
	--pkg gio-2.0                                  \
	--pkg posix                                    \
	$(BUILD_VALAFLAGS)                             \
	$(top_srcdir)/src/libtracker-common/libtracker-common.vapi \
	$(top_srcdir)/src/libtracker-sparql/tracker-sparql-$(TRACKER_API_VERSION).vapi \
	libtracker-data.vapi

AM_CPPFLAGS =                                          \
	$(BUILD_VALACFLAGS)                            \
	-I$(top_srcdir)/src                            \
	-I$(top_builddir)/src                          \
	-DSHAREDIR=\""$(datadir)"\"                    \
	$(LIBTRACKER_DATA_CFLAGS)

libtracker_datadir = $(libdir)/tracker-$(TRACKER_API_VERSION)
libtracker_data_LTLIBRARIES = libtracker-data.la

libtracker_data_la_SOURCES =                           \
	tracker-sparql-expression.vala                 \
	tracker-sparql-pattern.vala                    \
	tracker-sparql-query.vala                      \
	tracker-sparql-scanner.vala                    \
	tracker-turtle-reader.vala                     \
	tracker-class.c                                \
	tracker-collation.c                            \
	tracker-crc32.c \
	tracker-data-backup.c                          \
	tracker-data-manager.c                         \
	tracker-data-query.c                           \
	tracker-data-update.c                          \
	tracker-db-config.c                            \
	tracker-db-interface.c                         \
	tracker-db-interface-sqlite.c                  \
	tracker-db-manager.c                           \
	tracker-db-journal.c                           \
	tracker-db-backup.c                            \
	tracker-namespace.c                            \
	tracker-ontology.c                             \
	tracker-ontologies.c                           \
	tracker-property.c

libtracker_data_la_LIBADD =                            \
	$(top_builddir)/src/gvdb/libgvdb.la \
	$(top_builddir)/src/libtracker-common/libtracker-common.la \
	$(BUILD_LIBS)                                  \
	$(LIBTRACKER_DATA_LIBS)                        \
	-lm

if HAVE_TRACKER_FTS
libtracker_data_la_LIBADD +=                           \
	$(top_builddir)/src/libtracker-fts/libtracker-fts.la
endif

noinst_HEADERS =                                       \
	tracker-class.h                                \
	tracker-crc32.h \
	tracker-data.h                                 \
	tracker-collation.h                            \
	tracker-data-backup.h                          \
	tracker-data-manager.h                         \
	tracker-data-query.h                           \
	tracker-data-update.h                          \
	tracker-db-config.h                            \
	tracker-db-interface.h                         \
	tracker-db-interface-sqlite.h                  \
	tracker-db-manager.h                           \
	tracker-db-journal.h                           \
	tracker-db-backup.h                            \
	tracker-namespace.h                            \
	tracker-ontology.h                             \
	tracker-ontologies.h                           \
	tracker-property.h                             \
	tracker-sparql-query.h

# Configuration / GSettings
gsettings_ENUM_NAMESPACE = org.freedesktop.Tracker
gsettings_ENUM_FILES = $(top_srcdir)/src/libtracker-common/tracker-enums.h

gsettings_SCHEMAS = org.freedesktop.Tracker.DB.gschema.xml

@INTLTOOL_XML_NOMERGE_RULE@

@GSETTINGS_RULES@

if HAVE_XSLTPROC
DATE=`date +'%B %Y'`
man_MANS = tracker-db.cfg.5
else
man_MANS =
endif

%.cfg.5: $(gsettings_SCHEMAS)
	$(AM_V_GEN)$(XSLTPROC) \
	  --stringparam version "${VERSION}" \
	  --stringparam current-date "${DATE}" \
	  --stringparam config-file "$(@:.cfg.5=.cfg)" \
	  $(top_srcdir)/docs/manpages/gsettings.xsl $^ > $@

# Dist / Clean rules for all data
BUILT_SOURCES = \
	libtracker_data_la_vala.stamp

CLEANFILES = \
	libtracker_data_la_vala.stamp \
	$(gsettings_SCHEMAS) \
	$(man_MANS)

EXTRA_DIST = \
	libtracker-data.vapi \
	$(gsettings_SCHEMAS:.xml=.xml.in) \
	$(man_MANS)
