LIB=		fetch
CFLAGS+=	-I.
SRCS=		fetch.c common.c ftp.c http.c file.c \
		ftperr.h httperr.h
INCS=		fetch.h
MAN=		fetch.3
CLEANFILES=	ftperr.h httperr.h

CFLAGS+=	-DWITH_SSL -fPIC
LDADD=		-lssl -lcrypto

CFLAGS+=	-DFTP_COMBINE_CWDS

CSTD?=		c99

OBJS = fetch.o common.o ftp.o http.o file.o

LIBTARGET = libfetch.a
DYLIBTARGET = libfetch.so
CC ?=cc

ftperr.h: ftp.errors
	@echo "static struct fetcherr ftp_errlist[] = {" > ${.TARGET}
	@cat ${.CURDIR}/ftp.errors \
	  | grep -v ^# \
	  | sort \
	  | while read NUM CAT STRING; do \
	    echo "    { $${NUM}, FETCH_$${CAT}, \"$${STRING}\" },"; \
	  done >> ${.TARGET}
	@echo "    { -1, FETCH_UNKNOWN, \"Unknown FTP error\" }" >> ${.TARGET}
	@echo "};" >> ${.TARGET}

httperr.h: http.errors
	@echo "static struct fetcherr http_errlist[] = {" > ${.TARGET}
	@cat ${.CURDIR}/http.errors \
	  | grep -v ^# \
	  | sort \
	  | while read NUM CAT STRING; do \
	    echo "    { $${NUM}, FETCH_$${CAT}, \"$${STRING}\" },"; \
	  done >> ${.TARGET}
	@echo "    { -1, FETCH_UNKNOWN, \"Unknown HTTP error\" }" >> ${.TARGET}
	@echo "};" >> ${.TARGET}

build: $(LIBTARGET) $(DYLIBTARGET)

$(DYLIBTARGET) : $(CLEANFILES) $(OBJS)
	 $(CC) $(FLAGS) $(CFLAGS) -shared -o $(DYLIBTARGET) $(OBJS)

$(LIBTARGET) : $(CLEANFILES) $(OBJS)
	ar r $(LIBTARGET) $(OBJS)
	ranlib $(LIBTARGET)

clean :
	rm -f $(OBJS) $(LIBTARGET) $(DYLIBTARGET)
