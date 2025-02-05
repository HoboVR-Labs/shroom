cmake_minimum_required(VERSION 3.0)

project(shoom)

option(SHOOM_BUILD_TESTS "Build tests for shoom" OFF)

include_directories("include")

if (SHOOM_BUILD_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()

set (SOURCE_DIR src)

if (${CMAKE_GENERATOR} MATCHES "Visual")
set (SOURCES
    ${SOURCE_DIR}/shoom_win32.cc
)
else()
set (SOURCES
    ${SOURCE_DIR}/shoom_unix_darwin.cc
)
endif()

add_library(shoom STATIC ${SOURCES})

if (${CMAKE_GENERATOR} MATCHES "Visual")
    target_compile_options(shoom PRIVATE -W3 -EHsc)
else()
    target_compile_options(shoom PRIVATE -Wall -Wno-missing-braces -fPIC)
endif()

if (${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
    target_link_libraries(shoom -lrt) # shm_*
endif() # Linux
