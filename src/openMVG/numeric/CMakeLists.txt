file(
  GLOB_RECURSE
  numeric_files_header
  *.hpp
)
file(
  GLOB_RECURSE
  numeric_files_cpp
  *.cpp
)

file(GLOB_RECURSE REMOVEFILESUNITTEST *_test.cpp)
list(REMOVE_ITEM numeric_files_cpp ${REMOVEFILESUNITTEST})

add_library(openMVG_numeric
  ${numeric_files_header}
  ${numeric_files_cpp})
target_include_directories(openMVG_numeric PUBLIC
  $<BUILD_INTERFACE:${EIGEN_INCLUDE_DIRS}>
  $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
)
if (DEFINED OpenMVG_USE_INTERNAL_EIGEN)
  target_include_directories(openMVG_numeric PUBLIC
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/eigen/>)
else()
  target_include_directories(openMVG_numeric PUBLIC
    $<INSTALL_INTERFACE:${EIGEN_INCLUDE_DIRS}>)
endif()

set_target_properties(openMVG_numeric PROPERTIES SOVERSION ${OPENMVG_VERSION_MAJOR} VERSION "${OPENMVG_VERSION_MAJOR}.${OPENMVG_VERSION_MINOR}")
set_property(TARGET openMVG_numeric PROPERTY FOLDER OpenMVG/OpenMVG)
install(TARGETS openMVG_numeric DESTINATION lib EXPORT openMVG-targets)

UNIT_TEST(openMVG extract_columns openMVG_numeric)
UNIT_TEST(openMVG numeric openMVG_numeric)
UNIT_TEST(openMVG poly openMVG_numeric)
UNIT_TEST(openMVG lm openMVG_numeric)
UNIT_TEST(openMVG l1_solver openMVG_numeric)
