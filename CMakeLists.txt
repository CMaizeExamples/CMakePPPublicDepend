cmake_minimum_required(VERSION 3.13.4)
project(CMaize_Public_Depend)

include("${PROJECT_SOURCE_DIR}/cmake/get_cpp.cmake")

cpp_find_or_build_dependency(
    CMakePublic
    URL github.com/ally3999/CMakePublic
    BUILD_TARGET CMakePublic
    FIND_TARGET CMakePublic::CMakePublic
)

cpp_add_library(
    CMaizePublicDepend
    SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}"
    INCLUDE_DIR "${CMAKE_CURRENT_LIST_DIR}"
    DEPENDS CMakePublic
)

message("Processing public depend...")
