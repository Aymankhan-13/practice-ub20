cmake_minimum_required (VERSION 3.20)
project(simple_cpp)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR}/install)


# FIND_PACKAGE(Subversion)
# if(SUBVERSION_FOUND)
#   Subversion_WC_INFO(${PROJECT_SOURCE_DIR} Project)
#   message("Current revision is ${Project_WC_REVISION}")
#   set(BUILD_NUMBER ${Project_WC_REVISION})
# endif()

find_package(Git)
if(Git_FOUND)
    execute_process(COMMAND ${GIT_EXECUTABLE} describe --always --abbrev=40
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE GIT_COMMIT_HASH)
    message(STATUS gch: ${GIT_COMMIT_HASH})
    #add_definitions(-DGIT_COMMIT_HASH="${GIT_COMMIT_HASH}")
    set(BUILD_NUMBER ${GIT_COMMIT_HASH})

    execute_process(COMMAND ${GIT_EXECUTABLE} rev-parse --abbrev-ref HEAD
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE GIT_COMMIT_BRANCH)
    message(STATUS gbr: ${GIT_COMMIT_BRANCH})
    #add_definitions(-DGIT_COMMIT_BRANCH="${GIT_COMMIT_BRANCH}")
endif() #git
# if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
# set(CMAKE_CXX_CLANG_TIDY
#     clang-tidy;)
# endif()

set(VERSION_MAJOR 2)
set(VERSION_MINOR 1)

set(VENDOR_NAME "Videonetics")

configure_file (
  "${PROJECT_SOURCE_DIR}/Version.h.in"
  "${PROJECT_BINARY_DIR}/Version.h"
)


add_subdirectory ("sample_cpp")
