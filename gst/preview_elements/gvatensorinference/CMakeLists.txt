# ==============================================================================
# Copyright (C) 2021 Intel Corporation
#
# SPDX-License-Identifier: MIT
# ==============================================================================

cmake_minimum_required(VERSION 3.1)

set(TARGET_NAME "gstgvatensorinference")

find_package(PkgConfig REQUIRED)
pkg_check_modules(GSTBASE gstreamer-base-1.0>=1.16 REQUIRED)
pkg_check_modules(GSTREAMER gstreamer-1.0>=1.16 REQUIRED)
pkg_check_modules(GLIB2 glib-2.0 REQUIRED)
find_IE_package()

file(GLOB MAIN_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
)

file(GLOB MAIN_HEADERS
    ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp
)

add_library(${TARGET_NAME} STATIC ${MAIN_SRC} ${MAIN_HEADERS})
set_compile_flags(${TARGET_NAME})

target_include_directories(${TARGET_NAME}
PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
PRIVATE
    ${GLIB2_INCLUDE_DIRS}
    ${GSTBASE_INCLUDE_DIRS}
    ${GSTREAMER_INCLUDE_DIRS}
)

target_link_libraries(${TARGET_NAME}
PUBLIC
    ${InferenceEngine_LIBRARIES}
PRIVATE
    gvacommon
    ${GLIB2_LIBRARIES}
    ${GSTBASE_LDFLAGS}
    ${GSTREAMER_LDFLAGS}
    utils
)
