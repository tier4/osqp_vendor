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

ament_export_targets(osqpTargets HAS_LIBRARY_TARGET)

install(
  TARGETS osqp
  EXPORT osqpTargets
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
