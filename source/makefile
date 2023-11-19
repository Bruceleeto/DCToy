# Define KallistiOS environment variable
KOS_BASE=/opt/toolchains/dc/kos/
export KOS_BASE

# Use KallistiOS wrapper scripts for compilation
CC=kos-cc
CXX=kos-c++

# Include directories and flags
IDIR+=.
CFLAGS+=$(addprefix -I,$(IDIR)) $(KOS_CFLAGS) -std=c11 -g -Wall -W -Wno-unused-parameter -Wno-unused-function -Wno-unused-variable
LIBS+=



# Object and source files
ODIR = obj
SRC = $(wildcard *.c)
OBJ = $(addprefix $(ODIR)/,$(SRC:.c=.o))

# Output filename
OUTNAME=toy
OUT=../$(TOY_OUTDIR)/$(OUTNAME).elf

# Build targets
library: $(OBJ)
	$(CC) -DTOY_EXPORT $(CFLAGS) -shared -o $(OUT) $(OBJ) $(LIBS)

static: $(OBJ)
	ar crs ../$(TOY_OUTDIR)/lib$(OUTNAME).a $(OBJ)

library-release: $(OBJ) library
	kos-strip $(OUT)

static-release: $(OBJ) static
	kos-strip -d ../$(TOY_OUTDIR)/lib$(OUTNAME).a

# Create object directory
$(OBJ): | $(ODIR)

$(ODIR):
	mkdir $(ODIR)

# Compile objects
$(ODIR)/%.o: %.c
	$(CC) -c -o $@ $< $(CFLAGS)

# Clean up
.PHONY: clean

clean:
	$(RM) -r $(ODIR) ../$(TOY_OUTDIR)/$(OUTNAME).elf ../$(TOY_OUTDIR)/lib$(OUTNAME).a
