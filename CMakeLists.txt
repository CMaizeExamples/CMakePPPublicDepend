cmake_minimum_required(VERSION 3.13.4)

SET(CMAKE_INSTALL_PREFIX /Users/ally/CMaizeExamples/CMaizePublicDepend)
SET(CMAKE_PREFIX_PATH /Users/ally/CMaizeExamples/CMakePublic/lib)

project(CMaizePublicDepend VERSION 1.0.0 LANGUAGES CXX)

message("     CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")

include("${PROJECT_SOURCE_DIR}/cmake/get_cpp.cmake")
include(GNUInstallDirs)

cpp_find_or_build_dependency(
    CMakePublic
    #used for testing & dev
    URL github.com/ally3999/CMakePublic
    #used once PR accepted
    #URL github.com/CMaizeExamples/CMakePublic
    BUILD_TARGET CMakePublic
    FIND_TARGET CMakePublic::CMakePublic
)

message("     CMakePublic_DIR: ${CMakePublic_DIR}")

cpp_add_library(
    ${PROJECT_NAME}
    SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}"
    INCLUDE_DIR "${CMAKE_CURRENT_LIST_DIR}"
    DEPENDS CMakePublic
)

#do I need to modify the target_include_directories call in this file (or possibly in CMakePublic?)
#I keep getting this error:
#/Users/ally/CMaizeExamples/CMaizePublicDepend/cmake_public_depend.cpp:2:10: fatal error: 
#      'cmake_public.hpp' file not found
##include <cmake_public.hpp>

#investigate this!!!!
#target_include_directories(mylib PUBLIC
#  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/mylib>
#  $<INSTALL_INTERFACE:include/mylib>  # <prefix>/include/mylib
#)

target_include_directories (${PROJECT_NAME} PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-targets
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_BINDIR})

install(EXPORT ${PROJECT_NAME}-targets
    NAMESPACE ${PROJECT_NAME}::
    FILE ${PROJECT_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR})

message("Processing public depend...")
