cmake_minimum_required(VERSION 3.24)
project(Train VERSION 1.0 LANGUAGES CXX)
include(ExternalProject)

#--------------------------------------------------------------------#
#                          Get source files                          #

file(GLOB_RECURSE SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cc)
#--------------------------------------------------------------------#

add_executable(${PROJECT_NAME} ${SOURCE_FILES})

#--------------------------------------------------------------------#
#                        Include directories                         #

target_include_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/inc
)
#--------------------------------------------------------------------#

#--------------------------------------------------------------------#
#                           Set properties                           #

set_target_properties(${PROJECT_NAME} PROPERTIES
    # Specify directories
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin"

    # Set C++ slandered
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON

    OUTPUT_NAME ${PROJECT_NAME}
)
#--------------------------------------------------------------------#

#--------------------------------------------------------------------#
#                              Use mold                              #

if(NOT WIN32)
    target_link_options(${PROJECT_NAME} PUBLIC -fuse-ld=mold)
endif()
#--------------------------------------------------------------------#

#--------------------------------------------------------------------#
#                               Debug                                #

SET(CMAKE_BUILD_TYPE Debug)

# For memory
# SET(CMAKE_CXX_FLAGS_DEBUG "-gfull -ggdb3 -pedantic-errors -O0 -Weverything -Wno-c++98-compat -Wno-c++98-compat-pedantic -Wno-padded -Wno-documentation -Wno-documentation-unknown-command -fsanitize=memory -fno-omit-frame-pointer -fno-optimize-sibling-calls -fsanitize-memory-track-origins=2")

# For address
SET(CMAKE_CXX_FLAGS_DEBUG "-gfull -ggdb3 -Wno-newline-eof -pedantic-errors -O0 -Weverything -Wno-c++98-compat -Wno-c++98-compat-pedantic -Wno-padded -Wno-documentation -Wno-documentation-unknown-command -fsanitize=address -fno-omit-frame-pointer -fno-optimize-sibling-calls")
#-Werror Treat warnings as errors
#--------------------------------------------------------------------#

#--------------------------------------------------------------------#
#                              Release                               #

# SET(CMAKE_BUILD_TYPE Release)
SET(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
#--------------------------------------------------------------------#
