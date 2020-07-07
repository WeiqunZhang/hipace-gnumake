AMREX_HOME  ?= ../../amrex

DEBUG = FALSE
DIM = 3
COMP = gcc

TINY_PROFILE = TRUE

USE_MPI   = TRUE
USE_OMP   = FALSE

USE_CUDA  = FALSE
USE_DPCPP = FALSE
USE_HIP   = FALSE

EBASE     = HiPACE

USE_RPARH  = TRUE
BL_NO_FORT = TRUE

include $(AMREX_HOME)/Tools/GNUMake/Make.defs

include $(AMREX_HOME)/Src/Base/Make.package
include $(AMREX_HOME)/Src/Boundary/Make.package
include $(AMREX_HOME)/Src/AmrCore/Make.package
include $(AMREX_HOME)/Src/Particle/Make.package

HIPACE_dirs = ../src
HIPACE_dirs +=  ../src/fields ../src/fields/fft_poisson_solver ../src/fields/fft_poisson_solver/fft
HIPACE_dirs += ../src/particles ../src/particles/deposition
HIPACE_sources = $(notdir $(foreach dir, $(HIPACE_dirs), $(wildcard $(dir)/*.cpp)))
ifeq ($(USE_CUDA),TRUE)
  HIPACE_sources := $(filter-out WrapFFTW.cpp,$(HIPACE_sources))
  libraries += -lcufft
else
  HIPACE_sources := $(filter-out WrapCuFFT.cpp,$(HIPACE_sources))
  ifeq ($(USE_MPI),TRUE)
    libraries += -lfftw3_mpi
  endif
  libraries += -lfftw3
  ifdef FFTW_HOME
    VPATH_LOCATIONS += $(FFTW_HOME)/include
    INCLUDE_LOCATIONS += $(FFTW_HOME)/include
    LIBRARY_LOCATIONS += $(FFTW_HOME)/lib
  endif
endif

CEXE_sources      += $(HIPACE_sources)
INCLUDE_LOCATIONS += $(HIPACE_dirs)
VPATH_LOCATIONS   += $(HIPACE_dirs)

include $(AMREX_HOME)/Tools/GNUMake/Make.rules

