## This will read grab the SUPPORT value from the Makefile in src/
SUPPORT = $(shell head -12 /home/obel/gferasure/src/Makefile | tail -1 | cut -d "=" -f2)


#GENERAL = secret_split.cpp challenges.cpp encryption.cpp secret_split_and_rebuild.cpp

CXX?=g++

#SRCS = $(GENERAL)
LIBLOC = /home/obel/gferasure/src/
LIBNAME = gferasure
#out=./Intel_AESNI_Sample_Library_v1.2/intel_aes_lib/bin/aes_example64

ifeq (avx2,$(findstring avx2,$(SUPPORT)))
    SRCS += $(AVX2)
    DEFINES += -DGF_AVX2 -DARCH_SSSE3 -mavx2 -mavx
endif
ifeq (ssse3,$(findstring ssse3,$(SUPPORT)))
    SRCS += $(SSSE3)
    DEFINES += -DGF_SSSE3 -mssse3 -msse4
endif
ifeq (neon,$(findstring neon,$(SUPPORT)))
    SRCS += $(NEON)
    DEFINES += -DGF_NEON -mfloat-abi=hard -mfpu=neon -Ofast -Wno-array-bounds
    DEFINES += -funroll-all-loops -fvariable-expansion-in-unroller
endif
ifeq (shift2,$(findstring shift2,$(SUPPORT)))
    SRCS += $(SHIFT2)
    DEFINES += -DGF_SHIFT2 -mssse3 -msse4
endif
ifeq (clmul,$(findstring clmul,$(SUPPORT)))
    SRCS += $(CLMUL)
    DEFINES += -DGF_CLMUL
endif
# Enable rdrand support
ifeq (rdrand,$(findstring rdrand,$(SUPPORT)))
    DEFINES += -DARCH_RDRAND_X86
endif
##-Wextra -Werror
CXX_FLAGS?=$(OPTFLAGS) -fpic -march=native -std=c++11 -Wall -g -Wno-vla $(DEFINES) 

UNAME := $(shell uname -s)
## if FreeBSD based need changes to LD and DEFINES
ifeq ($(UNAME),Darwin)
    DEFINES += -DAPPLE
    CXX_FLAGS += -stdlib=libc++ -Wno-vla-extension
else ifeq ($(UNAME),FreeBSD)
    DEFINES += -DFREEBSD
    CXX_FLAGS += -stdlib=libc++ -Wno-vla-extension
endif

#CXX_LIBS?=-lboost_system -lrados
#CXX_INC?=$(LOCAL_LIBRADOS_INC)
CXX_CC=$(CXX) $(CXX_FLAGS) $(CXX_LIBS) -L$(LIBLOC) -I$(LIBLOC) -l$(LIBNAME) -Wl,-rpath=$(LIBLOC)

all:
	$(CXX_CC)

clean:
	rm -f prog *.o

