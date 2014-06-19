# $FreeBSD$

PROG=		fetch
CSTD?=		c99
LDFLAGS=	-lssl -lcrypto
CFLAGS+=	-I../libfetch

build: fetch.o
	$(CC) $(LDFLAGS) $(CFLAGS) -o $(PROG) fetch.o ../libfetch/libfetch.a
