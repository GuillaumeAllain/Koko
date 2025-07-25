#
# Makefile for Fortran modules
#
# Ulf GRIESMANN, April 2020
#

IFLAGS = -I../include

# module sources
fSRC   = globals.f nssmod.f
f90SRC = configfile.f90 kokoconfig.f90 getoptions.f90 commandline.f90 strings.f90 \
         rgb.f90 posix_regex.f90
F90SRC = opsys.F90 treap.F90 dictionary.F90
CSRC   = rgb_f90.c

MODS = $(fSRC:.f=.mod) $(f90SRC:.f90=.mod) $(F90SRC:.F90=.mod)
FOBJ = $(fSRC:.f=.o) $(f90SRC:.f90=.o) $(F90SRC:.F90=.o)
COBJ = $(CSRC:.c=.o)

# targets
all: regex_f90.inc libkmod

libkmod: $(MODS) $(FOBJ) $(COBJ)
	ar rs libkmod.a $(FOBJ) $(COBJ)
	mv libkmod.a ../lib

%.mod: %.f
	$(FC) -c $(FFLAGS) $(IFLAGS) $<

%.mod: %.f90
	$(FC) -c $(FFLAGS) $(IFLAGS) $<

%.mod: %.F90
	$(FC) -c $(FFLAGS) $(IFLAGS) $<

%.o : %.c
	$(CC) -c $(CFLAGS) $<

regex_f90.inc:
	$(CC) -o regex_t_size regex_t_size.c
	./regex_t_size > regex_f90.inc

clean:
	rm -f *.mod *.o *~ regex_f90.inc regex_t_size
