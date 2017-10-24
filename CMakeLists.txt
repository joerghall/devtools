# MIT License
#
# Copyright (c) 2017 Joerg Hallmann
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.
#
# https://github.com/joerghall/devtools
#
cmake_minimum_required(VERSION 3.5)

include(_submodules/cbt/cmake/setup.cmake)

set(CMAKE_CXX_STANDARD 14)

if(APPLE)
    set(CMAKE_OSX_DEPLOYMENT_TARGET 10.10)
endif()

if(BUILD_WINDOWS)
    add_artifact("python2" "https://h1grid.com:443/artifactory/cbt/devtools/python/2.7.13/python-2.7.13-windows-x64.tgz" ${ARTIFACT_TOOLSETS_CACHE} "python/2.7.13/windows-x64" "python-2.7.13-windows-x64.tgz")
elseif(BUILD_OSX)
    add_artifact("python2" "https://h1grid.com:443/artifactory/cbt/devtools/python/2.7.14/python-2.7.14-osx.tgz" ${ARTIFACT_TOOLSETS_CACHE} "python/2.7.14/osx" "python-2.7.14-osx.tgz")
elseif(BUILD_LINUX)
    add_artifact("python2" "https://h1grid.com:443/artifactory/cbt/devtools/python/2.7.14/python-2.7.14-linux.tgz" ${ARTIFACT_TOOLSETS_CACHE} "python/2.7.14/linux" "python-2.7.14-linux.tgz")
endif()

discover_source_version()

project(devtools)

enable_testing()

source_map_prefix("~/sourcesfs/github/joerghall/devtools/${SOURCE_COMMIT_ID}")

add_definitions(-DSOURCE_VERSION="${SOURCE_VERSION}")

if(UNIX AND NOT APPLE)
    add_definitions(-DCONFIG_LINUX)
elseif(APPLE)
    add_definitions(-DCONFIG_OSX)
else()
    add_definitions(-DCONFIG_WINDOWS)
endif()

file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
else ()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
endif ()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CPACK_OUTPUT_FILE_PREFIX ${CMAKE_BINARY_DIR}/packages)

