cmake_minimum_required(VERSION 3.5)
project(libutp LANGUAGES C)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -DPOSIX -g -fno-exceptions ${OPT}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_C_FLAGS} -fPIC -fno-rtti -Wno-sign-compare -fpermissive")

# Dynamically determine if librt is available
include(CheckLibraryExists)
check_library_exists(rt clock_gettime "" HAVE_LIBRT)
if(HAVE_LIBRT)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lrt")
endif()

# Source files for libutp
set(LIBUTP_SRC
    utp_internal.cpp
    utp_utils.cpp
    utp_hash.cpp
    utp_callbacks.cpp
    utp_api.cpp
    utp_packedsockaddr.cpp
)

# # Build libutp as a shared library
# add_library(libutp SHARED ${LIBUTP_SRC})
# set_target_properties(libutp PROPERTIES OUTPUT_NAME "utp")
# SET_SOURCE_FILES_PROPERTIES(${LIBUTP_SRC} PROPERTIES LANGUAGE C)

# Build libutp as a static library
add_library(libutp_static STATIC ${LIBUTP_SRC})
set_target_properties(libutp_static PROPERTIES OUTPUT_NAME "utp")
SET_SOURCE_FILES_PROPERTIES(${LIBUTP_SRC} PROPERTIES LANGUAGE C)

# target_link_libraries(libutp PUBLIC stdc++)
target_link_libraries(libutp_static PUBLIC stdc++)
target_include_directories(
    libutp_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)
