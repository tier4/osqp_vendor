cmake_minimum_required(VERSION 3.14)

project(osqp_vendor)

find_package(ament_cmake REQUIRED)

set(git_tag "v0.6.2")

include(FetchContent)
FetchContent_Declare(
  osqp
  GIT_REPOSITORY https://github.com/osqp/osqp
  GIT_TAG ${git_tag}
)

FetchContent_MakeAvailable(osqp)

ament_export_targets(osqpVendorTargets HAS_LIBRARY_TARGET)
ament_export_dependencies(osqp)

add_library(osqp_vendor INTERFACE)
target_link_libraries(osqp_vendor INTERFACE osqp::osqp)

install(
  TARGETS osqp_vendor
  EXPORT osqpVendorTargets
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
