cmake_minimum_required(VERSION 3.14)

project(osqp_eigen_vendor)

find_package(ament_cmake REQUIRED)
find_package(osqp_vendor REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)

set(git_tag "header-fault")

include(FetchContent)
FetchContent_Declare(
  OsqpEigen
  GIT_REPOSITORY https://github.com/mhubii/osqp-eigen
  GIT_TAG ${git_tag}
)

FetchContent_MakeAvailable(OsqpEigen)

ament_export_targets(OsqpEigenVendorTargets HAS_LIBRARY_TARGET)
ament_export_dependencies(osqp_vendor eigen3_cmake_module Eigen3 OsqpEigen)

add_library(osqp_eigen_vendor INTERFACE)
target_link_libraries(osqp_eigen_vendor INTERFACE OsqpEigen)

install(
  TARGETS osqp_eigen_vendor 
  EXPORT OsqpEigenVendorTargets
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
