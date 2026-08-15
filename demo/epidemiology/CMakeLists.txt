# -----------------------------------------------------------------------------
#
# Copyright (C) Lukas Breitwieser.
# All Rights Reserved.
#
# ----------------------------------------------------------------------------- 

cmake_minimum_required(VERSION 3.19.3)

project(epidemiology)

find_package(BioDynaMo REQUIRED)
include(${BDM_USE_FILE})
include_directories("src")

find_package(GSL REQUIRED)
include_directories(${GSL_INCLUDE_DIR})

# -----------------------------------------------------------------------------
# BarrierPoint instrumentation (barrierpoint)
#
# Off by default. Enable with:
#   cmake -DBARRIERPOINT_INSTRUMENTATION=ON ..
# Requires SNIPER_ROOT to be set in the environment, since sim_api.h is
# provided by Sniper (see trevorcarlson/barrierpoint)
# -----------------------------------------------------------------------------
option(BARRIERPOINT_INSTRUMENTATION "Enable SimRoiStart/SimRoiEnd markers for BarrierPoint sampling" OFF)

if(BARRIERPOINT_INSTRUMENTATION)
  if(NOT DEFINED ENV{SNIPER_ROOT})
    message(FATAL_ERROR "BARRIERPOINT_INSTRUMENTATION is ON but SNIPER_ROOT is not set in the environment. sim_api.h cannot be located.")
  endif()
  message(STATUS "BarrierPoint instrumentation ENABLED. Using sim_api.h from: $ENV{SNIPER_ROOT}/include")
  include_directories("$ENV{SNIPER_ROOT}/include")
  add_definitions(-DBARRIERPOINT_INSTRUMENTATION)
endif()

file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE SOURCES src/*.cc)

bdm_add_executable(epidemiology
                   HEADERS ${HEADERS}
                   SOURCES ${SOURCES}
                   LIBRARIES ${BDM_REQUIRED_LIBRARIES} GSL::gsl GSL::gslcblas optim)

