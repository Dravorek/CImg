project(CImg)

#set(CMAKE_INCLUDE_CURRENT_DIR ON)
#set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON) #C: consider disabling

set(CIMG_VERSION 1.6.7)

#include(GenerateExportHeader)

add_library(CImg INTERFACE )#CImg.h)


target_include_directories(CImg INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include/CImg>
)

#generate_export_header(CImg) #C: whut? seems to be only useful for shared library headers
#set_property(TARGET CImg PROPERTY VERSION ${CIMG_VERSION}) # C: not whitelisted for INTERFACE targets
#set_property(TARGET ClimbingStats PROPERTY SOVERSION 3) #only relevant for shared libs on unix systems?
#set_property(TARGET ClimbingStats PROPERTY
#  INTERFACE_ClimbingStats_MAJOR_VERSION 3) # C: no idea what to use for this
#set_property(TARGET ClimbingStats APPEND PROPERTY
#  COMPATIBLE_INTERFACE_STRING ClimbingStats_MAJOR_VERSION
#)

install(TARGETS CImg EXPORT CImgTargets
#  LIBRARY DESTINATION lib
#  ARCHIVE DESTINATION lib
#  RUNTIME DESTINATION bin
  PUBLIC_HEADER DESTINATION include
  INCLUDES DESTINATION include
)
install(
  FILES
    CImg.h
#    "${CMAKE_CURRENT_BINARY_DIR}/climbingstats_export.h"
  DESTINATION
    include
  COMPONENT
    Devel
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/CImg/CImgConfigVersion.cmake"
  VERSION ${CIMG_VERSION}
  COMPATIBILITY AnyNewerVersion
)

export(EXPORT CImgTargets
  FILE "${CMAKE_CURRENT_BINARY_DIR}/CImg/CImgTargets.cmake"
  NAMESPACE CImg::
)
configure_file(cmake/CImgConfig.cmake
  "${CMAKE_CURRENT_BINARY_DIR}/CImg/CImgConfig.cmake"
  COPYONLY
)

set(ConfigPackageLocation lib/cmake/CImg)
install(EXPORT CImgTargets
  FILE
    CImgTargets.cmake
  NAMESPACE
    CImg::
  DESTINATION
    ${ConfigPackageLocation}
)
install(
  FILES
    cmake/CImgConfig.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/CImg/CImgConfigVersion.cmake"
  DESTINATION
    ${ConfigPackageLocation}
  COMPONENT
    Devel
)

