cmake_minimum_required(VERSION 3.20)
cmake_policy(SET CMP0074 NEW)

set(CMAKE_CXX_STANDARD 17)

project(simple_vcpkg_library)

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    if(DEFINED ENV{CMAKE_INSTALL_PREFIX})
		set(CMAKE_INSTALL_PREFIX "$ENV{CMAKE_INSTALL_PREFIX}/${PROJECT_NAME}" 
			CACHE PATH "Force path to set CMAKE_INSTALL_PREFIX" FORCE)
	endif()
endif(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

#########################################################################################

option(BUILD_SHARED_LIBS "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)" ON)

SET (LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin CACHE INTERNAL "Single output directory for building all libraries.")
SET (EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin CACHE INTERNAL "Single output directory for building all executables.")

include(GNUInstallDirs)

#########################################################################################
 
add_subdirectory(LibB)

#########################################################################################

include(CMakePackageConfigHelpers)

# Export all targets
install(EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE LibEX::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

# Generate and install the Config file
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/CMake/Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

#########################################################################################