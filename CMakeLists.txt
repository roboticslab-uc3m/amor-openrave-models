cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# Define a project.
project(AMOR_OPENRAVE_MODELS LANGUAGES NONE)

# Find dependencies
find_package(YCM 0.8 REQUIRED)

# https://github.com/roboticslab-uc3m/questions-and-answers/issues/65
find_package(YARP 3.0 QUIET)
if(NOT YARP_FOUND)
    find_package(YARP 2.3.70 REQUIRED)
endif()

# Set some useful variables - paths relative to the installation prefix.
yarp_configure_external_installation(amor)

# Configure install rule, copy models to the build directory.
yarp_install(DIRECTORY openrave
             DESTINATION ${AMOR_CONTEXTS_INSTALL_DIR})

# Store this package in the user registry.
export(PACKAGE AMOR_OPENRAVE_MODELS)

# Define CMAKE_INSTALL_<dir> and CMAKE_INSTALL_FULL_<dir> variables.
include(GNUInstallDirs)

# Set installation path for CMake files.
if(WIN32)
    set(AMOR_OPENRAVE_MODELS_CMAKE_DESTINATION "CMake")
else()
    set(AMOR_OPENRAVE_MODELS_CMAKE_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/AMOR_OPENRAVE_MODELS")
endif()

# Create dummy CMake configuration export file.
file(WRITE ${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfig.cmake
     "# Dummy CMake configuration export file.")

# Install CMake configuration file.
install(FILES ${CMAKE_BINARY_DIR}/AMOR_OPENRAVE_MODELSConfig.cmake
        DESTINATION ${AMOR_OPENRAVE_MODELS_CMAKE_DESTINATION})

# Add uninstall rule.
include(AddUninstallTarget)
