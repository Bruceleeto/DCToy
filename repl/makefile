# Define KallistiOS environment variable
KOS_BASE=/opt/toolchains/dc/kos/
export KOS_BASE

# Use KallistiOS wrapper scripts for compilation
CC=kos-cc
CXX=kos-c++

# Include directories and flags
IDIR+=. ../source
CFLAGS+=$(addprefix -I,$(IDIR)) $(KOS_CFLAGS) -std=c11 -g -Wall -W -Wno-unused-parameter -Wno-unused-function -Wno-unused-variable
LIBS+=-ltoy -lm

ODIR = obj
SRC = $(wildcard *.c)
OBJ = $(addprefix $(ODIR)/,$(SRC:.c=.o))
OUTNAME=toy
OUT=../$(TOY_OUTDIR)/toyrepl.elf

all: $(OBJ)
	$(CC) -DTOY_IMPORT $(CFLAGS) -o $(OUT) $(OBJ) -L$(realpath $(shell pwd)/../$(TOY_OUTDIR)) $(LIBS)

release: all
	kos-strip $(OUT)

$(OBJ): | $(ODIR)

$(ODIR):
	mkdir $(ODIR)

$(ODIR)/%.o: %.c
	$(CC) -c -o $@ $< $(CFLAGS)

.PHONY: clean

clean:
	$(RM) $(ODIR)
	$(RM) ../$(TOY_OUTDIR)/lib$(OUTNAME).a
	$(RM) $(OUT)
