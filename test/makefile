# Define KallistiOS environment variable
KOS_BASE=/opt/toolchains/dc/kos/
export KOS_BASE

# Use KallistiOS wrapper scripts for compilation
CC=kos-cc
CXX=kos-c++

# Include directories and flags
IDIR +=. ../source ../repl
CFLAGS +=$(addprefix -I,$(IDIR)) $(KOS_CFLAGS) -std=c11 -g -Wall -W -Wno-unused-parameter -Wno-unused-function -Wno-unused-variable
LIBS +=-lm

ODIR = obj
SOURCE_TARGETS = $(wildcard ../source/*.c)
REPL_TARGETS = $(filter-out ../repl/repl_main.c, $(wildcard ../repl/*.c))
SOURCE_OBJS = $(addprefix $(ODIR)/,$(notdir $(SOURCE_TARGETS:.c=.o)))
REPL_OBJS = $(addprefix $(ODIR)/,$(notdir $(REPL_TARGETS:.c=.o)))

TEST_PARSER_SRC = test_parser.c
TEST_PARSER_OBJ = $(ODIR)/test_parser.o
TEST_PARSER_ELF = ../$(TOY_OUTDIR)/test_parser.elf
ROMDISK_IMG = romdisk.img
ROMDISK_O = romdisk.o

.PHONY: all clean rm-elf

all: rm-elf $(TEST_PARSER_ELF)

$(TEST_PARSER_ELF): $(TEST_PARSER_OBJ) $(SOURCE_OBJS) $(REPL_OBJS) $(ROMDISK_O)
	@$(CC) -o $@ $(TEST_PARSER_OBJ) $(SOURCE_OBJS) $(REPL_OBJS) $(ROMDISK_O) $(CFLAGS) $(LIBS)

$(ODIR):
	@mkdir -p $(ODIR)

$(TEST_PARSER_OBJ): $(TEST_PARSER_SRC) | $(ODIR)
	@$(CC) -c -o $@ $< $(CFLAGS)

$(SOURCE_OBJS): $(ODIR)/%.o: ../source/%.c | $(ODIR)
	@$(CC) -c -o $@ $< $(CFLAGS)

$(REPL_OBJS): $(ODIR)/%.o: ../repl/%.c | $(ODIR)
	@$(CC) -c -o $@ $< $(CFLAGS)

romdisk.img:
	@$(KOS_GENROMFS) -f $(ROMDISK_IMG) -d romdisk -v

$(ROMDISK_O): romdisk.img
	@$(KOS_BASE)/utils/bin2o/bin2o $(ROMDISK_IMG) romdisk $(ROMDISK_O)

rm-elf:
	@$(RM) -f $(TEST_PARSER_ELF) $(ROMDISK_IMG) $(ROMDISK_O)

clean:
	@$(RM) -r $(ODIR)
