# Copyright (c) 2017 ASMlover. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
#  * Redistributions of source code must retain the above copyright
#    notice, this list ofconditions and the following disclaimer.
#
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materialsprovided with the
#    distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required(VERSION 3.0)
set(LEARNGC_PROJECT "LearnGC")
project(${LEARNGC_PROJECT} CXX)

if (POLICY CMP0025)
  cmake_policy(SET CMP0025 NEW)
endif()
if (POLICY CMP0054)
  cmake_policy(SET CMP0054 NEW)
endif()

# set project version
set(LEARNGC_MAJOR_VERSION "0")
set(LEARNGC_MINOR_VERSION "1")
set(LEARNGC_PATCH_VERSION "0")
set(LEARNGC_VERSION_STRING "${LEARNGC_MAJOR_VERSION}.${LEARNGC_MINOR_VERSION}.${LEARNGC_PATCH_VERSION}")

# compiling with release module
if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "choose build type [Debug|Release]" FORCE)
endif()
message(STATUS "[${LEARNGC_PROJECT}] CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")

# build all binaries into specifiled directory
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# set building CMAKE_CXX_FLAGS
if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
  if (MSVC_VERSION LESS 1900)
    message(FATAL_ERROR "you need Visual Studio 2015 or later")
    return()
  endif()

  add_definitions(
    /DUNICODE
    /D_UNICODE
    /D_CRT_SECURE_NO_WARNINGS
    /D_CRT_NONSTDC_NO_WARNINGS)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /GF /GS /Gs /Zi /EHsc")
  set(CMAKE_CXX_FLAGS_DEBUG "/D_DEBUG /MDd")
  set(CMAKE_CXX_FLAGS_RELEASE "/DNDEBUG /MD /Ox")
else()
  if (CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS "4.8.1")
      message(FATAL_ERROR "you need GCC-4.8.1 or later")
      return()
    endif()
  elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS "3.3")
      message(FATAL_ERROR "you need Clang-3.3 or later")
      return()
    endif()
  endif()

  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror -std=c++1y -march=native")
  set(CMAKE_CXX_FLAGS_DEBUG "-g -ggdb -O0")
  set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
endif()
string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
message(STATUS "[${LEARNGC_PROJECT}] CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_${BUILD_TYPE}}")

include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/External/Chaos)

# building Chaos
add_subdirectory(${PROJECT_SOURCE_DIR}/External/Chaos/Chaos)

add_subdirectory(LearnGC/MarkSweep)
add_subdirectory(LearnGC/MarkSweep2)
add_subdirectory(LearnGC/TraceSweep)
add_subdirectory(LearnGC/TraceSweepMemory)
add_subdirectory(LearnGC/RefSweep)
add_subdirectory(LearnGC/RefSweepMemory)
add_subdirectory(LearnGC/DelayRef)
add_subdirectory(LearnGC/DelayRefMemory)
add_subdirectory(LearnGC/SemispaceCopy)
add_subdirectory(LearnGC/NurserySweep)
add_subdirectory(LearnGC/NurserySweepMemory)
add_subdirectory(LearnGC/SweepGC)
add_subdirectory(LearnGC/SweepGCMemory)
add_subdirectory(LearnGC/ParallelMark)
add_subdirectory(LearnGC/ParallelMarkMemory)
add_subdirectory(LearnGC/ParallelSweep)
add_subdirectory(LearnGC/ParallelSweepMemory)
add_subdirectory(LearnGC/ParallelGC)
add_subdirectory(LearnGC/ParallelGCMemory)
add_subdirectory(LearnGC/ParallelChannel)
add_subdirectory(LearnGC/ParallelChannelMemory)
add_subdirectory(LearnGC/ParallelExChannel)
add_subdirectory(LearnGC/ParallelExChannelMemory)
add_subdirectory(LearnGC/ParallelCopy)
add_subdirectory(LearnGC/ConcurrencySweep)
add_subdirectory(LearnGC/ConcurrencyCopy)
add_subdirectory(LearnGC/ConcurrencyCopyEx)
