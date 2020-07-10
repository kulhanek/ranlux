# ==============================================================================
# RANLUX CMake File
# ==============================================================================

CMAKE_MINIMUM_REQUIRED(VERSION 2.5 FATAL_ERROR)
#SET(CMAKE_VERBOSE_MAKEFILE ON)
PROJECT(RANLUX)

# ==============================================================================
# conditional setup ------------------------------------------------------------
# ==============================================================================

SET(LIBS_SHARED       ON  CACHE BOOL "Should the shared version of the library be built?")
SET(LIBS_STATIC       OFF CACHE BOOL "Should the static version of the library be built?")

# ==============================================================================
# project setup ----------------------------------------------------------------
# ==============================================================================

# define project name ----------------------------------------------------------
SET(RANLUX_VERSION 1.0)

# what languages we use --------------------------------------------------------
ENABLE_LANGUAGE(Fortran)

# ==============================================================================
# output directories -----------------------------------------------------------
# ==============================================================================

SET(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)

# ==============================================================================
# compiler setups --------------------------------------------------------------
# ==============================================================================

IF(NOT DEFINED COMPOSITE_PROJECT)
    # get the name of fortran compiler
    get_filename_component(FCOMP_NAME ${CMAKE_Fortran_COMPILER} NAME_WE)

    # default compiler setups - gfortran
    SET(CMAKE_Fortran_FLAGS_RELEASE "-O0")
    SET(CMAKE_Fortran_FLAGS_DEBUG   "-g -O0")

    # ifort
    IF(FCOMP_NAME STREQUAL ifort)
        SET(CMAKE_Fortran_FLAGS_RELEASE "-O2")
        SET(CMAKE_Fortran_FLAGS_DEBUG   "-g -traceback -check all -O0")
    ENDIF(FCOMP_NAME STREQUAL ifort)

    # fix the problem with dynamic libraries and xlf compiler on SP5
    IF(FCOMP_NAME STREQUAL xlf)
        SET(CMAKE_Fortran_FLAGS_RELEASE "-O2 -qsuppress=1501-510:cmpmsg")
        SET(CMAKE_Fortran_FLAGS_DEBUG   "-g")
        IF(LIBS_SHARED)
            MESSAGE(FATAL_ERROR "LIBS_SHARED is ON, which is not supported with xlf compiler!")
        ENDIF(LIBS_SHARED)
    ENDIF(FCOMP_NAME STREQUAL xlf)

    SET(CMAKE_BUILD_TYPE Release)
    #SET(CMAKE_BUILD_TYPE Debug)
ENDIF(NOT DEFINED COMPOSITE_PROJECT)

# ==============================================================================
# project subdirectories  ------------------------------------------------------
# ==============================================================================

ADD_SUBDIRECTORY(src)
