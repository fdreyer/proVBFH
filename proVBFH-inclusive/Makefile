######################################################################
# Makefile for proVBFH-inclusive 
#
#  - "make" creates the vbfh executable
#  - "make clean" removes all files created by the "make" command
######################################################################

# include the settings from Makefile.inc, generated by configure script
include Makefile.inc

# main program and modules to be compiled
MAIN = provbfh_incl provbfhh_incl
MODULES = integration phase_space parameters io_utils lcl_dec matrix_element \
          matrix_element_dihiggs phase_space_dihiggs tensor ME_expressions

# librairies and flags needed for compilation
#FF = ifort
SRC=$(PWD)/src
OBJ=$(PWD)/obj
VPATH=./:$(SRC):/$(OBJ)

FFLAGS= -O2 -ffixed-line-length-132
FFLAGS+= $(shell $(HPEXEC) --fflags)
INCLUDE= -I$(SRC) $(wildcard *.h)
FFLAGS+= $(INCLUDE) -J$(OBJ)
LDFLAGS= $(shell $(HPEXEC) --libs) $(shell $(LHEXEC) --libs)

all: provbfh_incl

# main program
$(MAIN): %: %.o $(addsuffix .o,$(MODULES)) Makefile
	$(FF) $(OBJ)/$@.o $(patsubst %,$(OBJ)/%,$(addsuffix .o,$(MODULES))) \
	$(FFLAGS) $(LDFLAGS) -o $@

# object files
%.o: %.f Makefile 
	$(FF) $(FFLAGS) -o $(OBJ)/$@ $< -c

%.o: %.f90 Makefile
	$(FF) $(FFLAGS) -o $(OBJ)/$@ $< -c

# f90 module dependencies
phase_space.o: matrix_element.o parameters.o
phase_space_dihiggs.o: matrix_element_dihiggs.o parameters.o
provbfh_incl.o: matrix_element.o parameters.o phase_space.o integration.o
provbfhh_incl.o: matrix_element_dihiggs.o parameters.o phase_space_dihiggs.o integration.o
parameters.o: io_utils.o lcl_dec.o integration.o
matrix_element.o: parameters.o
matrix_element_dihiggs.o: parameters.o tensor.o ME_expressions.o

# make clean
clean:
	rm -f $(OBJ)/*.o $(OBJ)/*.mod *~ *.log fort* $(MAIN)

