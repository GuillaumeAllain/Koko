#
# Simple Makefile for building the linenoise library as
# part of Koko
#

ifndef CFLAGS
  ifdef DEBUG
    CFLAGS = -g -Wc++11-extensions
  else
    CFLAGS = -O2 -Wc++11-extensions
  endif
endif

# source files
CPPSRC = wcwidth.cpp ConvertUTF.cpp linenoise.cpp
CSRC   = linenoise_f90.c

# object files
OBJ = $(CPPSRC:.cpp=.o) $(CSRC:.c=.o)

# target
all: liblinenoise

liblinenoise: $(OBJ)
	ar rs liblinenoise.a $(OBJ)
	mv liblinenoise.a ../lib

# rules
%.o: %.cpp
	$(CC) -c $(CFLAGS) $<

%.o: %.c
	$(CC) -c $(CFLAGS) $<

clean:
	rm -f *.o *~
