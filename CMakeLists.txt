cmake_minimum_required(VERSION 2.8.9)

# Define a project.
project(AMOR_OPENRAVE_MODELS)

# Find YARP and load configuration, fail otherwise.
find_package(YARP REQUIRED)

# Make YARP's modules visible to current CMake file.
list(APPEND CMAKE_MODULE_PATH ${YARP_MODULE_PATH})

# Load yarp_configure_external_installation() and yarp_install().
include(YarpInstallationHelpers)

# Set some useful variables - paths relative to the installation prefix.
yarp_configure_external_installation(amor)

set(models_dir openrave)

# Configure install rule, copy models to the build directory.
yarp_install(DIRECTORY openrave DESTINATION ${AMOR_CONTEXTS_INSTALL_DIR})

# Store this package in the user registry.
export(PACKAGE AMOR_OPENRAVE_MODELS)

# Define CMAKE_INSTALL_<dir> and CMAKE_INSTALL_FULL_<dir> variables.
include(GNUInstallDirs)

if(WIN32)
    set(AMOR_MODELS_CMAKE_DESTINATION "CMake")
else()
    set(AMOR_MODELS_CMAKE_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/AMOR_OPENRAVE_MODELS")
endif()

# Create dummy CMake configuration export file.
file(WRITE ${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfig.cmake "# Dummy CMake configuration export file.")

# Install CMake configuration file.
install(FILES ${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfig.cmake
        DESTINATION ${PROJECT_NAME}_CMAKE_DESTINATION)

# Configure CMake uninstall script.
configure_file("${CMAKE_SOURCE_DIR}/cmake/templates/AMOR_OPENRAVE_MODELSConfigUninstall.cmake.in"
               "${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfigUninstall.cmake" @ONLY)

# Add uninstall rule.
add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P
                  "${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfigUninstall.cmake")
