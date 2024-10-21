# gnu makefile for nasdaq/devel 
# builds library for nasdaq and makes executable readit
########################################################
# see http://www.gnu.org/software/make/manual/
# Here is a summary of the automatic variables.
# $@    The file name of the target.
# $%    The target member name, when the target is an archive member.
# $<    The name of the first prerequisite.
# $?    The names of all the prerequisites that are newer than the target
# $^    The names of all the prerequisites, omits duplicates
# $+                                      , retains duplicates
# $* The stem with which an implicit rule matches
########################################################
ARCH          = $(shell root-config --arch)
LOCAL        := $(shell pwd)
ROOTCFLAGS   := $(shell root-config --cflags)
ROOTLIBS     := $(shell root-config --libs)
ROOTGLIBS    := $(shell root-config --glibs)
BOBJ=$(HOME)/bacon2Data/bobj
CAENDINC=$(LOCAL)/../CAENDigitizer-v2.17.3/
CAENVINC=$(LOCAL)/../CAENVMELib-v4.0.2/
CAENCINC=$(LOCAL)/../CAENComm-v1.7.0/
LOCALINC=$(LOCAL)/../include/
SEARCH_DIR=$(ROOTLIBS) -L$(BOBJ) 
FFTW=/snap/root-framework/931/usr/lib/x86_64-linux-gnu/


EXTRAFLAGS =  -DLINUX=1 -DSTDC_HEADERS=1 -DHAVE_SYS_TYPES_H=1 -DHAVE_SYS_STAT_H=1 -DHAVE_STDLIB_H=1 -DHAVE_STRING_H=1 -DHAVE_MEMORY_H=1 -DHAVE_STRINGS_H=1 -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DHAVE_UNISTD_H=1 -DSTDC_HEADERS=1 -DHAVE_MALLOC_H=1 -DHAVE_STDINT_H=1 -DHAVE_STDLIB_H=1 -DHAVE_STRING_H=1 -DHAVE_SYS_TIME_H=1 -DHAVE_SYS_TIMEB_H=1 -DHAVE_TERMIOS_H=1 -DHAVE_UNISTD_H=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_STDLIB_H=1 -DHAVE_MALLOC=1 -DHAVE_SYS_SELECT_H=1 -DHAVE_SYS_SOCKET_H=1 -DSELECT_TYPE_ARG1=int -DSELECT_TYPE_ARG234=\(fd_set\ \*\) -DSELECT_TYPE_ARG5=\(struct\ timeval\ \*\) -DHAVE_ATEXIT=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_MEMSET=1 -DHAVE_SELECT=1 -DHAVE_STRSTR=1 
CXXFLAGS  += $(ROOTCFLAGS) -fpermissive  -fPIC -I$(LOCALINC) -I/usr/include -I/usr/local/include -I$(BOBJ) -I$(CAENDINC)/include -I$(CAENCINC)/include -I$(CAENVINC)/include


ifeq ($(ARCH),macosx64)
				LIBS := $(ROOTLIBS) -L$(BOBJ) -lBaconAna  $(ROOTLIBS) -L$(FFTW)/lib -lfftw3 $(CINTFLAGS)
				CXX := clang++ -arch x86_64 $(CXXFLAGS)
				LD :=  clang++ -arch x86_64

else ifeq ($(ARCH),linuxx8664gcc)
				LIBS := $(ROOTLIBS) -L$(BOBJ) -lBaconAna  $(ROOTLIBS) -L$(FFTW) -lfftw3 $(CINTFLAGS) -L/usr/lib -lCAENVME -lCAENComm -l CAENDigitizer
				CXX := g++ -march=x86-64 
				LD := g++ -march=x86-64 
else 
				LIBS := $(ROOTLIBS) -L$(BOBJ) -lBaconAna  $(ROOTLIBS)  $(CINTFLAGS)
				LIBLINK :=
endif 


#CPP+=-D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64
OBJS := X742CorrectionRoutines.o flash.o fft.o keyb.o spi.o WDconfig.o WDplot.o

$(filter-out $(SKIP_OBJ),$(patsubst %.h,%.o,$(wildcard *.h)))

all: WaveDump WaveComp

WaveDump: $(OBJS) 
	@echo "compiling ... $(CXX) $(CXXFLAGS) "
	$(CXX) $(CXXFLAGS) WaveDump.c  -o WaveDump  $(OBJS) $(LIBS)
	@echo "$@ done"

WaveComp: $(OBJS) WaveComp.c  
	@echo "compiling ... $(CXX) $(CXXFLAGS) "
	$(CXX) $(CXXFLAGS) WaveComp.c  -o WaveComp  $(OBJS) $(LIBS)
	@echo "$@ done"


%.o: %.c 
	@echo "compiling ... $(CXX) $(CXXFLAGS) "
	$(CXX) $(CXXFLAGS) -c $<
	@echo "$@ done"

.PHONY : clean show

clean:
	rm -f *.o 
	rm -f ana
	rm -rf ana.dSYM
	rm -f ana1
	rm -rf ana1.dSYM
	rm -f sis
	rm -rf sis.dSYM
	rm -f build
	rm -rf build.dSYM
	rm -f summary
	rm -rf summary.dSYM
show:
	@echo ROOTSYS =$(ROOTSYS)
	@echo LD =$(LD)
	@echo sys =$(SYS)
	@echo arch =$(ARCH)
	@echo LOCAL = $(LOCAL)
	@echo LOCALINC = $(LOCALINC)
	@echo BOBJ = $(BOBJ)
	@echo FFTW = $(FFTW)
	@echo include path = $(INC_PATH)
	@echo CXXFLAGS = $(CXXFLAGS)
	@echo LIBS = $(LIBS)
	@echo cpp = $(CPP)
	@echo OBJS = $(OBJS)
	@echo LD SEARCH_DIR=$(SEARCH_DIR)
