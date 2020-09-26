cmake_minimum_required(VERSION 3.13.4)

SET(CMAKE_INSTALL_PREFIX /Users/ally/CMaizeExamples/CMaizePublicDepend)
SET(CMAKE_PREFIX_PATH /Users/ally/CMaizeExamples/CMakePublic/lib)

project(CMaizePublicDepend VERSION 1.0.0 LANGUAGES CXX)

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

cpp_add_library(
    ${PROJECT_NAME}
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}"
    INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}"
    DEPENDS CMakePublic
)

target_include_directories (${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

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

message("CMAKEPUBLICDEPEND................................................................")
