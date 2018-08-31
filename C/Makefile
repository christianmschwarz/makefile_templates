PROJECT := PROJECT_NAME_HERE
SRCDIR := src
BINDIR := bin
OBJDIR := $(BINDIR)/obj
DEPSFILE := $(BINDIR)/dependencies
EXECFILE := $(BINDIR)/$(PROJECT)

SHELL := /bin/bash
ROOT := $(shell pwd)
SRCS := $(shell find $(SRCDIR) -name "*.c")
OBJS := $(patsubst $(SRCDIR)/%.c, $(OBJDIR)/%.o, $(SRCS))

all: $(PROJECT)
$(PROJECT): depend $(OBJS)
	@cd $(BINDIR)
	@gcc -o $(EXECFILE) $(OBJS)

run: $(PROJECT)
	@$(EXECFILE)

depend: $(DEPSFILE)
$(DEPSFILE):
	@mkdir $(BINDIR)
	@mkdir $(OBJDIR)
	@find $(SRCDIR) -name "*.c" | sed -e 's|^$(SRCDIR)\(.*\)\.c|\1\t\1|' \
		-e 's|\(.*\)\t\(.*\)\/\(.*\)|\
		gcc -MT $(OBJDIR)\2/\3.o -MM $(SRCDIR)\1.c \
		printf "\\tmkdir -p $(OBJDIR)\2 \&\& cd $(OBJDIR)\2 \&\& "\
		printf "gcc -c $(ROOT)/$(SRCDIR)\1.c\\n"|'\
		|sh > $(DEPSFILE)

clean:
	rm -rf $(BINDIR)

ifeq (,$(findstring clean,$(MAKECMDGOALS)))
include $(DEPSFILE)
endif
