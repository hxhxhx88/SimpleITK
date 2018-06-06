cmake_minimum_required ( VERSION 3.0 FATAL_ERROR )

project( SimpleITK_Go )

include(../../CMake/sitkProjectLanguageCommon.cmake NO_POLICY_SCOPE)

find_package( Go REQUIRED )
include_directories( ${SimpleITK_INCLUDE_DIRS} )

set_source_files_properties( SimpleITK.i PROPERTIES CPLUSPLUS ON )

# Run swig
set(CMAKE_SWIG_FLAGS -go -cgo -c++ -intgosize 64 ${CMAKE_SWIG_GLOBAL_FLAGS})
set(CMAKE_SWIG_OUTDIR ${CMAKE_CURRENT_BINARY_DIR})
set(SWIG_MODULE_simpleitk_EXTRA_DEPS ${SWIG_EXTRA_DEPS}
  ${CMAKE_CURRENT_SOURCE_DIR}/Go.i)

SWIG_add_module( simpleitk go SimpleITK.i  )
target_link_libraries( ${SWIG_MODULE_simpleitk_TARGET_NAME} ${SimpleITK_LIBRARIES} )
set_source_files_properties( ${swig_generated_file_fullname} PROPERTIES COMPILE_FLAGS "-w")
sitk_strip_target( ${SWIG_MODULE_simpleitk_TARGET_NAME} )
