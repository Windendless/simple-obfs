VERSION_INFO = 2:0:0

AM_CFLAGS = -g -O2 -Wall -Werror -Wno-deprecated-declarations -fno-strict-aliasing -std=gnu99 -D_GNU_SOURCE
AM_CFLAGS += $(PTHREAD_CFLAGS)
if !USE_SYSTEM_SHARED_LIB
AM_CFLAGS += -I$(top_srcdir)/libev
AM_CFLAGS += -I$(top_srcdir)/libudns
endif
AM_CFLAGS += -I$(top_srcdir)/libcork/include
AM_CFLAGS += $(LIBPCRE_CFLAGS)

OBFS_COMMON_LIBS = $(top_builddir)/libcork/libcork.la \
                   $(INET_NTOP_LIB) $(LIBPCRE_LIBS)

if !USE_SYSTEM_SHARED_LIB
OBFS_COMMON_LIBS += $(top_builddir)/libev/libev.la
else
OBFS_COMMON_LIBS += -lev -lm
endif

bin_PROGRAMS = obfs-local
if !BUILD_WINCOMPAT
bin_PROGRAMS += obfs-server
endif

obfs_src = obfs_http.c \
		   obfs_tls.c \
		   options.c \
		   base64.c

obfs_local_SOURCES = utils.c \
                   jconf.c \
                   json.c \
                   encrypt.c \
                   netutils.c \
                   local.c \
				   $(obfs_src)

obfs_server_SOURCES = utils.c \
                    netutils.c \
                    jconf.c \
                    json.c \
                    encrypt.c \
                    resolv.c \
                    server.c \
				    $(obfs_src)

obfs_local_LDADD = $(OBFS_COMMON_LIBS)
obfs_server_LDADD = $(OBFS_COMMON_LIBS)
if USE_SYSTEM_SHARED_LIB
obfs_local_LDADD += -ludns
obfs_server_LDADD += -ludns
else
obfs_local_LDADD += $(top_builddir)/libudns/libudns.la
obfs_server_LDADD += $(top_builddir)/libudns/libudns.la
endif

obfs_local_CFLAGS = $(AM_CFLAGS) -DMODULE_LOCAL
obfs_server_CFLAGS = $(AM_CFLAGS) -DMODULE_REMOTE

if BUILD_WINCOMPAT
obfs_local_SOURCES += win32.c
endif
