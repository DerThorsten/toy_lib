cmake_minimum_required(VERSION 3.9)
project(toy_lib VERSION 1.0.1 DESCRIPTION "toy_lib description")
include(GNUInstallDirs)
add_library(toy_lib SHARED src/toy_lib.cpp)
set_target_properties(toy_lib PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
    PUBLIC_HEADER include/toy_lib/toy_lib.hpp
    OUTPUT_NAME "libtoy_lib"
)

if (APPLE)
    set_target_properties(toy_lib PROPERTIES
        MACOSX_RPATH ON
    )
else ()
    set_target_properties(toy_lib PROPERTIES
        BUILD_WITH_INSTALL_RPATH 1
        SKIP_BUILD_RPATH FALSE
    )
endif ()

set_target_properties(toy_lib PROPERTIES
    INSTALL_RPATH_USE_LINK_PATH TRUE)


configure_file(toy_lib.pc.in toy_lib.pc @ONLY)

set(TOY_LIB_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_include_directories(toy_lib
                           PUBLIC
                           $<BUILD_INTERFACE:${TOY_LIB_INCLUDE_DIR}>
                           $<INSTALL_INTERFACE:include>)


install(TARGETS toy_lib
    EXPORT ${PROJECT_NAME}-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/toy_lib)
install(FILES ${CMAKE_BINARY_DIR}/toy_lib.pc
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)


include(CMakePackageConfigHelpers)

set(TOY_LIB_CMAKECONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}" CACHE STRING "install path for toy_libConfig.cmake")



# Configure 'toy_libConfig.cmake.in for an install tree
set(TOY_LIB_CONFIG_CODE "")
configure_package_config_file(${PROJECT_NAME}Config.cmake.in
                              "${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/${PROJECT_NAME}Config.cmake"
                              INSTALL_DESTINATION ${TOY_LIB_CMAKECONFIG_INSTALL_DIR})

write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
                                 VERSION ${${PROJECT_NAME}_VERSION}
                                 COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/${PROJECT_NAME}Config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
              DESTINATION ${TOY_LIB_CMAKECONFIG_INSTALL_DIR})

if (true)#TOY_LIB_BUILD_SHARED)
    install(EXPORT ${PROJECT_NAME}-targets
            FILE ${PROJECT_NAME}Targets.cmake
            DESTINATION ${TOY_LIB_CMAKECONFIG_INSTALL_DIR})
endif ()
