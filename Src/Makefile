#
# Basic Makefile for compiling Koko ODS with gfortran
#
# Ulf GRIESMANN, April 2020
#

SUBDIRS = kmath ldm config hardware macro optimize paraxial pickup \
          raytrace specsurf nonseq utility wavespot glass plotcad

# default platform is Linux
#ifndef OS
OS = MACOS
#endif

# default installation directory prefix
ifndef $(PREFIX)
  PREFIX=/usr/local
endif

# compiler flags
CC = g++         # for now, only support GCC
FC = gfortran
ifdef DEBUG
  FFLAGS = -g -cpp -Wall -ffixed-line-length-none -fno-align-commons -D$(OS)
  CFLAGS = -Wall -g -Wc++11-extensions -Wc++11-compat-deprecated-writable-strings
else
  ifdef NATIVE
    FFLAGS = -g -cpp -O2 -ffixed-line-length-none -march=native -mtune=native -fno-align-commons -D$(OS)
  else
    FFLAGS = -g -cpp -O2 -ffixed-line-length-none -fno-align-commons -D$(OS)
  endif  
  CFLAGS = -g -O2 -Wc+11-extensions -Wc++11-compat-deprecated-writable-strings
endif
IFLAGS = -I../include -I../modules

# needed in sub-make processes
export CC FC IFLAGS CFLAGS FFLAGS

LDIR = -L./lib
LIBS = -W $(addprefix -l,$(SUBDIRS)) -llinenoise -lkmod -W -lm -lstdc++ #-lregex

fSRC = koko.f cmder.f names.f libload.f saverest.f

FOBJ = $(fSRC:.f=.o) 

# targets
all: buildinfo libdir modules libraries koko-cli

.PHONY: modules
modules:
	make -C modules

.PHONY: libraries $(SUBDIRS)
libraries: $(SUBDIRS)
$(SUBDIRS):
	make -C $@
	make -C linenoise

koko-cli: buildinfo.inc $(FOBJ)
	$(FC) -o koko-cli $(FOBJ) $(LDIR) $(LIBS)

.PHONY: buildinfo
buildinfo:
	./buildinfo

%.o: %.f
	$(FC) -c $(FFLAGS) -I./include -I./modules $<

libdir:
	mkdir -p ./lib

clean:
	rm -f *.o *~ koko-cli buildinfo.inc
	rm -rf ./lib
	make -C modules clean
	make -C linenoise clean
	for d in $(SUBDIRS) ; do make -C $$d clean ; done

strip:
	strip -S ./koko-cli

install:
	mkdir -p $(PREFIX)/bin
	install -m 755 koko-cli $(PREFIX)/bin
