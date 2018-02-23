cmake_minimum_required(VERSION 3.5)

project(SlicerWMA)

#-----------------------------------------------------------------------------
# Extension meta-information
set(EXTENSION_HOMEPAGE "http://github.com/SlicerDMRI/SlicerWMA")
set(EXTENSION_CATEGORY "Diffusion/Tractography")
set(EXTENSION_CONTRIBUTORS "Lauren O'Donnell (Brigham & Women's Hospital), Fan Zhang (Brigham & Women's Hospital), Isaiah Norton (Brigham & Women's Hospital)")
set(EXTENSION_DESCRIPTION "This is a Slicer interface to Lauren O'Donnell's WhiteMatterAnalysis program:

https://github.com/SlicerDMRI/whitematteranalysis")
set(EXTENSION_ICONURL "http://www.example.com/Slicer/Extensions/SlicerWMA.png")
set(EXTENSION_SCREENSHOTURLS "http://www.example.com/Slicer/Extensions/SlicerWMA/Screenshots/1.png")
set(EXTENSION_DEPENDS "NA") # Specified as a space separated string, a list or 'NA' if any
set(EXTENSION_BUILD_SUBDIRECTORY inner-build)

set(SUPERBUILD_TOPLEVEL_PROJECT inner)

#-----------------------------------------------------------------------------
# Extension dependencies
find_package(Slicer REQUIRED)
include(${Slicer_USE_FILE})

#-----------------------------------------------------------------------------
# SuperBuild setup
option(${EXTENSION_NAME}_SUPERBUILD "Build ${EXTENSION_NAME} and the projects it depends on." ON)
mark_as_advanced(${EXTENSION_NAME}_SUPERBUILD)
if(${EXTENSION_NAME}_SUPERBUILD)
  include("${CMAKE_CURRENT_SOURCE_DIR}/SuperBuild.cmake")
  return()
endif()
include(SuperBuild.cmake)

#-----------------------------------------------------------------------------
# Extension modules
add_subdirectory(WhiteMatterAnalysisCLI)


#-----------------------------------------------------------------------------
# Install extension python packages
install(CODE "message(\"CPack: - Install directory: ${python_packages_DIR}\")")
install(
    DIRECTORY "${python_packages_DIR}/"
    DESTINATION ${Slicer_INSTALL_ROOT}${Slicer_BUNDLE_EXTENSIONS_LOCATION}
    COMPONENT RuntimeLibraries
    )

#-----------------------------------------------------------------------------
include(${Slicer_EXTENSION_GENERATE_CONFIG})
include(${Slicer_EXTENSION_CPACK})
