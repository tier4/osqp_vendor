# taken from https://github.com/tier4/osqp_vendor
cmake_minimum_required(VERSION 3.5)
project(osqp_eigen_vendor)

find_package(ament_cmake REQUIRED)
find_package(osqp REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)

macro(build_osqp_eigen)
  set(git_tag "v0.7.0")
  set(extra_cmake_args)

  get_property(multi_config GLOBAL PROPERTY GENERATOR_IS_MULTI_CONFIG)
  if(NOT multi_config AND DEFINED CMAKE_BUILD_TYPE)
    list(APPEND extra_cmake_args -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE})
  endif()

  include(ExternalProject)
  ExternalProject_Add(osqp-eigen-${git_tag}
    GIT_REPOSITORY https://github.com/robotology/osqp-eigen
    GIT_TAG ${git_tag}
    GIT_SHALLOW ON
    TIMEOUT 60
    # Suppress git update due to https://gitlab.kitware.com/cmake/cmake/-/issues/16419
    UPDATE_COMMAND ""
    CMAKE_ARGS
      -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/osqp_eigen_install
      ${extra_cmake_args}
  )

  # The external project will install to the build folder, but we'll install that on make install.  
  install(
    DIRECTORY
      ${CMAKE_CURRENT_BINARY_DIR}/osqp_eigen_install/
    DESTINATION
      ${CMAKE_INSTALL_PREFIX}
    USE_SOURCE_PERMISSIONS
    PATTERN *.hpp
  )
endmacro()

build_osqp_eigen()

ament_export_libraries(OsqpEigen)
ament_export_dependencies(osqp eigen3_cmake_module Eigen3)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package(CONFIG_EXTRAS osqp_eigen_vendor-extras.cmake)
