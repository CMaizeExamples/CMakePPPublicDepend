cmake_minimum_required(VERSION 3.13.4)
project(CMaizePublicDepend VERSION 1.0.0 LANGUAGES CXX)

include("${PROJECT_SOURCE_DIR}/cmake/get_cpp.cmake")
include(GNUInstallDirs)

cpp_find_or_build_dependency(
    CMakePublic
    #used for testing & dev
    URL github.com/ally3999/CMakePublic
    #URL github.com/CMaizeExamples/CMakePublic
    BUILD_TARGET CMakePublic
    FIND_TARGET CMakePublic::CMakePublic
)

#debugging message
#message("CMakePublic_DIR: ${CMakePublic_DIR}")

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
target_include_directories (${PROJECT_NAME} PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-targets
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

install(EXPORT ${PROJECT_NAME}-targets
    NAMESPACE ${PROJECT_NAME}::
    FILE ${PROJECT_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

message("Processing public depend...")
