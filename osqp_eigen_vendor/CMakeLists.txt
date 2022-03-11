cmake_minimum_required(VERSION 3.14)

project(osqp_eigen_vendor)

find_package(ament_cmake REQUIRED)
find_package(osqp_vendor REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)

set(git_tag "master")

include(FetchContent)
FetchContent_Declare(
  OsqpEigen
  GIT_REPOSITORY https://github.com/mhubii/osqp-eigen
  GIT_TAG ${git_tag}
)

FetchContent_MakeAvailable(OsqpEigen)

ament_export_targets(OsqpEigenTargets HAS_LIBRARY_TARGET)
ament_export_dependencies(osqp_vendor eigen3_cmake_module Eigen3)

# multiple uninstall targets: https://github.com/actor-framework/actor-framework/issues/866
# fix in yarp: https://github.com/robotology/ycm/pull/237
install(
  TARGETS OsqpEigen
  EXPORT OsqpEigenTargets
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
