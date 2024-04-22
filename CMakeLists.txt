# Copyright 2018 Harvard Medical School
# Adapted from https://github.com/Radiomics/SlicerRadiomics under
# BSD-3 License https://github.com/Radiomics/SlicerRadiomics/blob/master/LICENSE.txt

cmake_minimum_required(VERSION 3.5)

project(SlicerWMA)

#-----------------------------------------------------------------------------
# Extension meta-information
set(EXTENSION_HOMEPAGE "https://github.com/SlicerDMRI/SlicerWMA")
set(EXTENSION_CATEGORY "Libraries")
set(EXTENSION_CONTRIBUTORS "Lauren O'Donnell (Brigham & Women's Hospital), Fan Zhang (Brigham & Women's Hospital), Isaiah Norton (Brigham & Women's Hospital)")
set(EXTENSION_DESCRIPTION "SlicerWMA currently provides the WhiteMatterAnalysis (https://github.com/SlicerDMRI/whitematteranalysis) python package and dependencies installed within Slicer's Python environment.")
set(EXTENSION_ICONURL "https://raw.githubusercontent.com/SlicerDMRI/SlicerWMA/master/SlicerWMA.png")
set(EXTENSION_SCREENSHOTURLS "")
set(EXTENSION_DEPENDS "NA") # Specified as a space separated string, a list or 'NA' if any
set(EXTENSION_BUILD_SUBDIRECTORY inner-build)

set(SUPERBUILD_TOPLEVEL_PROJECT inner)

#-----------------------------------------------------------------------------
# Extension dependencies
find_package(Slicer REQUIRED)
include(${Slicer_USE_FILE})
mark_as_superbuild(Slicer_DIR)

#-----------------------------------------------------------------------------
# SuperBuild setup
option(${EXTENSION_NAME}_SUPERBUILD "Build ${EXTENSION_NAME} and the projects it depends on." ON)
mark_as_advanced(${EXTENSION_NAME}_SUPERBUILD)
if(${EXTENSION_NAME}_SUPERBUILD)
  include("${CMAKE_CURRENT_SOURCE_DIR}/SuperBuild.cmake")
  return()
endif()

#-----------------------------------------------------------------------------
# Extension modules
add_subdirectory(WMAScriptedModule)


#-----------------------------------------------------------------------------
# Install extension python packages
install(CODE "message(\"CPack: - Install directory: ${python_packages_DIR}\")")
install(
    DIRECTORY "${python_packages_DIR}/"
    DESTINATION ${Slicer_INSTALL_ROOT}${Slicer_BUNDLE_EXTENSIONS_LOCATION}
    COMPONENT RuntimeLibraries
    )

#-----------------------------------------------------------------------------
set(CPACK_INSTALL_CMAKE_PROJECTS "${CPACK_INSTALL_CMAKE_PROJECTS};${CMAKE_BINARY_DIR};${EXTENSION_NAME};ALL;/")
include(${Slicer_EXTENSION_GENERATE_CONFIG})
include(${Slicer_EXTENSION_CPACK})
